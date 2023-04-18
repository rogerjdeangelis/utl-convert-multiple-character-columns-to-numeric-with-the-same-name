# utl-convert-multiple-character-columns-to-numeric-with-the-same-name
Convert multiple character columns to numeric with the same name
    %let pgm=utl-convert-multiple-character-columns-to-numeric-with-the-same-name;

    Convert multiple character columns to numeric with the same name;

    github
    https://tinyurl.com/55rau6uw
    https://github.com/rogerjdeangelis/utl-convert-multiple-character-columns-to-numeric-with-the-same-name


    StackOverflow
    https://stackoverflow.com/questions/75385595/sas-convert-multiple-columns-to-numeric

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    data have;

       array r $ rf1 - rf10;

       do _N_ = 1 to 10;
          do over r;
             r = put(rand('integer', 1, 10), 8.);
          end;
          output;
       end;

    run;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Alphabetic List of Variables and Attributes                                                                            */
    /*                                                                                                                        */
    /*  #    Variable    Type    Len                                                                                          */
    /*                                                                                                                        */
    /*  1    RF1         Char      8                                                                                          */
    /*  2    RF2         Char      8                                                                                          */
    /*  3    RF3         Char      8                                                                                          */
    /*  4    RF4         Char      8                                                                                          */
    /*  5    RF5         Char      8                                                                                          */
    /*  6    RF6         Char      8                                                                                          */
    /*  7    RF7         Char      8                                                                                          */
    /*  8    RF8         Char      8                                                                                          */
    /*  9    RF9         Char      8                                                                                          */
    /* 10    RF10        Char      8                                                                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*                      Alphabetic List of Variables and Attributes                                                       */
    /*                                                                                                                        */
    /* #    Variable    Type    Len    Label                                                                                  */
    /*                                                                                                                        */
    /* 1    RF1         Num       8    Character rf1 converted to numeric rf1 using best12.                                   */
    /* 2    RF2         Num       8    Character rf2 converted to numeric rf2 using best12.                                   */
    /* 3    RF3         Num       8    Character rf3 converted to numeric rf3 using best12.                                   */
    /* 4    RF4         Num       8    Character rf4 converted to numeric rf4 using best12.                                   */
    /* 5    RF5         Num       8    Character rf5 converted to numeric rf5 using best12.                                   */
    /* 6    RF6         Num       8    Character rf6 converted to numeric rf6 using best12.                                   */
    /* 7    RF7         Num       8    Character rf7 converted to numeric rf7 using best12.                                   */
    /* 8    RF8         Num       8    Character rf8 converted to numeric rf8 using best12.                                   */
    /* 9    RF9         Num       8    Character rf9 converted to numeric rf9 using best12.                                   */
    /* 0    RF10        Num       8    Character rf10 converted to numeric rf10 using best12.                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     ___  __ _ ___   _ __  _ __ ___   ___ ___  ___ ___
    / __|/ _` / __| | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    \__ \ (_| \__ \ | |_) | | | (_) | (_|  __/\__ \__ \
    |___/\__,_|___/ | .__/|_|  \___/ \___\___||___/___/
                    |_|
    */

    %array(_num,values=1-10);

    proc sql;
      create
         table want as
      select
         %do_over(_num,phrase=%str(
            input(rf?,best.) as rf? label="Character rf? converted to numeric rf? using best12."
            ),between=comma)
      from
         have
    ;quit;

    /*
    __      ___ __  ___   _ __  _ __ ___   ___ ___  ___ ___
    \ \ /\ / / `_ \/ __| | `_ \| `__/ _ \ / __/ _ \/ __/ __|
     \ V  V /| |_) \__ \ | |_) | | | (_) | (_|  __/\__ \__ \
      \_/\_/ | .__/|___/ | .__/|_|  \___/ \___\___||___/___/
             |_|         |_|
    */

    %let _pth=%sysfunc(pathname(work));

    %utl_submit_wps64("

      libname wrk '&_pth';

      %array(_num,values=1-10);

      proc sql;
        create
           table want_wps as
        select
           %do_over(_num,phrase=%str(
              input(rf?,best.) as rf? label='Character rf? converted to numeric rf? using best12.'
              ),between=comma)
        from
           wrk.have
      ;quit;

      proc print data=want_wps;
      run;quit;
    ");

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs    rf1    rf2    rf3    rf4    rf5    rf6    rf7    rf8    rf9    rf10                                             */
    /*                                                                                                                        */
    /*   1     2       8      2     6       7     10      8     5       2      4                                              */
    /*   2     9       3      2     4       6      6      9     1       9      1                                              */
    /*   3     4       5      2     9       6      7      7     7       3      3                                              */
    /*   4     3      10      6     8       9      5      5     7       8      9                                              */
    /*   5     6       6      1     9       9      6      6     6       7      1                                              */
    /*   6     1       4      2     7       8      7      5     8       4      8                                              */
    /*   7     4       3      6     9      10      9      2     8       6      3                                              */
    /*   8     1       5      9     7       5      8      9     5       9      8                                              */
    /*   9     4       5     10     4       6      6     10     9       5      6                                              */
    /*  10     5       3      7     5       1      8      1     9      10     10                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
