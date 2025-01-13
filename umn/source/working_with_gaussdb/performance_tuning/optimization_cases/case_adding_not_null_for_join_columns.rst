:original_name: opengauss_opti_0068.html

.. _opengauss_opti_0068:

Case: Adding NOT NULL for JOIN Columns
======================================

Symptom
-------

::

   SELECT
    *
   FROM
   ( ( SELECT
     STARTTIME STTIME,
     SUM(NVL(PAGE_DELAY_MSEL,0)) PAGE_DELAY_MSEL,
     SUM(NVL(PAGE_SUCCEED_TIMES,0)) PAGE_SUCCEED_TIMES,
     SUM(NVL(FST_PAGE_REQ_NUM,0)) FST_PAGE_REQ_NUM,
     SUM(NVL(PAGE_AVG_SIZE,0)) PAGE_AVG_SIZE,
     SUM(NVL(FST_PAGE_ACK_NUM,0)) FST_PAGE_ACK_NUM,
     SUM(NVL(DATATRANS_DW_DURATION,0)) DATATRANS_DW_DURATION,
     SUM(NVL(PAGE_SR_DELAY_MSEL,0)) PAGE_SR_DELAY_MSEL
    FROM
     PS.SDR_WEB_BSCRNC_1DAY SDR
     INNER JOIN (SELECT
         BSCRNC_ID,
         BSCRNC_NAME,
         ACCESS_TYPE,
         ACCESS_TYPE_ID
        FROM
         nethouse.DIM_LOC_BSCRNC
        GROUP BY
         BSCRNC_ID,
         BSCRNC_NAME,
         ACCESS_TYPE,
         ACCESS_TYPE_ID) DIM
     ON SDR.BSCRNC_ID = DIM.BSCRNC_ID
     AND DIM.ACCESS_TYPE_ID IN (0,1,2)
     INNER JOIN nethouse.DIM_RAT_MAPPING RAT
     ON (RAT.RAT = SDR.RAT)
    WHERE
     ( (STARTTIME >= 1461340800
     AND STARTTIME < 1461427200) )
     AND RAT.ACCESS_TYPE_ID IN (0,1,2)
     --AND SDR.BSCRNC_ID IS NOT NULL
    GROUP BY
     STTIME ) ) ;

:ref:`Figure 1 <en-us_topic_0000002124277341__en-us_topic_0073253827_en-us_topic_0040046525_fig6426916816285>` shows the execution plan.

.. _en-us_topic_0000002124277341__en-us_topic_0073253827_en-us_topic_0040046525_fig6426916816285:

.. figure:: /_static/images/en-us_image_0000002124277833.jpg
   :alt: **Figure 1** Adding NOT NULL for JOIN columns (1)

   **Figure 1** Adding NOT NULL for JOIN columns (1)

Optimization Analysis
---------------------

#. As shown in :ref:`Figure 1 <en-us_topic_0000002124277341__en-us_topic_0073253827_en-us_topic_0040046525_fig6426916816285>`, the sequential scan phase is time consuming.

#. The JOIN performance is poor because a large number of null values exist in the JOIN column **BSCRNC_ID** of the **PS.SDR_WEB_BSCRNC_1DAY** table.

   Therefore, you are advised to manually add **NOT NULL** for **JOIN** columns in the statement, as shown below:

   ::

      SELECT
       *
      FROM
      ( ( SELECT
        STARTTIME STTIME,
        SUM(NVL(PAGE_DELAY_MSEL,0)) PAGE_DELAY_MSEL,
        SUM(NVL(PAGE_SUCCEED_TIMES,0)) PAGE_SUCCEED_TIMES,
        SUM(NVL(FST_PAGE_REQ_NUM,0)) FST_PAGE_REQ_NUM,
        SUM(NVL(PAGE_AVG_SIZE,0)) PAGE_AVG_SIZE,
        SUM(NVL(FST_PAGE_ACK_NUM,0)) FST_PAGE_ACK_NUM,
        SUM(NVL(DATATRANS_DW_DURATION,0)) DATATRANS_DW_DURATION,
        SUM(NVL(PAGE_SR_DELAY_MSEL,0)) PAGE_SR_DELAY_MSEL
       FROM
        PS.SDR_WEB_BSCRNC_1DAY SDR
        INNER JOIN (SELECT
            BSCRNC_ID,
            BSCRNC_NAME,
            ACCESS_TYPE,
            ACCESS_TYPE_ID
           FROM
            nethouse.DIM_LOC_BSCRNC
           GROUP BY
            BSCRNC_ID,
            BSCRNC_NAME,
            ACCESS_TYPE,
            ACCESS_TYPE_ID) DIM
        ON SDR.BSCRNC_ID = DIM.BSCRNC_ID
        AND DIM.ACCESS_TYPE_ID IN (0,1,2)
        INNER JOIN nethouse.DIM_RAT_MAPPING RAT
        ON (RAT.RAT = SDR.RAT)
       WHERE
        ( (STARTTIME >= 1461340800
        AND STARTTIME < 1461427200) )
        AND RAT.ACCESS_TYPE_ID IN (0,1,2)
        AND SDR.BSCRNC_ID IS NOT NULL
       GROUP BY
        STTIME ) ) A;

   :ref:`Figure 2 <en-us_topic_0000002124277341__en-us_topic_0073253827_en-us_topic_0040046525_fig1063411317326>` shows the execution plan.

   .. _en-us_topic_0000002124277341__en-us_topic_0073253827_en-us_topic_0040046525_fig1063411317326:

   .. figure:: /_static/images/en-us_image_0000002088678098.jpg
      :alt: **Figure 2** Adding NOT NULL for JOIN columns (2)

      **Figure 2** Adding NOT NULL for JOIN columns (2)
