# utl-using-sas-compatible-character-and-numeric-missing-values-in-spss-pspp
Using sas compatible character and numeric missing in spss pspp
    %let pgm=utl-using-sas-compatible-character-and-numeric-missing-values-in-spss-pspp;

    Using sas compatible character and numeric missing in spss pspp

    The op was having difficulty importing spss sav into sas files because of a unknown missing values.

    github
    https://tinyurl.com/v9u29ppd
    https://github.com/rogerjdeangelis/utl-using-sas-compatible-character-and-numeric-missing-values-in-spss-pspp

    sas communities
    https://tinyurl.com/3hr77zsu
    https://communities.sas.com/t5/SAS-Programming/Values-in-SPSS-like-don-t-know-amp-not-applicable-are-imported/m-p/765835#M242612

    You can use the open source PSPP clone of SPSS to read change the missing values to values sas
    will convert to missing values

    THE FIX (YOU DO NOT NEED SPSS TO FIX THIS PROBLEM)

        RECODE                                TO USE SAS MISSINGS (YOU CAN READ THE PROBEM SAV AN EDIT IT IN PSPP)

     DATA LIST FREE / id gender (A2) age.     DATA LIST FREE / id gender (A2) age
     BEGIN DATA                               BEGIN DATA
     1 M   25                                 1 M   25
     2 F   30                                 2 F   30
     3 NA  -99                                3 ""  .
     4 F   40                                 4 ""  .
     5 NA  -99                                5 F   40
     END DATA.                                END DATA.
     MISSING VALUES age (-99).
     MISSING VALUES gender ('NA').

    RELATED REPOS
    -----------------------------------------------------------------------------------------------------------------------------------
    https://github.com/rogerjdeangelis/utl-creating-spss-tables-from-a-sas-datasets-using-sas-r-and-python
    https://github.com/rogerjdeangelis/utl-dropping-down-to-spss-using-the-pspp-free-clone-and-running-a-spss-linear-regression
    https://github.com/rogerjdeangelis/utl-identifying-the-html-table-and-exporting-to-spss-then-sas-scraping
    https://github.com/rogerjdeangelis/utl-import-dbf-dif-ods-xlsx-spss-json-stata-csv-html-xml-tsv-files-without-sas-access-products
    https://github.com/rogerjdeangelis/utl-removing-factors-and-preserving-type-and-length-when-importing-spss-sav-tables
    https://github.com/rogerjdeangelis/utl-sas-to-and-from-sqllite-excel-ms-access-spss-stata-using-r-packages-without-sas


    /**************************************************************************************************************************/
    /* PROCBEM                                   | PROCESS                                        | OUTPUT                    */
    /* =======                                   | =======                                        | ======                    */
    /* SAS PROC IMPORT DOES NOT CONVERT          | RECODE SPSS/PSPP                               | SAS                       */
    /* THE USER MISSINGS BELOW PROPERLY          |                                                | ID    GENDER    AGE       */
    /*                                           | YOU CAN READ AN EXISTING SAV                   |                           */
    /*                                           | Wr66ITH PSPP CAN RECODE MISSINGS               |    1       M        25    */
    /* DATA LIST FREE / id gender (A2) age.      | TO SAS MISSINGS,without SPSS                   | 2       F        30       */
    /* BEGIN DATA                                |                                                | 3                 .       */
    /* 1 M   25                                  |                                                | 4                 .       */
    /* 2 F   30                                  | %utlfkil(d:\sav\mydata.sav);                   | 5       F        40       */
    /* 3 NA  -99                                 |                                                |                           */
    /* 4 F   40                                  | %utl_psppbeginx;                               |                           */
    /* 5 NA  -99                                 | parmcards4;                                    | cntMisChr  cntMisNum      */
    /* END DATA.                                 | DATA LIST FREE / id gender (A2) age            | --------------------      */
    /* USER MISSIN                               | BEGIN DATA                                     |         2          2      */
    /* MISSING VALUES age (--9).                 | 1 M   25                                       |                           */
    /* MISSING VALUES gender ('NA').             | 2 F   30                                       |                           */
    /*                                           | 3 ""  .                                        |                           */
    /*                                           | 4 ""  .                                        |                           */
    /*                                           | 5 F   40                                       |                           */
    /*                                           | END DATA.                                      |                           */
    /*                                           | SAVE OUTFILE='d:\sav\mydata.sav'.              |                           */
    /*                                           | ;;;;                                           |                           */
    /*                                           | %utl_psppendx;                                 |                           */
    /*                                           |                                                |                           */
    /*                                           | THIS READS THE PSPP SAV FILE                   |                           */
    /*                                           | AND REMAPS MISSINGS TO SAS MISSINGS            |                           */
    /*                                           | AND CREATES SAS DATASET WITHOUT SAS ACCESS     |                           */
    /*                                           |                                                |                           */
    /*                                           | %utl_rbeginx;                                  |                           */
    /*                                           | parmcards4;                                    |                           */
    /*                                           | library(haven)                                 |                           */
    /*                                           | library(sqldf)                                 |                           */
    /*                                           | source("c:/oto/fn_tosas9x.R")                  |                           */
    /*                                           | options(sqldf.dll = "d:/dll/sqlean.dll")       |                           */
    /*                                           | want <- read_sav('d:/sav/mydata.sav')          |                           */
    /*                                           | fn_tosas9x(                                    |                           */
    /*                                           |       inp    = want                            |                           */
    /*                                           |      ,outlib ="d:/sd1/"                        |                           */
    /*                                           |      ,outdsn ="want"                           |                           */
    /*                                           |      )                                         |                           */
    /*                                           | ;;;;                                           |                           */
    /*                                           | %utl_rendx;                                    |                           */
    /*                                           |                                                |                           */
    /*                                           | proc print data=sd1.want;                      |                           */
    /*                                           | run;quit;                                      |                           */
    /*                                           |                                                |                           */
    /*                                           | /*---- CHECK ----*/                            |                           */
    /*                                           |                                                |                           */
    /*                                           | proc sql;                                      |                           */
    /*                                           |   select                                       |                           */
    /*                                           |      nmiss(gender)  as cntMisChr               |                           */
    /*                                           |     ,nmiss(age)    as cntMisNum                |                           */
    /*                                           |   from                                         |                           */
    /*                                           |      sd1.want                                  |                           */
    /*                                           | ;quit;                                         |                           */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

