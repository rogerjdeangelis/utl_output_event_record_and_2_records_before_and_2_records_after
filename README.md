# utl_output_event_record_and_2_records_before_and_2_records_after
Output event record and 2 records before and 2 records after.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Output event record and 2 records before and 2 records after

       Same result in WPS and SAS

       if flag=1 then output event record and 2 records before and 2 records after

       None of the solutions handle consecutive flags.
       Using point option in set statement seem to work

    github
    https://tinyurl.com/y76levco
    https://github.com/rogerjdeangelis/utl_output_event_record_and_2_records_before_and_2_records_after

    see
    https://stackoverflow.com/questions/51030148/how-do-i-grab-rows-surrounding-a-flagged-value

    INPUT
    =====

     WORK.HAVE total obs=12  |         RULES (20 Obs out)
                             |
        CODE     FLAG        |   GRP
                             |
       abc123      0         |    1   abc123      0
       xyz456      0         |    1   xyz456      0
       wer098      1         |    1   wer098      1    * two before and two after the 1
       jio234      0         |    1   jio234      0
       bcx190      0         |    1   bcx190      0


       eiw157      1         |    2   jio234      0
       nzi123      1         |    2   bcx190      0
       epj676      0         |    2   eiw157      1   * two before and two after the 1
                                  2   nzi123      1
                                  2   epj676      0

                                  3   bcx190      0
                                  3   eiw157      1
                                  3   nzi123      1  *  two before and two after the 1
                                  3   epj676      0
                                  3   ere654      0

       ere654      0         |    4   epj676      0
       yru493      1         |    4   ere654      0
       ale674      0         |    4   yru493      1  *  two before and two after run;quit;
       dy6514      0         |    4   ale674      0
                             |    4   dy6514      0

    PROCESS
    =======

    data want;
      retain grp 0;
      do pt=3 to obs-2 by 1;
         set have point=pt nobs=obs; ptv=flag;
        if ptv then do;
           grp=grp+1;
           do pt3=pt-2 to pt+2;
              set have point = pt3;
              output;
           end;
        end;
      end;
      drop ptv;
    stop;
    run;quit;


    OUTPUT
    ======

    Up to 40 obs WORK.WANT total obs=20

      GRP     CODE     FLAG

       1     abc123      0
       1     xyz456      0
       1     wer098      1
       1     jio234      0
       1     bcx190      0

       2     jio234      0
       2     bcx190      0
       2     eiw157      1
       2     nzi123      1
       2     epj676      0

       3     bcx190      0
       3     eiw157      1
       3     nzi123      1
       3     epj676      0
       3     ere654      0

       4     epj676      0
       4     ere654      0
       4     yru493      1
       4     ale674      0
       4     dy6514      0


    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data have;
      input code $ flag ;
    cards4;
    abc123   0
    xyz456   0
    wer098   1
    jio234   0
    bcx190   0
    eiw157   1
    nzi123   1
    epj676   0
    ere654   0
    yru493   1
    ale674   0
    dy6514   0
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    data want;
      retain grp 0;
      do pt=3 to obs-2 by 1;
         set wrk.have point=pt nobs=obs; ptv=flag;
        if ptv then do;
           grp=grp+1;
           do pt3=pt-2 to pt+2;
              set wrk.have point = pt3;
              output;
           end;
        end;
      end;
      drop ptv;
    stop;
    run;quit;
    proc print;
    run;quit;
    ');


