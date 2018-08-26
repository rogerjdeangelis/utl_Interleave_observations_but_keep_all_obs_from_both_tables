# utl_Interleave_observations_but_keep_all_obs_from_both_tables
Interleave observations but keep all obs from both tables.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Interleave observations but keep all obs from both tables

      This is like an interleave outer join;

      Insired by
      https://stackoverflow.com/users/4965549/tom

      PROBLEM:
      --------

       data problem;
          set HavLarger;  output;  * has 5 obs;
          set HavSmaller; output;  * has 2 obs stops datastep when exhausted;
       run;quit;

       Interleave will only have 5 observations

       PROBLEM total obs=5

        Obs    REC

         1       1
         2      11
         3       2
         4      12
         5       3

       I WANT

        Obs    REC

         1       1
         2      11
         3       2
         4      12
         5       3

         6       4  Add these from larger table
         7       5


    INPUT
    =====

      WORK.HAVLARGER total obs=5

        Obs    REC

         1      1
         2      2
         3      3
         4      4
         5      5


      WORK.HAVSMALLER total obs=2

         Obs    REC

          1      11
          2      12


    PROCESS
    =======

       data want;
         set havLarger;
         output;
         if _n_ <= nobs then do;
            set havSmaller nobs=nobs;
            output;
         end;
       run;quit;

       NOTE: The data set WORK.WANT has 7 observations and 1 variables.


    OUTPUT
    ======

      WORK.WANT total obs=7

        Obs    REC

         1       1
         2      11
         3       2
         4      12
         5       3
         6       4
         7       5

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data havLarger;
      do rec=1 to 5;
        output;
      end;
    run;quit;

    data havSmaller;
      do rec=11 to 12;
        output;
      end;
    run;quit;


