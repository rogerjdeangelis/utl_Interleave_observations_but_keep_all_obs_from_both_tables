# utl_Interleave_observations_but_keep_all_obs_from_both_tables
Interleave observations but keep all obs from both tables.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Interleave observations but keep all obs from both tables

      This is like an interleave outer join;

      Insired by
      https://stackoverflow.com/users/4965549/tom
      
      Recent addition on end
      see sage advise from
      "Keintz, Mark" <mkeintz@WHARTON.UPENN.EDU

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


    *__  __            _
    |  \/  | __ _ _ __| | __
    | |\/| |/ _` | '__| |/ /
    | |  | | (_| | |  |   <
    |_|  |_|\__,_|_|  |_|\_\

    ;

    "Keintz, Mark" <mkeintz@WHARTON.UPENN.EDU

    I should also have added that if one wanted to stick with the original
    answer using conditional sets, you could always add a
       call missing(of _all_);
    at the bottom of the data step.   But often there are other tracking
    variables you would want to preserve over data step iterations.

    The solution presented depends on the unspoken assumption that both
    datasets have exactly the same variables.  Take a look below at SEX HEIGHT
     and WEIGHT resulting from this answer.  Instead of missing values where
    intuitively expected, it has retained values from the data set
    that doesn't contain the observation-in-hand.

    data havlarger;
      set sashelp.class (drop=height weight);
      if mod(_n_,4)=1;
    run;
    data havsmaller ;
      set sashelp.class (obs=6 drop=sex);
      if mod(_n_,4)=2;
    run;

    data want;
      set havLarger;
      output;
      if _n_ <= nobs then do;
        set havSmaller nobs=nobs;
        output;
      end;
    run;

    Of course you could issue a keep statement restricted to common
    variables, but that's a dataset-specific solution.  Better to:

    data vlarg/view=vlarg;
      set havlarger;
      byvar=_n_;
    run;
    data vsmal/view=vsmal;
      set havsmaller;
      byvar=_n_;
    run;
    data want (drop=byvar);
      set vlarg vsmal;
      by byvar;
    run;

    Conditional execution of SET statements are good for carrying
    forward historic values - but I wouldn't recommend that
    technique for this problem

    Regards,
    Mark



