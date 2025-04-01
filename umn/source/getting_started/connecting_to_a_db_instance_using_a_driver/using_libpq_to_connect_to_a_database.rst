:original_name: opengauss_libpg_connect.html

.. _opengauss_libpg_connect:

Using libpq to Connect to a Database
====================================

libpq is a C application programming interface to GaussDB. libpq contains a set of library functions that allow client programs to send query requests to GaussDB servers and obtain query results. It is also the underlying engine of other GaussDB application interfaces, such as ODBC. This section provides two examples to show how to write code using libpq.

Obtaining the Driver Package
----------------------------

`Download <https://dbs-download.obs.otc.t-systems.com/rds/GaussDB_opengauss_client_tools.zip>`__ the GaussDB driver package **GaussDB_opengauss_client_tools.zip**.

Prerequisites
-------------

A C development environment has been installed on the local PC.

To compile and link a libpq source program, perform the following operations:

-  Decompress the **GaussDB-Kernel-V**\ *xxx*\ **R**\ *xxx*\ **C**\ *xx*\ **-EULER-64bit-Libpq.tar.gz** file. The required header file is stored in the **include** folder, and the **lib** folder contains the required libpq library file.

   .. note::

      In addition to **libpq-fe.h**, the **include** folder contains the header files **postgres_ext.h**, **gs_thread.h**, and **gs_threadlocal.h** by default. These three header files are the dependent files of **libpq-fe.h**.

-  Include the **libpq-fe.h** header file.

   .. code-block::

      #include <libpq-fe.h>

-  Use the **-I** *directory* option to provide the installation location of the header file. (Sometimes the compiler looks for the default directory. Therefore, this option can be ignored.) For example:

   .. code-block::

      gcc -I (Directory of the header file) -L (Directory of the libpq library) testprog.c -lpq

-  To use makefile, add the following options to the CPPFLAGS, LDFLAGS, and LIBS variables:

   .. code-block::

      CPPFLAGS += -I (directory of the header file)
      LDFLAGS += -L (directory of the libpq library)
      LIBS += -lpq

Code for Common Functions
-------------------------

Examples

Example 1:

::

   /*
    * testlibpq.c
    */
   #include <stdio.h>
   #include <stdlib.h>
   #include <libpq-fe.h>

   static void
   exit_nicely(PGconn *conn)
   {
       PQfinish(conn);
       exit(1);
   }

   int
   main(int argc, char **argv)
   {
       const char *conninfo;
       PGconn     *conn;
       PGresult   *res;
       int         nFields;
       int         i,j;

       /*
         * If the user provides the values of conninfo character strings on the command line, use them.
         * Otherwise, environment variables or default values of other connection parameters
         * are used.
        */
       if (argc > 1)
           conninfo = argv[1];
       else
           conninfo = "dbname=postgres port=42121 host='10.44.133.171' application_name=test connect_timeout=5 sslmode=allow user='test' password='xxxx'";

       /* Connect to a database. */
       conn = PQconnectdb(conninfo);

       /* Check whether the backend connection is successfully established. */
       if (PQstatus(conn) != CONNECTION_OK)
       {
           fprintf(stderr, "Connection to database failed: %s",
                   PQerrorMessage(conn));
           exit_nicely(conn);
       }

       /*
        * Transaction blocks must be used when cursors are used in a test instance.
        * Place all data in one "select * from pg_database".
        * PQexec() is simple and is not recommended.
        */

       /* Start a transaction block. */
       res = PQexec(conn, "BEGIN");
       if (PQresultStatus(res) != PGRES_COMMAND_OK)
       {
           fprintf(stderr, "BEGIN command failed: %s", PQerrorMessage(conn));
           PQclear(res);
           exit_nicely(conn);
       }

       /*
        * PQclear PGresult should be executed when it is no longer needed, to avoid memory leakage.
        */
       PQclear(res);

       /*
        * Obtain data from the pg_database system catalog.
        */
       res = PQexec(conn, "DECLARE myportal CURSOR FOR select * from pg_database");
       if (PQresultStatus(res) != PGRES_COMMAND_OK)
       {
           fprintf(stderr, "DECLARE CURSOR failed: %s", PQerrorMessage(conn));
           PQclear(res);
           exit_nicely(conn);
       }
       PQclear(res);

       res = PQexec(conn, "FETCH ALL in myportal");
       if (PQresultStatus(res) != PGRES_TUPLES_OK)
       {
           fprintf(stderr, "FETCH ALL failed: %s", PQerrorMessage(conn));
           PQclear(res);
           exit_nicely(conn);
       }

        /* Print the attribute name. */
       nFields = PQnfields(res);
       for (i = 0; i < nFields; i++)
           printf("%-15s", PQfname(res, i));
       printf("\n\n");

       /* Print rows. */
       for (i = 0; i < PQntuples(res); i++)
       {
           for (j = 0; j < nFields; j++)
               printf("%-15s", PQgetvalue(res, i, j));
           printf("\n");
       }

       PQclear(res);

       /* Close the console... Do not check for errors.... */
       res = PQexec(conn, "CLOSE myportal");
       PQclear(res);

       //* End the transaction. */
       res = PQexec(conn, "END");
       PQclear(res);

       /* Close the database connection and free the memory used by the object. */
       PQfinish(conn);

       return 0;
   }

