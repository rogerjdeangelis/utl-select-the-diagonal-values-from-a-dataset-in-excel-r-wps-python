# utl-select-the-diagonal-values-from-a-dataset-in-excel-r-wps-python
Select the diagonal values from a dataset in excel r wps python 
    %let pgm=utl-select-the-diagonal-values-from-a-dataset-in-excel-r-wps-python;

    Select the diagonal values from a dataset in excel r wps python

    github
    https://tinyurl.com/2s3yhz5u
    https://github.com/rogerjdeangelis/utl-select-the-diagonal-values-from-a-dataset-in-excel-r-wps-python

    stackoverflow
    https://stackoverflow.com/questions/77149898/condition-on-multiple-columns-in-r

    No neeed for the second dataset

     TWO INPUTS

      I  wps sas dataset ( for solutions 1-5 )
      II excel workbook  ( for solution 6 )

     SOLUTIONS

      1 wps datastep
      2 wps sql
      3 wps native r
      4 wps  r sql
      5 wps python sql
      6 wps excel add named range(table) in place

    /*__   _                   _                              _       _                 _
    |_ _| (_)_ __  _ __  _   _| |_  __      ___ __  ___    __| | __ _| |_ __ _ ___  ___| |_
     | |  | | `_ \| `_ \| | | | __| \ \ /\ / / `_ \/ __|  / _` |/ _` | __/ _` / __|/ _ \ __|
     | |  | | | | | |_) | |_| | |_   \ V  V /| |_) \__ \ | (_| | (_| | || (_| \__ \  __/ |_
    |___| |_|_| |_| .__/ \__,_|\__|   \_/\_/ | .__/|___/  \__,_|\__,_|\__\__,_|___/\___|\__|
                  |_|                        |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;informat
    GENE1 $1.
    TYPE1 8.
    TYPE2 8.
    TYPE3 8.
    TYPE4 8.
    ;input
    GENE1 TYPE1 TYPE2 TYPE3 TYPE4;
    cards4;
    A 50 20 5 1
    B 40 30 50 2
    C 20 20 30 3
    D 10 10 10 40
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                   |                                    |                                               */
    /*                INPUT              |      PROCESS                       |     OUTPUT                                    */
    /*                                   |                                    |                                               */
    /*   GENE1 TYPE1 TYPE2 TYPE3 TYPE4   |     SELECT DIAGONAL OF TYPE ARRAY  |     GENE1 TYPES SCORE                         */
    /*                                   |                                    |                                               */
    /* 1     A    50    20     5     1   |     set sd1.have;                  |     A     TYPE1    50 ==> Diagonal Value      */
    /* 2     B    40    30    50     2   |     array typex $32 type1-type4;   |     B     TYPE2    30                         */
    /* 3     C    20    20    30     3   |     types=vname(typex[_n_]);       |     C     TYPE3    30                         */
    /* 4     D    10    10    10    40   |     score=typex[_n_];              |     D     TYPE4    40                         */
    /*                                   |                                    |                                               */
    /**************************************************************************************************************************/


    /*__ ___   _                   _                       _                      _    _                 _
    |_ _|_ _| (_)_ __  _ __  _   _| |_    _____  _____ ___| | __      _____  _ __| | _| |__   ___   ___ | | __
     | | | |  | | `_ \| `_ \| | | | __|  / _ \ \/ / __/ _ \ | \ \ /\ / / _ \| `__| |/ / `_ \ / _ \ / _ \| |/ /
     | | | |  | | | | | |_) | |_| | |_  |  __/>  < (_|  __/ |  \ V  V / (_) | |  |   <| |_) | (_) | (_) |   <
    |___|___| |_|_| |_| .__/ \__,_|\__|  \___/_/\_\___\___|_|   \_/\_/ \___/|_|  |_|\_\_.__/ \___/ \___/|_|\_\
                      |_|
    */


    %utlfkil(d:/xls/have.xlsx);

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(openxlsx);
    wb <- createWorkbook("d:/xls/have.xlsx");
    addWorksheet(wb, "Sheet 1");
    writeData(wb, sheet = 1, x =have , startCol = 1, startRow = 1);
    createNamedRegion(
      wb = wb,
      sheet = 1,
      name = "have",
      rows = 1:(nrow(have) + 1),
      cols = 1:ncol(have)
    );
    addWorksheet(wb, "Sheet 2");
    names(wb);
    saveWorkbook(wb,"d:/xls/have.xlsx",overwrite=TRUE);
    endsubmit;
    ');



    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* INPUTS EXCEL                                                                                                           */
    /*                                                                                                                        */
    /* EXCEL WORKBOOKd:/xls/have.xlsx (two sheets)           WE WILL UPDATE THIS SHEE IN PLACE WITH WANT                      */
    /*                                                                                                                        */
    /*   +-------------                                       +-------------                                                  */                                                 */
    /* 1 |  HAVE      | =>named range                       1 |  A1        |                                                  */                                                 */
    /*   +------------+                                       +------------+                                                  */                                                 */
    /*                                                                                                                        */                                                 */
    /*   +----------------------+-------------+               +----------------------+-------------+                          */                                                 */
    /*   |     A  |  B   |   C  |  D   |  E   |               |     A  |  B   |   C  |  D   |  E   | ...                      */                                                 */
    /*   +----------------------+--------------               +----------------------+--------------                          */                                                 */
    /* 1 | GENE1  |TYPE1 |TYPE2 |TYPE3 |TYPE4 |             1 |        |      |      |      |      | ...                      */                                                 */
    /*   +--------+------+------+------+------+               +--------+------+------+------+------+                          */                                                 */
    /* 2 |    A   | 50   |  20  |  5   |  1   |             2 |        |      |      |      |      | ...                      */                                                 */
    /*   +--------+------+------+------+------+               +--------+------+------+------+------+                          */                                                 */
    /* 3 |    B   | 40   |  30  | 50   |  2   |             3 |        |      |      |      |      | ...                      */                                                 */
    /*   +--------+------+------+------+------+               +--------+------+------+------+------+                          */                                                 */
    /* 4 |    C   | 20   |  20  | 30   |  3   |             4 |        |      |      |      |      | ...                      */                                                 */
    /*   +--------+------+------+------+------+               +--------+------+------+------+------+                          */                                                 */
    /* 5 |    D   | 10   |  10  | 10   | 40   |             5 |        |      |      |      |      | ...                      */                                                 */
    /*   +--------+------+------+------+------+               +--------+------+------+------+------+                          */                                                 */
    /*                                                          ...      ...    ...    ...    ...                             */                                                 */
    /*   +-------------                                       +-------------                                                  */                                                 */
    /*   | SHEET 1    |                                       | SHEET 2    |                                                  */                                                 */
    /*   +------------+                                       +------------+                                                  */                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                            _       _            _
    / | __      ___ __  ___    __| | __ _| |_ __ _ ___| |_ ___ _ __
    | | \ \ /\ / / `_ \/ __|  / _` |/ _` | __/ _` / __| __/ _ \ `_ \
    | |  \ V  V /| |_) \__ \ | (_| | (_| | || (_| \__ \ ||  __/ |_) |
    |_|   \_/\_/ | .__/|___/  \__,_|\__,_|\__\__,_|___/\__\___| .__/
                 |_|                                          |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('

    libname sd1 "d:/sd1";

    data sd1.want;

      length types $32;

      set sd1.have;
        array typex type1-type4;
        types=vname(typex[_n_]);
        score=typex[_n_];

      drop type1-type4;

    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Obs    GENE1    TYPES    SCORE                                                                                         */
    /*                                                                                                                        */
    /*  1       A      TYPE1      50                                                                                          */
    /*  2       B      TYPE2      30                                                                                          */
    /*  3       C      TYPE3      30                                                                                          */
    /*  4       D      TYPE4      40                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                   _
    |___ \  __      ___ __  ___   ___  __ _| |
      __) | \ \ /\ / / `_ \/ __| / __|/ _` | |
     / __/   \ V  V /| |_) \__ \ \__ \ (_| | |
    |_____|   \_/\_/ | .__/|___/ |___/\__, |_|
                     |_|                 |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %array(_ty,values=%varlist(sd1.have,keep=type:));
    %array(_gn,data=sd1.have,var=gene1);

    %utl_submit_wps64x(resolve('
    libname sd1 "d:/sd1";
    options validvarname=any;
    proc sql;
      create
         table sd1.want as
      select
         gene1
        ,case
           %do_over(_gn _ty,phrase=%str(
             when gene1="?_gn" then "?_ty"))
           else "ERR"
         end as type
        ,case
           %do_over(_gn _ty,phrase=%str(
             when gene1="?_gn" then ?_ty))
           else .
         end as score
      from
         sd1.have
    ;quit;
    proc print;
    run;quit;
    '));

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs    GENE1    type     score                                                                                         */
    /*                                                                                                                        */
    /*  1       A      TYPE1      50                                                                                          */
    /*  2       B      TYPE2      30                                                                                          */
    /*  3       C      TYPE3      30                                                                                          */
    /*  4       D      TYPE4      40                                                                                          */
    /*                                                                                                                        */
    /* GENERATED CODE (MAY BE FASTER THAN YOU THINK (ALL WHENS COMPUTED SIMULTANEOUSLY) THREE THROWN OUT LATER                */
    /*                                                                                                                        */
    /*  proc sql;                                                                                                             */
    /*     create                                                                                                             */
    /*        table sd1.want as                                                                                               */
    /*     select                                                                                                             */
    /*        gene1                                                                                                           */
    /*       ,case                                                                                                            */
    /*          when gene1="A" then "TYPE1"                                                                                   */
    /*          when gene1="B" then "TYPE2"                                                                                   */
    /*          when gene1="C" then "TYPE3"                                                                                   */
    /*          when gene1="D" then "TYPE4"                                                                                   */
    /*          else "ERR"                                                                                                    */
    /*        end as type                                                                                                     */
    /*       ,case                                                                                                            */
    /*          when gene1="A" then TYPE1                                                                                     */
    /*          when gene1="B" then TYPE2                                                                                     */
    /*          when gene1="C" then TYPE3                                                                                     */
    /*          when gene1="D" then TYPE4                                                                                     */
    /*          else .                                                                                                        */
    /*        end as score                                                                                                    */
    /*     from                                                                                                               */
    /*        sd1.have                                                                                                        */
    /*  ;quit;                                                                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____                                    _   _
    |___ /  __      ___ __  ___   _ __   __ _| |_(_)_   _____   _ __
      |_ \  \ \ /\ / / `_ \/ __| | `_ \ / _` | __| \ \ / / _ \ | `__|
     ___) |  \ V  V /| |_) \__ \ | | | | (_| | |_| |\ V /  __/ | |
    |____/    \_/\_/ | .__/|___/ |_| |_|\__,_|\__|_| \_/ \___| |_|
                     |_|
    */

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(dplyr);
    dia<-as.data.frame(diag(as.matrix(have[, -1]))) %>% rename_with(.cols = 1, ~"SCORE");
    dia$GENE1 = have$GENE1;
    dia$TYPES = colnames(have[,-1]);
    dia;
    endsubmit;
    import data=sd1.want r=dia;
    run;quit;
    ');

    proc print data=sd1.want width=min;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS PROC R                                                                                                        */
    /*                                                                                                                        */
    /*    SCORE GENE1 TYPES                                                                                                   */
    /*                                                                                                                        */
    /*  1    50     A TYPE1                                                                                                   */
    /*  2    30     B TYPE2                                                                                                   */
    /*  3    30     C TYPE3                                                                                                   */
    /*  4    40     D TYPE4                                                                                                   */
    /*                                                                                                                        */
    /* WPS                                                                                                                    */
    /*                                                                                                                        */
    /* Obs    SCORE    GENE1    TYPES                                                                                         */
    /*                                                                                                                        */
    /*  1       50       A      TYPE1                                                                                         */
    /*  2       30       B      TYPE2                                                                                         */
    /*  3       30       C      TYPE3                                                                                         */
    /*  4       40       D      TYPE4                                                                                         */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                                           _
    | || |   __      ___ __  ___   _ __   ___  __ _| |
    | || |_  \ \ /\ / / `_ \/ __| | `__| / __|/ _` | |
    |__   _|  \ V  V /| |_) \__ \ | |    \__ \ (_| | |
       |_|     \_/\_/ | .__/|___/ |_|    |___/\__, |_|
                      |_|                        |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %array(_ty,values=%varlist(sd1.have,keep=type:));
    %array(_gn,data=sd1.have,var=gene1);

    options ls=255;
    %utl_submit_wps64x(resolve('
    libname sd1 "d:/sd1";
    proc r;
    export data=sd1.have r=have;
    submit;
    library(sqldf);
    want <- sqldf("
      select
         gene1
        ,case
           %do_over(_gn _ty,phrase=%str(
             when gene1=`?_gn` then `?_ty`))
           else `ERR`
         end as type
        ,case
           %do_over(_gn _ty,phrase=%str(
             when gene1=`?_gn` then ?_ty))
           else NULL
         end as score
      from
         have
    ");
    want;
    endsubmit;
    import data=sd1.want r=want;
    run;quit;
    '));

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS System                                                                                                        */
    /*                                                                                                                        */
    /*    GENE1  type score                                                                                                   */
    /*                                                                                                                        */
    /*  1     A TYPE1    50                                                                                                   */
    /*  2     B TYPE2    30                                                                                                   */
    /*  3     C TYPE3    30                                                                                                   */
    /*  4     D TYPE4    40                                                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*___                                   _               _   _
    | ___|  __      ___ __  ___   ___  __ _| |  _ __  _   _| |_| |__   ___  _ __
    |___ \  \ \ /\ / / `_ \/ __| / __|/ _` | | | `_ \| | | | __| `_ \ / _ \| `_ \
     ___) |  \ V  V /| |_) \__ \ \__ \ (_| | | | |_) | |_| | |_| | | | (_) | | | |
    |____/    \_/\_/ | .__/|___/ |___/\__, |_| | .__/ \__, |\__|_| |_|\___/|_| |_|
                     |_|                 |_|   |_|    |___/
    */

    %utl_submit_wps64x("
    options validvarname=any lrecl=32756;
    libname sd1 'd:/sd1';
    proc sql;select max(cnt) into :_cnt from (select count(nam) as cnt from sd1.have group by nam);quit;
    %array(_unq,values=1-&_cnt);
    proc python;
    export data=sd1.have python=have;
    submit;
    print(have);
    from os import path;
    import pandas as pd;
    import numpy as np;
    import pandas as pd;
    from pandasql import sqldf;
    mysql = lambda q: sqldf(q, globals());
    from pandasql import PandaSQL;
    pdsql = PandaSQL(persist=True);
    sqlite3conn = next(pdsql.conn.gen).connection.connection;
    sqlite3conn.enable_load_extension(True);
    sqlite3conn.load_extension('c:/temp/libsqlitefunctions.dll');
    mysql = lambda q: sqldf(q, globals());
    want = pdsql('''
      select
         gene1
        ,case
           %do_over(_gn _ty,phrase=%str(
             when gene1=`?_gn` then `?_ty`))
           else `ERR`
         end as type
        ,case
           %do_over(_gn _ty,phrase=%str(
             when gene1=`?_gn` then ?_ty))
           else NULL
         end as score
      from
         have
    ''');
    print(want);
    endsubmit;
    run;quit;
    "));


    /*----                                                                   ----*/
    /*---- cleanup delete macro arrays                                       ----*/
    /*----                                                                   ----*/

    %arrayDelete(_ty);
    %arrayDelete(_gn);


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The PYTHON Procedure                                                                                                  */
    /*                                                                                                                        */
    /*    GENE1  TYPE1  TYPE2  TYPE3  TYPE4                                                                                   */
    /*  0     A   50.0   20.0    5.0    1.0                                                                                   */
    /*  1     B   40.0   30.0   50.0    2.0                                                                                   */
    /*  2     C   20.0   20.0   30.0    3.0                                                                                   */
    /*  3     D   10.0   10.0   10.0   40.0                                                                                   */
    /*                                                                                                                        */
    /*  The WPS System                                                                                                        */
    /*                                                                                                                        */
    /*    GENE1   type  score                                                                                                 */
    /*  0     A  TYPE1   50.0                                                                                                 */
    /*  1     B  TYPE2   30.0                                                                                                 */
    /*  2     C  TYPE3   30.0                                                                                                 */
    /*  3     D  TYPE4   40.0                                                                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*__                       _             _     _   _        _     _       _               _
     / /_     _____  _____ ___| |   __ _  __| | __| | | |_ __ _| |__ | | ___ (_)_ __    _ __ | | __ _  ___ ___
    | `_ \   / _ \ \/ / __/ _ \ |  / _` |/ _` |/ _` | | __/ _` | `_ \| |/ _ \| | `_ \  | `_ \| |/ _` |/ __/ _ \
    | (_) | |  __/>  < (_|  __/ | | (_| | (_| | (_| | | || (_| | |_) | |  __/| | | | | | |_) | | (_| | (_|  __/
     \___/   \___/_/\_\___\___|_|  \__,_|\__,_|\__,_|  \__\__,_|_.__/|_|\___||_|_| |_| | .__/|_|\__,_|\___\___|
                                                                                       |_|
    */

    /*----                                                                   ----*/
    /*---- ADD NAMED RANGE WANT INTO SHEET 2                                 ----*/
    /*----                                                                   ----*/

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('

    /*----                                                                   ----*/
    /*----  Write the want table back to the excel workbook in place         ----*/
    /*----                                                                   ----*/

    libname sd1 "d:/sd1";
    libname xls excel "d:/xls/have.xlsx";

    data sd1.want;
      set xls.have;
      array typex type1-type4;
      types=vname(typex[_n_]);
      score=typex[_n_];
      drop type1-type4;
    run;quit;

    libname xls clear;

    proc r;
    export data=sd1.want r=want;
    submit;
    library(openxlsx);
    wb <- loadWorkbook("d:/xls/have.xlsx");
    writeData(wb, sheet = 2, x =want , startCol = 1, startRow = 1);
    createNamedRegion(
      wb = wb,
      sheet = 1,
      name = "want",
      rows = 1:(nrow(want) + 1),
      cols = 1:ncol(want)
    );
    saveWorkbook(wb,"d:/xls/have.xlsx",overwrite=TRUE);
    endsubmit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* INPUTS EXCEL                                           OUTPUT                                                          */
    /*                                                                                                                        */
    /* EXCEL WORKBOOK d:/xls/have.xlsx (two sheets)           WE WILL UPDATE THIS SHEE IN PLACE WITH WANT                     */
    /*                                                                                                                        */
    /*   +-------------                                       +-------------                                                  */
    /* 1 |  HAVE      | =>named range                       1 |  WANT      | =>named range                                    */
    /*   +------------+                                       +------------+                                                  */
    /*                                                                                                                        */
    /*   +----------------------+-------------+               +----------------------+                                        */
    /*   |     A  |  B   |   C  |  D   |  E   |               |     A  |  B   |   C  |                                        */
    /*   +----------------------+--------------               +----------------------+                                        */
    /* 1 | GENE1  |TYPE1 |TYPE2 |TYPE3 |TYPE4 |             1 |  GENE1 |TYPES |SCORE | fROM DIAGONAL                          */
    /*   +--------+------+------+------+------+               +--------+------+------+                                        */
    /* 2 |    A   | 50*  |  20  |  5   |  1   |             2 |    A   |TYPE1 | 50   |                                        */
    /*   +--------+------+------+------+------+               +--------+------+------+                                        */
    /* 3 |    B   | 40   |  30* | 50   |  2   |             3 |    B   |TYPE2 | 30   |                                        */
    /*   +--------+------+------+------+------+               +--------+------+------+                                        */
    /* 4 |    C   | 20   |  20  | 30*  |  3   |             4 |    C   |TYPE3 | 30   |                                        */
    /*   +--------+------+------+------+------+               +--------+------+------+                                        */
    /* 5 |    D   | 10   |  10  | 10   | 40*  |             5 |    D   |TYPE4 | 40   |                                        */
    /*   +--------+------+------+------+------+               +--------+------+------+                                        */
    /*                                                          ...      ...    ...                                           */
    /*   +-------------                                       +-------------                                                  */
    /*   | SHEET 1    |                                       | SHEET 2    |                                                  */                                              */
    /*   +------------+                                       +------------+                                                  */                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
