# utl-proc-report-compute-with-multiple-across-variables
Proc Report Compute with multiple across variables.
    Proc Report Compute with multiple across variables

    I create a table and a static proc report. Yable should be more useful?
    Note this is a flexible transpose.

    Two pairs of variables are from one variable, predicted sales.

     EAST sales variable
     WEST sales variable

     FURNITURE sales
     OFFICE    sales

    github
    https://tinyurl.com/y7ahqvxw
    https://github.com/rogerjdeangelis/utl-proc-report-compute-with-multiple-across-variables

    SAS Forum
    https://tinyurl.com/y9lmctn7
    https://communities.sas.com/t5/SAS-Programming/Proc-Report-Compute-with-multiple-across-variables/m-p/510427

    macros
    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories


    INPUT
    =====

    WORK.HAVE total obs=8

      COUNTRY    REGION    PRODTYPE     PREDICT

      CANADA      EAST     FURNITURE      851
      CANADA      EAST     OFFICE         452
      CANADA      WEST     OFFICE         231
      CANADA      WEST     OFFICE          98
      GERMANY     EAST     FURNITURE      110
      GERMANY     EAST     OFFICE         953
      CANADA      WEST     FURNITURE      583
      CANADA      EAST     OFFICE         679


    EXAMPLE OUTPUT
    --------------

    WORK.WANT total obs=3
                                -----                              ----
      COUNTRY    EAST    WEST   RULES        FURNITURE    OFFICE   RULES

      CANADA     1982     912  =231+98+583      1434       1460  = 452+231+98+679
      GERMANY    1810    1466                    686       2590


    PROCESS
    =======

    ods output report=want(drop=_break_ rename=(
        %utl_renamel( old=_c2_ _c3_ _c4_ _c5_,new=east west furniture office)));
    proc report data=have nowindows missing headline headskip;
        column country (region prodtype) , predict;
        define country / group;
        define region  / across;
        define prodtype / across;
    run;quit;

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    PROC SQL;
    CREATE TABLE have AS
    SELECT   country
            ,region
            ,prodtype
            ,predict
    FROM sashelp.prdsale(obs=8)
    WHERE mod(monotonic(),75)=0
    ORDER BY ranuni(94612);
    QUIT;


