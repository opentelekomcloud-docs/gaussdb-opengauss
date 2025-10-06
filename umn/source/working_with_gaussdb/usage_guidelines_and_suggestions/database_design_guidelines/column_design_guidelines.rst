:original_name: opengauss_tips_0012.html

.. _opengauss_tips_0012:

Column Design Guidelines
========================

-  Use recommended data types for column design.

   Recommended data types must be used for column design. If you want to use prohibited or not recommended data types, contact technical support for evaluation.

   Some data types are not recommended or are prohibited because they apply to limited service scenarios and are not used on a large scale for commercial purposes.

   If there are urgent field type requirements, contact technical support and submit the requirements.

   .. table:: **Table 1** Database data types

      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Data Type                 | Description                                                                                  | Recommended or Not                                                                                                       |
      +===========================+==============================================================================================+==========================================================================================================================+
      | UUID                      | Different instances may generate the same UUID.                                              | No (Prohibited). It is recommended that the service directly use the distributed ID provided by the middleware platform. |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Sequence integer          | Auto-increment column, including SMALLSERIAL, SERIAL, and BIGSERIAL                          | No (Prohibited)                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Integer                   | TINYINT, SMALLINT, INTEGER, BIGINT                                                           | Yes                                                                                                                      |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Any precision             | NUMERIC/DECIMAL                                                                              | Yes                                                                                                                      |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Floating point            | REAL/FLOAT4,DOUBLE PRECISION/FLOAT8,FLOAT                                                    | Yes                                                                                                                      |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Boolean                   | BOOLEAN                                                                                      | Yes                                                                                                                      |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Fixed-length character    | CHAR(n)                                                                                      | Yes                                                                                                                      |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Variable-length character | VARCHAR(n),NVARCHAR2(n)                                                                      | Yes                                                                                                                      |
      |                           |                                                                                              |                                                                                                                          |
      |                           | VARCHAR/TEXT                                                                                 |                                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Time                      | DATE, TIME, TIMESTAMP, SMALLDATETIME, INTERVAL, REALTIME                                     | Yes                                                                                                                      |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      |                           | TIMETZ, TIMESTAMPTZ                                                                          | No                                                                                                                       |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Binary                    | BYTEA (variable-length binary)                                                               | Yes                                                                                                                      |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      |                           | CLOB (character large object), BLOB (binary large object), RAW (variable-length hexadecimal) | No (Prohibited)                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Bit string                | BIT(n), VARBIT(n)                                                                            | Yes                                                                                                                      |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Special character         | NAME and "CHAR" are usually used within the database system.                                 | No (Prohibited)                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | JSON                      | JSON data does not support operators.                                                        | No (Prohibited)                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | User-defined              | Defines enumerated types.                                                                    | No (Prohibited)                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | HLL                       | You are advised to use the HLL functions to reduce the impact on performance.                | No (Prohibited)                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Money                     | Stores a currency amount with fixed fractional precision.                                    | No (Prohibited)                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Geometric                 | POINT, LSEG, BOX, PATH, POLYGON, CIRCLE                                                      | No (Prohibited)                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Network address           | Stores IPv4, IPv6, and MAC addresses.                                                        | No (Prohibited)                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Text search               | Supports full text search.                                                                   | No (Prohibited)                                                                                                          |
      +---------------------------+----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+

-  Select a proper string type. The variable-length character class VARCHAR is preferred. CHAR(*n*) is used only when this field is a fixed-length character or spaces need to be automatically added.

   .. note::

      For a typical fixed-length field, for example, **sex**, you can enter only **f** or **m** that occupies a byte. You are advised to use the fixed-length data type (for example, CHAR(*n*)) for this type of fields.

      If such requirement does not exist or longer characters may be required for future expansion, use variable-length character types (such as VARCHAR and TEXT) preferentially. You are not advised to specify the length of variable-length characters.

      The reasons are as follows:

      -  For fixed-length fields, the input data that is shorter than the fixed length will be padded with space characters and then be saved to the database. This wastes the storage space in the database.
      -  For fixed-length character types, the entire table needs to be scanned and rewritten if the length needs to be extended later. This causes high performance overheads and affects online services.

      For a variable-length field with a fixed length, the system checks whether the length exceeds the limit each time upon data insertion. This causes performance overheads.

-  Do not store data of the numeric type in the fields of character type.

   If numeric calculation or comparison (for example, adding a filter condition) is performed on data stored in columns of the character type, unnecessary overhead will be caused due to data type conversion, and the column indexes may become invalid, affecting query performance.

-  Do not store data of the time or date type in fields of the character type.

   If calculation or comparison (for example, adding a filter condition) with data of the time or date type is performed on data stored in columns of the character type, unnecessary overhead will be caused by data type conversion, and the column indexes may become invalid, affecting query performance.

-  Add NOT NULL constraints to fields that never have NULL values.

   In certain cases, the optimizer may specially optimize NOT NULL field to greatly improve query performance.

-  Use the same data type for associated fields.

   If the field types are inconsistent during a join operation, overheads are caused by data type conversion.

-  The number of long fields, such as VARCHAR(1000) and VARCHAR(4000), cannot exceed 8.

-  When defining a field, you are advised to create a comment for the field for subsequent maintenance.

-  Add NOT NULL constraints to the fields that are used for WHERE filtering and join operations.

   In certain cases, the optimizer may specially optimize NOT NULL field to greatly improve query performance.

-  You are not advised to reserve fields for tables. In most cases, you can quickly add or delete table fields, or change the default values of fields.

   .. note::

      An added column must meet the following requirements. Otherwise, the entire table is updated, leading to additional overheads and affecting online services.

      -  The data type is BOOLEAN, BYTEA, SMALLINT, BIGINT, SMALLINT, INTEGER, NUMERIC, FLOAT, DOUBLE PRECISION, CHAR, VARCHAR, TEXT, TIMESTAMPTZ, TIMESTAMP, DATE, TIME, TIMETZ, or INTERVAL.
      -  The length of the default value cannot exceed 128 bytes.
      -  The default value does not contain the volatile function.
      -  The default value is required and cannot be NULL.

      If you are not sure whether the condition is met, contact database technical support for evaluation.

-  Use the most specific numeric data types. If all of the following numeric types provide the required service precision, they are recommended in descending order of priority: integer, floating point, and NUMERIC.

-  Properly specify the data type of a numeric field based on the value range, and use the NUMERIC or DECIMAL type as less as possible.

   NUMERIC and DECIMAL are equivalent. NUMERIC or DECIMAL data operations consume great CPU resources.

   .. table:: **Table 2** Storage space and value range of numeric data types

      +-------------------------+-----------------+----------------------------+---------------------------+
      | Type                    | Storage (Bytes) | Minimum Value              | Maximum Value             |
      +=========================+=================+============================+===========================+
      | TINYINT                 | 1               | 0                          | 255                       |
      +-------------------------+-----------------+----------------------------+---------------------------+
      | SMALLINT                | 2               | -32768                     | 32767                     |
      +-------------------------+-----------------+----------------------------+---------------------------+
      | INTEGER                 | 4               | -2,147,483,648             | 2,147,483,647             |
      +-------------------------+-----------------+----------------------------+---------------------------+
      | BIGINT                  | 8               | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 |
      +-------------------------+-----------------+----------------------------+---------------------------+
      | REAL/FLOAT4             | 4               | 6-bit decimal digits       |                           |
      +-------------------------+-----------------+----------------------------+---------------------------+
      | DOUBLE PRECISION/FLOAT8 | 8               | 15-bit decimal digits      |                           |
      +-------------------------+-----------------+----------------------------+---------------------------+