Example 2:

::

   /*
    * testlibpq2.c
     *     Test out-of-line parameters and binary I/Os.
    *
    * Before running this example, run the following command to populate a database:
    *
    *
    * CREATE TABLE test1 (i int4, t text);
    *
     * INSERT INTO test1 values (2, 'ho there');
    *
    * Expected output:
    *
    *
    * tuple 0: got
    *  i = (4 bytes) 2
    *  t = (8 bytes) 'ho there'
    *
    */
   #include <stdio.h>
   #include <stdlib.h>
   #include <string.h>
   #include <sys/types.h>
   #include <libpq-fe.h>

   /* for ntohl/htonl */
   #include <netinet/in.h>
   #include <arpa/inet.h>

   static void
   exit_nicely(PGconn *conn)
   {
       PQfinish(conn);
       exit(1);
   }

   /*
    * This function prints the query results in binary format, which are
    * fetched from the table created in the comment above.
    */
   static void
   show_binary_results(PGresult *res)
   {
       int         i;
       int         i_fnum,
                   t_fnum;

       /* Use PQfnumber to avoid assumptions about field order in the result. */
       i_fnum = PQfnumber(res, "i");
       t_fnum = PQfnumber(res, "t");

       for (i = 0; i < PQntuples(res); i++)
       {
           char       *iptr;
           char       *tptr;
           int         ival;

           /* Obtain the field value. (The possibility that the field may be null is ignored). */
           iptr = PQgetvalue(res, i, i_fnum);
           tptr = PQgetvalue(res, i, t_fnum);

           /*
            * The binary representation of INT4 is network byte order,
             * which is better to be replaced with the local byte order.
            */
           ival = ntohl(*((uint32_t *) iptr));

           /*
            * The binary representation of TEXT is text. libpq can append a zero byte to it
             * and think of it as a C string.
            *
            */

           printf("tuple %d: got\n", i);
           printf(" i = (%d bytes) %d\n",
                  PQgetlength(res, i, i_fnum), ival);
           printf(" t = (%d bytes) '%s'\n",
                  PQgetlength(res, i, t_fnum), tptr);
           printf("\n\n");
       }
   }

   int
   main(int argc, char **argv)
   {
       const char *conninfo;
       PGconn     *conn;
       PGresult   *res;
       const char *paramValues[1];
       int         paramLengths[1];
       int         paramFormats[1];
       uint32_t    binaryIntVal;

       /*
        * If the user provides a parameter on the command line,
        * the value is a conninfo character string. Otherwise,
        * an environment variable or a default value is used.
        */
       if (argc > 1)
           conninfo = argv[1];
       else
           conninfo = "dbname=postgres port=42121 host='10.44.133.171' application_name=test connect_timeout=5 sslmode=allow user='test' password='xxxx'";

       /* Connect to the database. */
       conn = PQconnectdb(conninfo);

       /* Check whether the connection to the server is successfully established. */
       if (PQstatus(conn) != CONNECTION_OK)
       {
           fprintf(stderr, "Connection to database failed: %s",
                   PQerrorMessage(conn));
           exit_nicely(conn);
       }

       /* Convert the integer value "2" into the network byte order. */
       binaryIntVal = htonl((uint32_t) 2);

       /* Set the parameter array for PQexecParams. */
       paramValues[0] = (char *) &binaryIntVal;
       paramLengths[0] = sizeof(binaryIntVal);
       paramFormats[0] = 1;        /* Binary */

       res = PQexecParams(conn,
                          "SELECT * FROM test1 WHERE i = $1::int4",
                          1,       /* One parameter */
                          NULL,    /* Enable the backend to deduce the parameter type. */
                          paramValues,
                          paramLengths,
                          paramFormats,
                          1);      /* Require binary results. */

       if (PQresultStatus(res) != PGRES_TUPLES_OK)
       {
           fprintf(stderr, "SELECT failed: %s", PQerrorMessage(conn));
           PQclear(res);
           exit_nicely(conn);
       }

       show_binary_results(res);

       PQclear(res);

       /* Close the database connection and free the memory used by the object. */
       PQfinish(conn);

       return 0;
   }
