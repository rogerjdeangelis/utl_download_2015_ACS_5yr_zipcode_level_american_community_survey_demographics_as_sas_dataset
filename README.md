# utl_download_2015_ACS_5yr_zipcode_level_american_community_survey_demographics_as_sas_dataset
Download 2015 ACS 5yr zipcode level American Community Survey demographic as sas dataset.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Download 2015 ACS 5yr zipcode level American Community Survey demographic as sas dataset
    
    new link to U of Missouri 2018 5yr ACS ZCTA data for entire us 
    http://mcdc.missouri.edu/data/acs2018/

    Zipcode dataset (all numeric variables = 100*round(variable,1) )
    Miniscule file, 61mb, but github only allows mico miniscule files(<25mb)

    SAS data mo_zcta2015Cut.sas7bdat 49mb
    https://www.dropbox.com/s/67djpangxjthv8o/mo_zcta2015cut.zip?dl=0

    github
    https://tinyurl.com/ybomcqsg
    https://github.com/rogerjdeangelis/utl_download_2015_ACS_5yr_zipcode_level_american_community_survey_demographics_as_sas_dataset

    VooDoo output
    https://tinyurl.com/yayeb8uy
    https://github.com/rogerjdeangelis/utl_download_2015_ACS_5yr_zipcode_level_american_community_survey_demographics_as_sas_dataset/blob/master/mo_zcta2015Cut_vdo.zip

    I have repositories with VooDoo and Optlen

    Many thanks to the University of Missouri
    Specifically: John Blodgett blodgettj@umsystem.edu
                  Glenn Rice riceg@missouri.edu

    2016 may be available now, 2017 not yet, in the works?

    ZCTA is slightly different from Zipcode.
    Use alt zips for matching if you do not match on ZCTA(33k out of 42k zips) and altzips are missing

    ALTZIPS

      Rank     Value                    Frequency
      ----     -----                    ---------


        2    00501 00544                       1  Only use these if you do not match on ZCTA5
        3    01004 01059                       1
        4    01014                             1
        5    01021                             1
        6    01041                             1
        7    01061                             1
        8    01090                             1
        9    01101 01102 01115 01144           1
       10    01111 01139                       1
      ...

    Where to get the data

    Observations           32989
    Variables              680

    MOE variables have been removed, download if you want then,
    These are used to compute Margin of Error, see ACS DOC
    https://www.census.gov/programs-surveys/acs/guidance/training-presentations/acs-moe.html

    *      _   _                     _       _
      ___ | |_| |__   ___ _ __    __| | __ _| |_ __ _
     / _ \| __| '_ \ / _ \ '__|  / _` |/ _` | __/ _` |
    | (_) | |_| | | |  __/ |    | (_| | (_| | || (_| |
     \___/ \__|_| |_|\___|_|     \__,_|\__,_|\__\__,_|

    ;

    https://tinyurl.com/y9vmttro
    http://mcdc.missouri.edu/cgi-bin/broker?_PROGRAM=websas.uex2dex.sas&_SERVICE=appdev&path=/pub/data/acs2015&dset=uszctas5yr&view=0

    I. Choose Output Format(s)
         select SAS dataset

       View qmeta Metadata report:
         select all variables

    2. Select Extract data

       Wait a minute
       a screen will pop up

       select
         download


    * GET LIST OF MARGIN OF ERROR VARIABLES TO DROP;

    proc sql;
      select name into :moes separated by " "
      from sashelp.vcolumn
      where libname='LUP' and
            memname='MO_ZCTA2015' and
            scan(upcase(name),-1,'_')="MOE"
    ;quit;


    * REMOVE MISSING AND DUPLICATE VARIABLES AND ROUND 100*VALUE;

    data lup.mo_zcta2015Cut ;
      * drop 100% missing and margin of error variables;
      retain

        ZCTA5      /* primary keys */
        LOGRECNO   /* very usefull to match back to census data */
        GEOID

        DATECREATED
        PERIOD
        SUMLEV
        VINTAGE
      ;
      format county $5.;

      set lup.mo_zcta2015(drop=
        &moes       /* margin of error  variables for stat sig testing */

        AIANHH      /* 100% missing */
        AIANHHFP
        AIHHTLI
        AITS
        ANRC
        BG
        CBSA
        CBSATYPE
        CBSAYR
        CNECTA
        CONCIT
        CONGDIST
        COUSUBFP
        CSA
        DIVISION
        MACC
        MEMI
        METDIV
        NECTA
        NECTADIV
        PCIND
        PCTNOTINSUREDUNDER1
        PLACEFP
        PUMA
        REGION
        SDELM
        SDSEC
        SDUNI
        SLDL
        SLDU
        SUBMCD
        TAZ
        TRACT
        UA
        UR
        ZIPNAME
        AREANAME

        FIPCO       /* duplicate columns */
        ESRIID

        FAMHHS
        PCTFAMHHS   /* perfect correlated with other variables also same data? */
        TOTHHS
        BORNABROAD
        OCCHUS
        FBMINUSSEA

    );

     * rounded 100 times - reduces size to 3 bytes - just because of limited space on github */
     array nums[*] _numeric_;
     do i=1 to dim(nums);
       nums[i]=100*round(nums[i],1);
     end;
     drop I;
    run;quit;

    * logarithmic transformations for skewed variables ;

    * reduce lengths
    %utl_optlen(inp=lup.mo_zcta2015Cut,out=lup.mo_zcta2015Cut);

    %inc "c:/oto/oto_voodoo.sas";

    %utlnopts;
    %utlvdoc
        (
        libname        = lup
        ,data          = mo_zcta2015Cut
        ,key           = zcta
        ,ExtrmVal      = 10
        ,UniPlot       = 1
        ,UniVar        = 1
        ,chart         = 1
        ,misspat       = 0
        ,taball        = 0
        ,tabone        = stab
        ,mispop        = 1
        ,mispoptbl     = 1
        ,dupcol        = 1
        ,unqtwo        = 0
        ,vdocor        = 1
        ,oneone        = 0
        ,cramer        = 0
        ,optlength     = 1
        ,maxmin        = 1
        ,unichr        = 1
        ,outlier       = 1
        ,printto       = d:\pos\vdo\&data..txt
        ,Cleanup       = 0
        );


    *         _             _                        __
    __      _| |__  _   _  | |_ _ __ __ _ _ __  ___ / _| ___  _ __ _ __ ___
    \ \ /\ / / '_ \| | | | | __| '__/ _` | '_ \/ __| |_ / _ \| '__| '_ ` _ \
     \ V  V /| | | | |_| | | |_| | | (_| | | | \__ \  _| (_) | |  | | | | | |
      \_/\_/ |_| |_|\__, |  \__|_|  \__,_|_| |_|___/_|  \___/|_|  |_| |_| |_|
                    |___/
    ;
    *
     _ __ __ ___      __
    | '__/ _` \ \ /\ / /
    | | | (_| |\ V  V /
    |_|  \__,_| \_/\_/

    ;

    The UNIVARIATE Procedure
    Variable:  MEDIANHVALUE  (Median home value)

                                Moments

    N                       30667    Sum Weights              30667
    Mean               17511047.4    Sum Observations    5.37011E11
    Std Deviation      15534035.3    Variance            2.41306E14
    Skewness           3.99331736    Kurtosis            27.1010567
    Uncorrected SS     1.68035E19    Corrected SS         7.3999E18
    Coeff Variation    88.7099152    Std Error Mean      88705.1104


                  Basic Statistical Measures

        Location                    Variability

    Mean     17511047
    Median   12910000

                              Histogram                          #  Boxplot
     2.05E8+*                                                   20     *
           .*                                                    1     *
     1.85E8+*                                                    2     *
           .*                                                    5     *
     1.65E8+*                                                    8     *
           .*                                                    8     *
     1.45E8+*                                                    2     *
           .*                                                   15     *
     1.25E8+*                                                   13     *
           .*                                                   27     *
     1.05E8+*                                                   39     *
           .*                                                   73     *
      8.5E7+*                                                   97     *
           .*                                                  159     *
      6.5E7+**                                                 314     *
           .**                                                 386     0
      4.5E7+***                                                790     0
           .*******                                           1714     0
      2.5E7+****************                                  4100  +-----+
           .************************************************ 12644  *--+--*
    5000000+***************************************          10250  +-----+
            ----+----+----+----+----+----+----+----+----+---
            * may represent up to 264 counts

    *_                        __                               _
    | |_ _ __ __ _ _ __  ___ / _| ___  _ __ _ __ ___   ___  __| |
    | __| '__/ _` | '_ \/ __| |_ / _ \| '__| '_ ` _ \ / _ \/ _` |
    | |_| | | (_| | | | \__ \  _| (_) | |  | | | | | |  __/ (_| |
     \__|_|  \__,_|_| |_|___/_|  \___/|_|  |_| |_| |_|\___|\__,_|

    ;
    Variable:  MEDIANHVALUE  (Median home value)

                                Moments

    N                       30667    Sum Weights              30667
    Mean               16.4444214    Sum Observations    504301.071
    Std Deviation       0.6440953    Variance            0.41485875
    Skewness           0.50641678    Kurtosis            0.68754994
    Uncorrected SS     8305661.39    Corrected SS        12722.0585
    Coeff Variation    3.91680122    Std Error Mean      0.00367802


    Basic Statistical Measures

        Location

    Mean     16.44442
    Median   16.37351
    Mode     16.23588


                             Histogram                          #  Boxplot
     19.25+*                                                   23     0
          .*                                                   83     0
          .***                                                468     0
          .********                                          1406     |
          .*******************                               3672     |
          .**************************************            7456  +-----+
          .************************************************  9531  *--+--*
          .***********************************               6767  +-----+
          .******                                            1047     |
          .*                                                  168     0
          .*                                                   25     0
     13.75+*                                                   21     0
           ----+----+----+----+----+----+----+----+----+---
           * may represent up to 199 counts


    * TRANSFORMATIONS ON SKEWED VARIABLES;

    ods output summary=skuOds;
    proc means stackodsoutput skew max data=lup.mo_zcta2015cut(drop=intptlat intptlon
      datecreated pctstate naltzips landsqmi areasqmi totpopsf1 logrecno);
    run;quit;

    * BUILD TRANSFORMATION EQUATIONS;

    data _null_;
      length equ $32756;
      retain equ;
      set skuOds end=dne;
      if skew>0 then equ=cats(equ,variable,'=log(',variable,');');
      else equ=cats(equ,variable,'=log(',put(max+1,15.),'-',variable,');');
      if dne then call symputx('equs',equ);
    run;quit;

    * APPLY TRANSFORMATIONS;

    data lup.mo_zcta2015Xfm;
      set lup.mo_zcta2015cut;
      array num[*] _numeric_;
      do i=1 to dim(num);
        if num[i]=0 then num[i]=1;
      end;
      &equs;
      drop i;
    run;quit;

    *_     __          ____
    \ \   / /__   ___ |  _ \  ___   ___
     \ \ / / _ \ / _ \| | | |/ _ \ / _ \
      \ V / (_) | (_) | |_| | (_) | (_) |
       \_/ \___/ \___/|____/ \___/ \___/

    ;

    Dataset summary for LUP.MO_ZCTA2015CUT

        Observations:         32989
        Variables:            680
        ----------------------------------------

        Variables by type:
        -------------------
           Numeric:      668
             Quantity:        667
             Date/Time:         1

           Character:     12
        =========================

    * I dropped these and the margin of error variables;

    The following variables are missing or unevaluated for all occurances

           #    Variable Name                      Label
        ----    -------------------------------    ----------------------------------------

           1    AIANHH
           2    AIANHHFP
           3    AIHHTLI
           4    AITS
           5    ANRC
           6    BG
           7    CBSA
           8    CBSATYPE
           9    CBSAYR
          10    CNECTA
          11    CONCIT
          12    CONGDIST                           114th (2015) Congressional District
          13    COUSUBFP                           County Subdiv (Town) code
          14    CSA                                Consolidated Stat Area
          15    DIVISION
          16    MACC
          17    MEMI
          18    METDIV                             Metro Div.
          19    NECTA                              New England City/Town Area
          20    NECTADIV
          21    PCIND
          22    PCTNOTINSUREDUNDER1
          23    PLACEFP
          24    PUMA
          25    REGION
          26    SDELM
          27    SDSEC
          28    SDUNI                              Unified Schl Dist
          29    SLDL                               State Leg Dist - Lower Chamber    label
          30    SLDU                               State Leg Dist - Upper Chamber
          31    SUBMCD
          32    TAZ
          33    TRACT
          34    UA                                 Urban Area/Cluster
          35    UR


    KEEP THESE

     The following variables are uniformly evaluated with NO missing values
    a single non-missing value is present for all observations

           #    Variable                           Value
        ----    -------------------------------    ------------------------------

           1    DATECREATED                        08DEC16
           2    PERIOD                             5yr
           3    SUMLEV                             860
           4    VINTAGE                            2015


     
    DROP These These char variables have equal values for all observations

    BATCH

    Variables with All Equal Values

    Variable  Type  Len   Compare   Len   Label                        Compare Label

    COUNTY    CHAR    5   FIPCO       5                                Primary County
    ZCTA5     CHAR    5   ESRIID      5   ZIP Census Tabulation Area



    DROP ONE OF THESE
     
                          Correlated                Correlation    Number    Spearman
    Variable               With                          Coef       of Obs       P

    FAMILIES               FAMHHS                      1.00000       32989     <.0001
    PCTFAMILIES            PCTFAMHHS                   1.00000       32421     <.0001
    HOUSEHOLDER            TOTHHS                      1.00000       32989     <.0001
    BORNOUTSIDEUSNATIVE    BORNABROAD                  1.00000       32989     <.0001
    OCCHUS                 TOTHHS                      1.00000       32989     <.0001
    OCCHUS                 HOUSEHOLDER                 1.00000       32989     <.0001
    FBMINUSSEA             FOREIGNBORN                 1.00000       32989     0.2768
    NONINSTOVER65          POVUNIVERSEOVER65           1.00000       32989     <.0001
    PCTFBMINUSSEA          PCTFOREIGNBORN              1.00000       32680     <.0001





    Middle Observation(16494 ) of lup.mo_zcta2015cut - Total Obs 32,989


     -- CHARACTER --
    PERIOD                           C3       5yr                 PERIOD
    SUMLEV                           C3       860                 SUMLEV
    VINTAGE                          C4       2015                VINTAGE
    COUNTY                           C5       26097               COUNTY
    STATE                            C2       26                  Primary state
    ZCTA5                            C5       49838               ZIP Census Tabulation Area
    FIPCO                            C5       26097               Primary County
    FIPCO2                           C5                           FIPCO2
    ALTZIPS                          C71                          Alternate ZIP codes associated with this ZCTA
    STAB                             C2       MI                  STAB
    ESRIID                           C5       49838               ESRIID
    GEOID                            C12      86000US49838        GEOID
    TOTOBS                           C16      32,989              TOTOBS


     -- NUMERIC --
    DATECREATED                      N4       2079600             DATECREATED
    PCTCNTY                          N3       10000               PCTCNTY
    PCTSTATE                         N3       10000               % of ZCTAs total pop in primary State
    NALTZIPS                         N3       0                   # of alternative ZIP codes
    INTPTLAT                         N3       4600                INTPTLAT
    INTPTLON                         N3       -8600               INTPTLON
    LANDSQMI                         N4       13900               Land Area Sq. Miles
    AREASQMI                         N4       14500               Total Area Sq Mls
    TOTPOPSF1                        N5       50400               2010 Census Total Pop count
    LOGRECNO                         N4       2711500             LOGRECNO
    TOTPOP                           N5       55800               Total population
    AGE0_4                           N4       2000                Under 5 years
    PCTAGE0_4                        N3       400                 pctAge0_4
    AGE5_9                           N4       3100                5 to 9 years
    PCTAGE5_9                        N3       600                 pctAge5_9
    AGE10_14                         N4       4200                10 to 14 years
    PCTAGE10_14                      N3       800                 pctAge10_14
    AGE15_19                         N4       2700                15 to 19 years
    PCTAGE15_19                      N3       500                 pctAge15_19
    AGE20_24                         N4       1600                20 to 24 years
    PCTAGE20_24                      N3       300                 pctAge20_24
    AGE25_34                         N4       5400                25 to 34 years
    PCTAGE25_34                      N3       1000                pctAge25_34
    AGE35_44                         N4       4600                35 to 44 years
    PCTAGE35_44                      N3       800                 pctAge35_44
    AGE45_54                         N4       7000                45 to 54 years
    PCTAGE45_54                      N3       1300                pctAge45_54
    AGE55_59                         N4       4100                55 to 59 years
    PCTAGE55_59                      N3       700                 pctAge55_59
    AGE60_64                         N4       6500                60 to 64 years
    PCTAGE60_64                      N3       1200                pctAge60_64
    AGE65_74                         N4       10900               65 to 74 years
    PCTAGE65_74                      N3       2000                pctAge65_74
    AGE75_84                         N4       2500                75 to 84 years
    PCTAGE75_84                      N3       500                 pctAge75_84
    OVER85                           N4       1200                85 years and over
    PCTOVER85                        N3       200                 pctOver85
    MEDIANAGE                        N3       5300                Median age in years
    OVER5                            N5       53800               5 years and over
    PCTOVER5                         N3       9600                pctOver5
    OVER15                           N5       46500               15 years and over
    PCTOVER15                        N3       8300                pctOver15
    UNDER18                          N4       11100               Under 18 years of age
    PCTUNDER18                       N3       2000                pctUnder18
    OVER18                           N4       44700               18 years and over
    PCTOVER18                        N3       8000                pctOver18
    OVER21                           N4       43500               21 years and over
    PCTOVER21                        N3       7800                pctOver21
    OVER25                           N4       42200               25 years and over
    PCTOVER25                        N3       7600                pctOver25
    OVER62                           N4       18500               62 years and over
    PCTOVER62                        N3       3300                pctOver62
    OVER65                           N4       14600               65 years and over
    PCTOVER65                        N3       2600                pctOver65
    MALES                            N4       28400               Male
    PCTMALES                         N3       5100                pctMales
    OVER18MALES                      N4       21600               18 years old and over
    PCTOVER18MALES                   N3       7600                pctOver18Males
    OVER65MALES                      N4       7900                65 years old and over
    PCTOVER65MALES                   N3       2800                pctOver65Males
    FEMALES                          N4       27400               Female
    PCTFEMALES                       N3       4900                pctFemales
    OVER18FEMALES                    N4       23100               18 years old and over
    PCTOVER18FEMALES                 N3       8400                pctOver18Females
    OVER65FEMALES                    N4       6700                65 years old and over
    PCTOVER65FEMALES                 N3       2500                pctOver65Females
    ONERACE                          N5       52100               One race
    PCTONERACE                       N3       9300                pctOneRace
    WHITE1                           N5       46900               White alone
    PCTWHITE1                        N3       8400                pctWhite1
    BLACK1                           N5       100                 Black or African American
    PCTBLACK1                        N3       0                   pctBlack1
    INDIAN1                          N4       5100                American Indian and Alaska Native
    PCTINDIAN1                       N3       900                 pctIndian1
    ASIAN1                           N4       0                   Asian
    PCTASIAN1                        N3       0                   pctAsian1
    HAWNPI1                          N4       0                   Native Hawaiian and Other Pacific Islander
    PCTHAWNPI1                       N3       0                   pctHawnPI1
    OTHER1                           N4       0                   Some other race
    PCTOTHER1                        N3       0                   pctOther1
    MULTIRACE                        N4       3700                Two or more races
    PCTMULTIRACE                     N3       700                 pctMultiRace
    WHITE2                           N5       50200               White (alone or in combination)
    PCTWHITE2                        N3       9000                pctWhite2
    BLACK2                           N4       1200                Black (alone or in combination)
    PCTBLACK2                        N3       200                 pctBlack2
    INDIAN2                          N4       8100                American Indian (alone or in combination)
    PCTINDIAN2                       N3       1500                pctIndian2
    ASIAN2                           N4       0                   Asian (alone or in combination)
    PCTASIAN2                        N3       0                   pctAsian2
    HAWNPI2                          N4       0                   Native Hawaiian (alone or in combination)
    PCTHAWNPI2                       N3       0                   pctHawnPI2
    OTHER2                           N4       0                   Some other race (alone or in combination)
    PCTOTHER2                        N3       0                   pctOther2
    HISPANICPOP                      N5       600                 Hispanic or Latino of any race
    PCTHISPANICPOP                   N3       100                 pctHispanicPop
    NONHISPPOP                       N5       55200               Not Hispanic or Latino
    PCTNONHISPPOP                    N3       9900                pctNonHispPop
    NONHISPWHITE                     N4       46900               White alone
    PCTNONHISPWHITE                  N3       8400                pctNonHispWhite
    NONHISPBLACK                     N4       0                   Black or African American alone
    PCTNONHISPBLACK                  N3       0                   pctNonHispBlack
    NONHISPAMIND                     N4       5100                American Indian and Alaska Native alone
    PCTNONHISPAMIND                  N3       900                 pctNonHispAmInd
    NONHISPASIAN                     N4       0                   Asian alone
    PCTNONHISPASIAN                  N3       0                   pctNonHispAsian
    NONHISPHAWNPI                    N4       0                   Native Hawaiian and Other Pacific Islander alone
    PCTNONHISPHAWNPI                 N3       0                   pctNonHispHawnPI
    TOTHHS                           N4       24700               Total households
    HHINC0                           N4       3600                Less than $10,000
    PCTHHINC0                        N3       1500                pctHHInc0
    HHINC10                          N4       800                 $10,000 to $14,999
    PCTHHINC10                       N3       300                 pctHHInc10
    HHINC15                          N4       5700                $15,000 to $24,999
    PCTHHINC15                       N3       2300                pctHHInc15
    HHINC25                          N4       4500                $25,000 to $34,999
    PCTHHINC25                       N3       1800                pctHHInc25
    HHINC35                          N4       3800                $35,000 to $49,999
    PCTHHINC35                       N3       1500                pctHHInc35
    HHINC50                          N4       3400                $50,000 to $74,999
    PCTHHINC50                       N3       1400                pctHHInc50
    HHINC75                          N4       1100                $75,000 to $99,999
    PCTHHINC75                       N3       500                 pctHHInc75
    HHINC100                         N4       1500                $100,000 to $149,999
    PCTHHINC100                      N3       600                 pctHHInc100
    HHINC150                         N4       0                   $150,000 to $199,999
    PCTHHINC150                      N3       0                   pctHHInc150
    HHINC200                         N4       300                 $200,000 or more
    PCTHHINC200                      N3       100                 pctHHInc200
    NUMHHEARNINGS                    N4       10900               With earnings
    PCTNUMHHEARNINGS                 N3       4400                pctNumHHEarnings
    NUMHHSOCSEC                      N4       11700               With social security
    PCTNUMHHSOCSEC                   N3       4700                pctNumHHSocSec
    NUMHHRETINC                      N4       10100               With retirement income
    PCTNUMHHRETINC                   N3       4100                pctNumHHRetInc
    NUMHHSUPPSECINC                  N4       2900                With supplemental security income
    PCTNUMHHSUPPSECINC               N3       1200                pctNumHHSuppSecInc
    NUMHHPUBASSIST                   N4       1500                With cash public assistance income
    PCTNUMHHPUBASSIST                N3       600                 pctNumHHPubAssist
    NUMHHFOODSTMP                    N4       7600                With food stamp benefits in the past 12 months
    PCTNUMHHFOODSTMP                 N3       3100                pctNumHHFoodStmp
    MEDIANHHINC                      N5       3086500             Median household income
    AVGHHINC                         N5       3926500             Mean household income
    AVGHHEARNINGS                    N5       3223200             Mean household earnings
    AVGHHSOCSEC                      N4       1812400             Mean household social security income
    AVGHHRETINC                      N5       2309800             Mean household retirement income
    AVGHHSUPPSECINC                  N4       955500              Mean household supplemental security income
    AVGHHPUBASSIST                   N4       532000              Mean household cash public assistance income
    FAMHHS                           N4       19300               Family households
    PCTFAMHHS                        N3       7800                pctFamHHs
    FAMHHINC0                        N4       3000                Less than $10,000
    PCTFAMHHINC0                     N3       1600                pctFamHHInc0
    FAMHHINC10                       N4       200                 $10,000 to $14,999
    PCTFAMHHINC10                    N3       100                 pctFamHHInc10
    FAMHHINC15                       N4       3900                $15,000 to $24,999
    PCTFAMHHINC15                    N3       2000                pctFamHHInc15
    FAMHHINC25                       N4       3900                $25,000 to $34,999
    PCTFAMHHINC25                    N3       2000                pctFamHHInc25
    FAMHHINC35                       N4       2600                $35,000 to $49,999
    PCTFAMHHINC35                    N3       1400                pctFamHHInc35
    FAMHHINC50                       N4       3000                $50,000 to $74,999
    PCTFAMHHINC50                    N3       1600                pctFamHHInc50
    FAMHHINC75                       N4       900                 $75,000 to $99,999
    PCTFAMHHINC75                    N3       500                 pctFamHHInc75
    FAMHHINC100                      N4       1500                $100,000 to $149,999
    PCTFAMHHINC100                   N3       800                 pctFamHHInc100
    FAMHHINC150                      N4       0                   $150,000 to $199,999
    PCTFAMHHINC150                   N3       0                   pctFamHHInc150
    FAMHHINC200                      N4       300                 $200,000 or more
    PCTFAMHHINC200                   N3       200                 pctFamHHInc200
    MEDIANFAMINC                     N5       3258900             Median family income
    AVGFAMINC                        N5       4276400             Mean family income
    PCI                              N5       1737200             Per-capita income
    NONFAMHHS                        N4       5400                Nonfamily households
    PCTNONFAMHHS                     N3       2200                pctNonFamHHs
    MEDIANNONFAMINC                  N5       1900000             Median nonfamily income
    AVGNONFAMINC                     N5       2450600             Mean nonfamily income
    FULLTIMEWORKERS                  N4       7300                All full-time workers
    FULLTIMEWORKERSMALE              N4       2700                All male full-time workers
    PCTFULLTIMEWORKERSMALE           N3       3700                pctFullTimeWorkersMale
    FULLTIMEWORKERSFEMALE            N4       4600                All female full-time workers
    PCTFULLTIMEWORKERSFEMALE         N3       6300                pctFullTimeWorkersFemale
    MEDIANEARNINGS                   N5       .                   Median earnings for workers
    MEDIANEARNINGSMALE               N5       .                   Median earnings for male full-time, year-round workers
    MEDIANEARNINGSFEMALE             N5       .                   Median earnings for female full-time, year-round workers
    POVUNIVERSE                      N5       55800               Persons for whom poverty status is determined
    POOR                             N4       16100               Persons below poverty
    PCTPOOR                          N3       2900                pctPoor
    POVUNIVERSEUNDER18               N4       11100               Persons under 18 for whom poverty status is determined
    POORUNDER18                      N4       4900                Persons under 18 in poverty
    PCTPOORUNDER18                   N3       4400                pctPoorUnder18
    POVUNIVERSE18_64                 N4       30100               Persons aged 18 to 64 for whom poverty status is determined
    POOR18TO64                       N4       10000               Persons aged 18 to 64 in poverty
    PCTPOOR18TO64                    N3       3300                pctPoor18to64
    POVUNIVERSEOVER65                N4       14600               Persons over 65 for whom poverty status is determined
    POOROVER65                       N4       1200                Persons over 65 in poverty
    PCTPOOROVER65                    N3       800                 pctPoorOver65
    POVUNIVERSEFAMILYPOP             N5       48100               Persons in families for whom poverty status is determined
    PCTFAMILYPOPPOOR                 N3       8600                pctFamilyPopPoor
    POVUNIVERSEUNRELATED             N4       7700                Unrelated individuals for whom poverty status is determined
    POORINFAMILY                     N4       11900               Persons in families in poverty
    PCTPOORINFAMILY                  N3       2500                pctPoorInFamily
    POORFAMILIES                     N4       4400                Family households in poverty
    PCTPOORFAMILIES                  N3       2300                pctPoorFamilies
    POORUNRELATED                    N4       4200                Unrelated persons in poverty 15 years and over
    PCTPOORUNRELATED                 N3       5500                pctPoorUnrelated
    POVRATIOUNDERHALF                N4       9100                Poverty ratio under 0.5
    PCTPOVRATIOUNDERHALF             N3       1600                pctPovRatioUnderHalf
    POVRATIOV5TOV99                  N4       7000                Poverty ratio in 0.5 to 0.99
    PCTPOVRATIOV5TOV99               N3       1300                pctPovRatiov5tov99
    POVRATIO1TO2                     N4       16500               Poverty ratio in 1 to 2
    PCTPOVRATIO1TO2                  N3       3000                pctPovRatio1to2
    POVRATIOOVER2                    N4       23200               Poverty ratio in 2 and over
    PCTPOVRATIOOVER2                 N3       4200                pctPovRatioOver2
    OVER16                           N5       46300               Population 16 years and over
    LABORFORCE                       N4       17500               In labor force
    PCTLABORFORCE                    N3       3800                pctLaborForce
    CIVLABFORCE                      N4       17500               Civilian labor force
    PCTCIVLABFORCE                   N3       3800                pctCivLabForce
    EMPLOYEDCLF                      N4       15500               Employed civilians
    PCTEMPLOYEDCLF                   N3       8900                pctEmployedCLF
    UNEMPLOYEDCLF                    N4       2000                Unemployed civilians
    PCTUNEMPLOYEDCLF                 N3       1100                pctUnemployedCLF
    MILITARY                         N4       0                   In military
    PCTMILITARY                      N3       0                   pctMilitary
    NOTINLF                          N4       28800               Not in labor force
    PCTNOTINLF                       N3       6200                pctNotInLF
    OVER16FEMALES                    N4       23300               Females 16 years and over
    PCTOVER16FEMALES                 N3       5000                pctOver16Females
    LABORFORCEFEMALES                N4       9700                Females in labor force
    PCTLABORFORCEFEMALES             N3       2100                pctLaborForceFemales
    CIVLABFORCEFEMALES               N4       9700                Females in civilian labor force
    PCTCIVLABFORCEFEMALES            N3       2100                pctCivLabForceFemales
    EMPLOYEDFEMALES                  N4       7700                Employed females
    PCTEMPLOYEDFEMALES               N3       7900                pctEmployedFemales
    OWNKIDSUNDER6                    N4       2300                (Own) children under 6
    OWNKIDSU6ALLPRNTSWK              N4       1000                All parents working
    PCTOWNKIDSU6ALLPRNTSWK           N3       4400                pctOwnKidsU6AllPrntsWk
    OWNKIDSOVER6                     N4       8500                (Own) children aged 6 to 17
    OWNKIDSO6ALLPRNTSWK              N4       5200                All parents working
    PCTOWNKIDSO6ALLPRNTSWK           N3       6100                pctOwnKidsO6AllPrntsWk
    WORKER16                         N4       15500               Workers 16 years and over
    COMMUTERS                        N4       14400               Workers 16+ who commute to work
    PCTCOMMUTERS                     N3       9300                pctCommuters
    DRIVEALONE                       N4       12200               Car, truck, or van; drove alone
    PCTDRIVEALONE                    N3       7900                pctDriveAlone
    CARPOOL                          N4       1900                Car, truck, or van; carpooled
    PCTCARPOOL                       N3       1200                pctCarpool
    PUBLICTRANS                      N4       0                   Public transportation (excluding taxicab)
    PCTPUBLICTRANS                   N3       0                   pctPublicTrans
    WALKTOWORK                       N4       0                   Walked to work
    PCTWALKTOWORK                    N3       0                   pctWalkToWork
    OTHERCOMMUTE                     N4       300                 Other means of commuting
    PCTOTHERCOMMUTE                  N3       200                 pctOtherCommute
    WORKATHOME                       N4       1100                Worked at home
    PCTWORKATHOME                    N3       700                 pctWorkAtHome
    AVGCOMMUTE                       N3       3200                Mean travel time to work in minutes
    MANPROFOCCS                      N4       2900                Management, business, science, and arts occupations
    PCTMANPROFOCCS                   N3       1900                pctManProfOccs
    SERVICEOCCS                      N4       3100                Service occupations
    PCTSERVICEOCCS                   N3       2000                pctServiceOccs
    SALESOFFOCCS                     N4       2700                Sales and office occupations
    PCTSALESOFFOCCS                  N3       1700                pctSalesOffOccs
    FARMFISHOCCS                     N4       1000                Farming, fishing, and forestry occupations
    PCTFARMFISHOCCS                  N3       700                 pctFarmFishOccs
    CONSOCCS                         N4       1700                Construction, extraction, installation, maintenance, and repair occupations
    PCTCONSOCCS                      N3       1100                pctConsOccs
    TRANSOCCS                        N4       4100                Production, transportation, and material moving occupations
    PCTTRANSOCCS                     N3       2700                pctTransOccs
    AGRICULTURE                      N4       1200                Agriculture, forestry, fishing and hunting, and mining
    PCTAGRICULTURE                   N3       800                 pctAgriculture
    CONSTRUCTION                     N4       1700                Construction
    PCTCONSTRUCTION                  N3       1100                pctConstruction
    MANUFACTURING                    N4       2500                Manufacturing
    PCTMANUFACTURING                 N3       1600                pctManufacturing
    WHOLESALETRADE                   N4       0                   Wholesale trade
    PCTWHOLESALETRADE                N3       0                   pctWholesaleTrade
    RETAILTRADE                      N4       500                 Retail trade
    PCTRETAILTRADE                   N3       300                 pctRetailTrade
    TRANSPORTATION                   N4       500                 Transportation and warehousing, and utilities
    PCTTRANSPORTATION                N3       300                 pctTransportation
    INFORMATION                      N4       0                   Information
    PCTINFORMATION                   N3       0                   pctInformation
    FINANCE_INS                      N4       1400                Finance and insurance, and real estate and rental and leasing
    PCTFINANCE_INS                   N3       900                 pctFinance_Ins
    PROFESSIONAL                     N4       200                 Professional, scientific, management, and administrative
    PCTPROFESSIONAL                  N3       100                 pctProfessional
    EDUC_HEALTH_SOCSVCS              N4       3000                Educational services, and health care and social assistance
    PCTEDUC_HEALTH_SOCSVCS           N3       1900                pctEduc_Health_SocSvcs
    RECREATIONETC                    N4       3000                Arts, entertainment, and recreation, and accommodation and food services
    PCTRECREATIONETC                 N3       1900                pctRecreationEtc
    OTHERINDUSTRIES                  N4       300                 Other services, except public administration
    PCTOTHERINDUSTRIES               N3       200                 pctOtherIndustries
    PUBLICADMIN                      N4       1200                Public administration
    PCTPUBLICADMIN                   N3       800                 pctPublicAdmin
    PRIVWAGEWORKERS                  N4       11600               Private wage and salary workers
    PCTPRIVWAGEWORKERS               N3       7500                pctPrivWageWorkers
    GOVWORKERS                       N4       3600                Government workers
    PCTGOVWORKERS                    N3       2300                pctGovWorkers
    SELFEMPWORKERS                   N4       300                 Self-employed workers in own not incorporated business
    PCTSELFEMPWORKERS                N3       200                 pctSelfEmpWorkers
    UNPAIDFAMWORKERS                 N3       0                   Unpaid family workers
    PCTUNPAIDFAMWORKERS              N3       0                   pctUnpaidFamWorkers
    TOTPOPNONINST                    N5       55800               Civilian noninstitutionalized persons
    NONINSTUNDER65                   N5       41200               Under 65 years of age
    PCTNONINSTUNDER65                N3       7400                pctNonInstUnder65
    NOTINSUREDUNDER65                N4       3300                Without insurance coverage
    PCTNOTINSUREDUNDER65             N3       800                 pctNotInsuredUnder65
    PUBLICINSURANCEUNDER65           N4       21400               With public insurance
    PCTPUBLICINSURANCEUNDER65        N3       5200                pctPublicInsuranceUnder65
    PRIVATEINSURANCEONLYUNDER65      N4       19800               With private insurance
    PCTPRIVATEINSURANCEONLYUNDER65   N3       4800                pctPrivateInsuranceOnlyUnder65
    NONINSTUNDER18                   N4       11100               Under 18 years of age
    PCTNONINSTUNDER18                N3       2000                pctNonInstUnder18
    NOTINSUREDUNDER18                N4       700                 Without insurance coverage
    FAMILIES                         N4       19300               Family households
    PCTFAMILIES                      N3       7800                pctFamilies
    FAMSWITHKIDS                     N4       5400                With own children under 18 years
    PCTFAMSWITHKIDS                  N3       2200                pctFamsWithKids
    MARRIEDCOUPLES                   N4       16400               Married-couple families
    PCTMARRIEDCOUPLES                N3       6600                pctMarriedCouples
    COUPLESWITHKIDS                  N4       3400                With own children under 18 years
    PCTCOUPLESWITHKIDS               N3       1400                pctCouplesWithKids
    SINGLEMALEFAMILIES               N4       600                 Male householder, no wife present
    PCTSINGLEMALEFAMILIES            N3       200                 pctSingleMaleFamilies
    SINGLEFATHERS                    N4       0                   With own children under 18 years
    PCTSINGLEFATHERS                 N3       0                   pctSingleFathers
    SINGLEFEMALEFAMILIES             N4       2300                Female householder, no husband present
    PCTSINGLEFEMALEFAMILIES          N3       900                 pctSingleFemaleFamilies
    SINGLEMOTHERS                    N4       2000                With own children under 18 years
    PCTSINGLEMOTHERS                 N3       800                 pctSingleMothers
    LIVINGALONE                      N4       4800                Householder living alone
    PCTLIVINGALONE                   N3       8900                pctLivingAlone
    OVER65ALONE                      N4       1800                65 years and over living alone
    PCTOVER65ALONE                   N3       3300                pctOver65Alone
    HHSWITHKIDS                      N4       6300                Households with one or more people under 18 years
    PCTHHSWITHKIDS                   N3       2600                pctHHsWithKids
    HHSWITHELDERS                    N4       9600                Households with one or more people 65 years and over
    PCTHHSWITHELDERS                 N3       3900                pctHHsWithElders
    AVGHHSIZE                        N3       200                 Average household size
    AVGFAMSIZE                       N3       200                 Average family size
    HHPOP                            N5       55800               Household population
    PCTHHPOP                         N3       10000               pctHHPop
    FAMHHPOP                         N5       49900               Living in family households
    PCTFAMHHPOP                      N3       8900                pctFamHHpop
    NONFAMHHPOP                      N4       5900                Living in nonfamily households
    PCTNONFAMHHPOP                   N3       1100                pctNonFamHHpop
    GRPQUARTERS                      N4       0                   Living in group quarters
    PCTGRPQUARTERS                   N3       0                   pctGrpQuarters
    HOUSEHOLDER                      N4       24700               Householder
    PCTHOUSEHOLDER                   N3       4400                pctHouseholder
    SPOUSE                           N4       15800               Spouse
    PCTSPOUSE                        N3       2800                pctSpouse
    CHILD                            N4       11200               Child
    PCTCHILD                         N3       2000                pctChild
    OTHERRELATIVE                    N4       1800                Other relatives
    PCTOTHERRELATIVE                 N3       300                 pctOtherRelative
    NONRELATIVE                      N4       2300                Nonrelatives
    PCTNONRELATIVE                   N3       400                 pctNonRelative
    UNMARRIEDPARTNER                 N4       2300                Unmarried partner
    PCTUNMARRIEDPARTNER              N3       400                 pctUnmarriedPartner
    UMPARTNERHHSPERK                 N4       9300                Unmarried-partner HHs per 1000
    NEVERMARRIED                     N4       6700                Never married
    PCTNEVERMARRIED                  N3       1400                pctNeverMarried
    MARRIED                          N4       32500               Now married, except separated
    PCTMARRIED                       N3       7000                pctMarried
    SEPARATED                        N4       0                   Separated
    PCTSEPARATED                     N3       0                   pctSeparated
    WIDOWED                          N4       3700                Widowed
    PCTWIDOWED                       N3       800                 pctWidowed
    DIVORCED                         N4       3600                Divorced, and not currently married
    PCTDIVORCED                      N3       800                 pctDivorced
    WOMEN15TO50                      N4       8700                Women 15 to 50 years old
    UNMARRIEDWOMEN15TO50             N4       3700                Unmarried women 15 to 50 years old
    PCTUNMARRIEDWOMEN15TO50          N3       4300                pctUnmarriedWomen15to50
    WOMENGIVINGBIRTH                 N4       0                   Women 15 to 50 years old who had a birth in the past 12 months
    PCTWOMENGIVINGBIRTH              N3       0                   pctWomenGivingBirth
    UNMARRIEDGIVINGBIRTH             N4       0                   Unmarried women who gave birth
    PCTUNMARRIEDGIVINGBIRTH          N3       .                   pctUnmarriedGivingBirth
    WOMEN15TO19                      N3       0                   Women 15 to 19 years of age
    PCTWOMEN15TO19                   N3       .                   pctWomen15to19
    WOMEN20TO34                      N4       0                   Women 20 to 34 years of age
    PCTWOMEN20TO34                   N3       .                   pctWomen20to34
    WOMEN35TO50                      N4       0                   Women 35 to 50 years of age
    PCTWOMEN35TO50                   N3       .                   pctWomen35to50
    UNMARRIEDGIVINGBIRTHPERK         N4       0                   Per 1,000 unmarried women
    GIVINGBIRTHPERK                  N4       0                   Per 1,000 women 15 to 50 years old
    BIRTHRATE15_19                   N4       0                   Per 1,000 women 15 to 19 years old
    BIRTHRATE20_34                   N4       0                   Per 1,000 women 20 to 34 years old
    BIRTHRATE35_50                   N4       0                   Per 1,000 women 35 to 50 years old
    GRANDPRNTSLVNGWITHGRNDKID        N4       1200                Grandparents living with own grandchildren under 18 years
    GRANDPRNTSCARING                 N4       1100                Grandparents responsible for grandchildren
    PCTGRANDPRNTSCARING              N3       9200                pctGrandPrntsCaring
    GRANDPRNTSCARINGLT1              N4       700                 Less than 1 year
    PCTGRANDPRNTSCARINGLT1           N3       6400                pctGrandPrntsCaringLT1
    GRANDPRNTSCARING1OR2             N4       0                   1 or 2 years
    PCTGRANDPRNTSCARING1OR2          N3       0                   pctGrandPrntsCaring1or2
    GRANDPRNTSCARING3OR4             N4       0                   3 or 4 years
    PCTGRANDPRNTSCARING3OR4          N3       0                   pctGrandPrntsCaring3or4
    GRANDPRNTSCARING5ORMORE          N4       400                 5 or more years
    PCTGRANDPRNTSCARING5ORMORE       N3       3600                pctGrandPrntsCaring5orMore
    OVER3                            N5       53800               Population 3 years and over
    ENROLLEDOVER3                    N4       9800                Population 3 years and over enrolled in school
    PCTENROLLEDOVER3                 N3       1800                pctEnrolledOver3
    INNURSERY                        N4       0                   In nursery school, preschool
    PCTINNURSERY                     N3       0                   pctInNursery
    INKINDERGARTEN                   N4       300                 In kindergarten
    PCTINKINDERGARTEN                N3       300                 pctInKindergarten
    INELEMENTARY                     N4       6100                In elementary school, grades 1-8
    PCTINELEMENTARY                  N3       6200                pctInElementary
    INHIGHSCHOOL                     N4       2600                In high school, grades 9-12
    PCTINHIGHSCHOOL                  N3       2700                pctInHighSchool
    INCOLLEGE                        N4       800                 In college or graduate school
    PCTINCOLLEGE                     N3       800                 pctInCollege
    LESSTHAN9TH                      N4       1200                Less than 9th grade
    PCTLESSTHAN9TH                   N3       300                 pctLessThan9th
    SOMEHIGHSCHOOL                   N4       6300                9th to 12th grade, no diploma
    PCTSOMEHIGHSCHOOL                N3       1500                pctSomeHighSchool
    HIGHSCHOOL                       N4       20400               High school graduate (includes equivalency)
    PCTHIGHSCHOOL                    N3       4800                pctHighSchool
    SOMECOLLEGE                      N4       8500                Some college, no degree
    PCTSOMECOLLEGE                   N3       2000                pctSomeCollege
    ASSOCIATES                       N4       2200                Associates degree
    PCTASSOCIATES                    N3       500                 pctAssociates
    BACHELORS                        N4       1200                Bachelors degree
    PCTBACHELORS                     N3       300                 pctBachelors
    GRADPROF                         N4       2400                Graduate or professional degree
    PCTGRADPROF                      N3       600                 pctGradProf
    HIGHSCHOOLORMORE                 N4       34700               High school graduate or higher
    PCTHIGHSCHOOLORMORE              N3       8200                pctHighSchoolOrMore
    BACHELORSORMORE                  N4       3600                Bachelor degree or higher
    PCTBACHELORSORMORE               N3       900                 pctBachelorsormore
    CIVILIAN                         N4       44700               Civilian population 18 years and over
    PCTCIVILIAN                      N3       10000               pctCivilian
    VETERAN                          N4       4200                Civilian veterans
    PCTVETERAN                       N3       900                 pctVeteran
    DISABLED                         N4       13200               With a disability
    PCTDISABLED                      N3       2400                pctDisabled
    DISABLEDUNDER18                  N4       900                 With a disability
    PCTDISABLEDUNDER18               N3       800                 pctDisabledUnder18
    NONINST18_64                     N4       30100               Persons 18 to 64 years
    PCTNONINST18_64                  N3       5400                pctNonInst18_64
    DISABLED18_64                    N4       7100                With a disability
    PCTDISABLED18_64                 N3       2400                pctDisabled18_64
    NONINSTOVER65                    N4       14600               Persons 65 years and over
    PCTNONINSTOVER65                 N3       2600                pctNonInstOver65
    DISABLEDELDER                    N4       5200                With a disability
    PCTDISABLEDELDER                 N3       3600                pctDisabledElder
    OVER1                            N5       55800               Population 1 year and over
    SAMEHOUSE                        N5       45300               Same house
    PCTSAMEHOUSE                     N3       8100                pctSameHouse
    DIFFHOUSE                        N4       10500               Different house in the U.S.
    PCTDIFFHOUSE                     N3       1900                pctDiffHouse
    DIFFHOUSESAMECOUNTY              N4       7200                Same county
    PCTDIFFHOUSESAMECOUNTY           N3       6900                pctDiffHouseSameCounty
    DIFFCOUNTY                       N4       3300                Different county (any)
    PCTDIFFCOUNTY                    N3       3100                pctDiffCounty
    DIFFCNTYSAMESTATE                N4       3300                Different county (same state)
    PCTDIFFCNTYSAMESTATE             N3       3100                pctDiffCntySameState
    DIFFSTATE                        N4       0                   Different state
    PCTDIFFSTATE                     N3       0                   pctDiffState
    LIVEDABROAD                      N4       0                   Lived abroad 1 year ago
    PCTLIVEDABROAD                   N3       0                   pctLivedAbroad
    USNATIVE                         N5       55600               U.S. native
    PCTUSNATIVE                      N3       10000               pctUSNative
    BORNINUS                         N4       55100               Born in United States
    PCTBORNINUS                      N3       9900                pctBornInUS
    BORNINCURRSTATE                  N4       47600               Born in state of current residence
    PCTBORNINCURRSTATE               N3       8600                pctBornInCurrState
    BORNINDIFFSTATE                  N4       7500                Born in different state than current residence
    PCTBORNINDIFFSTATE               N3       1400                pctBornInDiffState
    BORNABROAD                       N4       500                 Born in Puerto Rico, U.S. Island areas, or born abroad to American parents
    PCTBORNABROAD                    N3       100                 pctBornAbroad
    FOREIGNBORN                      N4       200                 Foreign born
    PCTFOREIGNBORN                   N3       0                   pctForeignBorn
    NATURALIZED                      N4       200                 Naturalized U.S. citizen
    PCTNATURALIZED                   N3       10000               pctNaturalized
    NONCITIZEN                       N4       0                   Not a U.S. citizen
    PCTNONCITIZEN                    N3       0                   pctNonCitizen
    BORNOUTSIDEUS                    N4       700                 Population born outside the United States
    PCTBORNOUTSIDEUS                 N3       100                 pctBornOutsideUS
    BORNOUTSIDEUSNATIVE              N4       500                 Native, born outside U.S.
    PCTBORNOUTSIDEUSNATIVE           N3       7100                pctBornOutsideUSNative
    NATIVEENTEREDGE2000              N4       0                   Entered U.S. 2000 or later
    PCTNATIVEENTEREDGE2000           N4       0                   pctNativeEnteredGE2000
    NATIVEENTEREDLT2000              N4       500                 Entered U.S. before 2000
    PCTNATIVEENTEREDLT2000           N3       10000               pctNativeEnteredLT2000
    FBENTEREDGE2000                  N4       0                   Entered U.S. 2000 or later
    PCTFBENTEREDGE2000               N3       0                   pctFBEnteredGE2000
    FBENTEREDLT2000                  N4       200                 Entered U.S. before 2000
    PCTFBENTEREDLT2000               N3       10000               pctFBEnteredLT2000
    FBMINUSSEA                       N4       200                 Foreign-born population, excluding population born at sea
    PCTFBMINUSSEA                    N3       0                   pctFBMinusSea
    FBEUROPE                         N4       200                 Europe
    PCTFBEUROPE                      N3       10000               pctFBEurope
    FBASIA                           N4       0                   Asia
    PCTFBASIA                        N3       0                   pctFBAsia
    FBAFRICA                         N4       0                   Africa
    PCTFBAFRICA                      N3       0                   pctFBAfrica
    FBOCEANIA                        N4       0                   Oceania
    PCTFBOCEANIA                     N3       0                   pctFBOceania
    FBLATINAMERICA                   N4       0                   South and Central America (includes Mexico)
    PCTFBLATINAMERICA                N3       0                   pctFBLatinAmerica
    FBNORTHAMERICA                   N4       0                   Northern America
    PCTFBNORTHAMERICA                N3       0                   pctFBNorthAmerica
    ENGLISHONLY                      N4       53000               English only
    PCTENGLISHONLY                   N3       9900                pctEnglishOnly
    OTHLANG                          N5       800                 Language other than English
    PCTOTHLANG                       N3       200                 pctOthLang
    OTHLANGENGLISHLTD                N4       100                 Speaks English less than "very well"
    PCTOTHLANGENGLISHLTD             N3       1300                pctOthLangEnglishLTD
    SPANISH                          N4       700                 Speak Spanish
    PCTSPANISH                       N3       100                 pctSpanish
    SPANISHENGLISHLTD                N4       0                   Speaks English less than "very well"
    PCTSPANISHENGLISHLTD             N3       0                   pctSpanishEnglishLTD
    TOTHUS                           N4       65200               Total housing units
    OCCHUS                           N4       24700               Occupied housing units
    PCTOCCHUS                        N3       3800                pctOccHUs
    OWNEROCC                         N4       20500               Owner-occupied
    PCTOWNEROCC                      N3       8300                pctOwnerOcc
    RENTEROCC                        N4       4200                Renter-occupied
    PCTRENTEROCC                     N3       1700                pctRenterOcc
    AVGOWNERHHSIZE                   N3       200                 Average household size of owner-occupied unit
    AVGRENTERHHSIZE                  N3       300                 Average household size of renter-occupied unit
    VACHUS                           N4       40500               Vacant housing units
    PCTVACHUS                        N3       6200                pctVacHUs
    VACANTFORSALE                    N4       3200                For sale
    PCTVACANTFORSALE                 N3       800                 pctVacantForSale
    VACANTFORRENT                    N4       900                 For rent
    PCTVACANTFORRENT                 N3       200                 pctVacantForRent
    VACANTSEASONAL                   N4       32300               For seasonal, recreational, or occasional use
    PCTVACANTSEASONAL                N3       8000                pctVacantSeasonal
    TOTALOWNERUNITS                  N4       23700               Total owner units
    OWNERVACRATE                     N3       1400                Homeowner vacancy rate
    TOTALRENTALUNITS                 N4       5100                Total rental units
    RENTERVACRATE                    N3       1800                Rental vacancy rate
    PERSONSINOWNERUNITS              N4       42800               People living in owned homes
    PCTPERSONSINOWNERUNITS           N3       7700                pctPersonsInOwnerUnits
    PERSONSINRENTERUNITS             N5       13000               People living in rental homes
    PCTPERSONSINRENTERUNITS          N3       2300                pctPersonsInRenterUnits
    UNITS1                           N4       59100               Single-family units
    PCTUNITS1                        N3       9100                pctUnits1
    UNITS1DETACHED                   N4       59100               Single unit, detached
    PCTUNITS1DETACHED                N3       10000               pctUnits1Detached
    UNITS1ATTACHED                   N4       0                   Single unit, attached
    PCTUNITS1ATTACHED                N3       0                   pctUnits1Attached
    UNITS2                           N4       300                 Duplexes
    PCTUNITS2                        N3       100                 pctUnits2
    UNITS3_4                         N4       0                   3 or 4 units
    PCTUNITS3_4                      N3       0                   pctUnits3_4
    UNITS5_9                         N4       0                   5 to 9 units
    PCTUNITS5_9                      N3       0                   pctUnits5_9
    UNITS10_19                       N4       400                 10 to 19 units
    PCTUNITS10_19                    N3       100                 pctUnits10_19
    UNITS20UP                        N4       0                   20 or more units
    PCTUNITS20UP                     N3       0                   pctUnits20up
    MOBILEHOMES                      N4       5200                Mobile home
    PCTMOBILEHOMES                   N3       800                 pctMobileHomes
    BOATRV                           N4       200                 Boat, RV, van, etc.
    PCTBOATRV                        N3       0                   pctBoatRV
    MOBILEHOMESPERK                  N4       8000                Mobile homes per 1000 HUs
    BUILT2010ORLATER                 N4       1300                Built 2010 or later
    PCTBUILT2010ORLATER              N3       200                 pctBuilt2010orLater
    BUILT2000_2009                   N4       5300                Built 2000 to 2009
    PCTBUILT2000_2009                N3       800                 pctBuilt2000_2009
    BUILT1990_1999                   N4       15800               Built 1990 to 1999
    PCTBUILT1990_1999                N3       2400                pctBuilt1990_1999
    BUILT1980_1989                   N4       6300                Built 1980 to 1989
    PCTBUILT1980_1989                N3       1000                pctBuilt1980_1989
    BUILT1970_1979                   N4       7200                Built 1970 to 1979
    PCTBUILT1970_1979                N3       1100                pctBuilt1970_1979
    BUILT1960_1969                   N4       7400                Built 1960 to 1969
    PCTBUILT1960_1969                N3       1100                pctBuilt1960_1969
    BUILT1950_1959                   N4       5900                Built 1950 to 1959
    PCTBUILT1950_1959                N3       900                 pctBuilt1950_1959
    BUILT1940_1949                   N4       7700                Built 1940 to 1949
    PCTBUILT1940_1949                N3       1200                pctBuilt1940_1949
    BUILTBEFORE1940                  N4       8300                Built 1939 or earlier
    PCTBUILTBEFORE1940               N3       1300                pctBuiltBefore1940
    MOVEDIN2010ORLATER               N4       6000                Moved in 2010 or later
    PCTMOVEDIN2010ORLATER            N3       2400                pctMovedIn2010orLater
    MOVEDIN2000_2009                 N4       9500                Moved in 2000 to 2009
    PCTMOVEDIN2000_2009              N3       3900                pctMovedIn2000_2009
    MOVEDIN1990_1999                 N4       5000                Moved in 1990 to 1999
    PCTMOVEDIN1990_1999              N3       2000                pctMovedIn1990_1999
    MOVEDIN1980_1989                 N4       1800                Moved in 1980 to 1989
    PCTMOVEDIN1980_1989              N3       700                 pctMovedIn1980_1989
    MOVEDINBEFORE1980                N4       2400                Moved in 1979 or earlier
    PCTMOVEDINBEFORE1980             N3       1000                pctMovedInBefore1980
    NOVEHICLES                       N4       1400                No vehicles available
    PCTNOVEHICLES                    N3       600                 pctNoVehicles
    VEHICLES1                        N4       6100                1 vehicle available
    PCTVEHICLES1                     N3       2500                pctVehicles1
    VEHICLES2                        N4       14000               2 vehicles available
    PCTVEHICLES2                     N3       5700                pctVehicles2
    VEHICLESGE3                      N4       3200                3 or more vehicles available
    PCTVEHICLESGE3                   N3       1300                pctVehiclesGE3
    HHFUTILGAS                       N4       1500                Utility gas
    PCTHHFUTILGAS                    N3       600                 pctHHFUtilGas
    HHFLPGAS                         N4       12900               Bottled, tank, or LP gas
    PCTHHFLPGAS                      N3       5200                pctHHFLPGas
    HHFELECTRIC                      N4       2000                Electricity
    PCTHHFELECTRIC                   N3       800                 pctHHFElectric
    HHFKEROSENE                      N4       200                 Fuel oil, kerosene, etc.
    PCTHHFKEROSENE                   N3       100                 pctHHFKerosene
    HHFCOAL                          N4       0                   Coal or coke
    PCTHHFCOAL                       N3       0                   pctHHFCoal
    HHFWOOD                          N4       7700                Wood
    PCTHHFWOOD                       N3       3100                pctHHFWood
    HHFSOLAR                         N4       0                   Solar energy
    PCTHHFSOLAR                      N3       0                   pctHHFSolar
    HHFOTHER                         N4       400                 Other fuel
    PCTHHFOTHER                      N3       200                 pctHHFOther
    HHFNOFUEL                        N4       0                   No fuel used
    PCTHHFNOFUEL                     N3       0                   pctHHFNoFuel
    NOPLUMBING                       N4       200                 Lacking complete plumbing facilities
    PCTNOPLUMBING                    N3       100                 pctNoPlumbing
    NOKITCHEN                        N4       0                   Lacking complete kitchen facilities
    PCTNOKITCHEN                     N3       0                   pctNoKitchen
    NOPHONE                          N4       200                 No telephone service available
    PCTNOPHONE                       N3       100                 pctNoPhone
    PERSONSPERROOMLOW                N4       23300               1.00 or less
    PCTPERSONSPERROOMLOW             N3       9400                pctPersonsPerRoomLow
    PERSONSPERROOMMEDIUM             N4       1200                1.01 to 1.50
    PCTPERSONSPERROOMMEDIUM          N3       500                 pctPersonsPerRoomMedium
    PERSONSPERROOMHIGH               N4       200                 1.51 or more
    PCTPERSONSPERROOMHIGH            N3       100                 pctPersonsPerRoomHigh
    HVALUNDER50                      N4       2700                Less than $50,000
    PCTHVALUNDER50                   N3       1300                pctHvalUnder50
    HVAL50                           N4       4600                $50,000 to $99,999
    PCTHVAL50                        N3       2200                pctHval50
    HVAL100                          N4       3800                $100,000 to $149,999
    PCTHVAL100                       N3       1900                pctHval100
    HVAL150                          N4       1700                $150,000 to $199,999
    PCTHVAL150                       N3       800                 pctHval150
    HVAL200                          N4       6000                $200,000 to $299,999
    PCTHVAL200                       N3       2900                pctHval200
    HVAL300                          N4       1400                $300,000 to $499,999
    PCTHVAL300                       N3       700                 pctHval300
    HVAL500                          N4       300                 $500,000 to $999,999
    PCTHVAL500                       N3       200                 pctHval500
    HVALOVERMILLION                  N4       0                   $1,000,000 or more
    PCTHVALOVERMILLION               N3       0                   pctHvalOverMillion
    HVALOVER2MILLION                 N4       0                   $2,000,000 or more
    PCTHVALOVER2MILLION              N3       0                   pctHvalOver2Million
    MEDIANHVALUE                     N5       13370000            Median home value
    AVGHVALUE                        N5       15438500            Average home value
    HUSMORT                          N4       11900               Housing units with a mortgage
    PCTHUSMORT                       N3       5800                pctHUsMort
    HUSMORTOVER30PCT                 N4       5700                Owner costs 30% or more of HH income
    PCTHUSMORTOVER30PCT              N3       4800                pctHUsMortOver30Pct
    MEDIANOWNERCOSTSMORT             N4       92500               Median owner costs
    HUSNOMORT                        N4       8600                Housing units without a mortgage
    PCTHUSNOMORT                     N3       4200                pctHUsNoMort
    HUSNOMORTOVER30PCT               N4       900                 Nonmortgage owner costs 30% or more of HH income
    PCTHUSNOMORTOVER30PCT            N3       1100                pctHUsNoMortOver30Pct
    MEDIANOWNERCOSTSNOMORT           N4       40800               Median owner costs
    CASHRENTER                       N4       2700                Paying cash rent
    PCTCASHRENTER                    N3       6400                pctCashRenter
    NOCASHRENTER                     N4       1500                Paying no cash rent
    PCTNOCASHRENTER                  N3       3600                pctNoCashRenter
    MEDIANGROSSRENT                  N4       84500               Median rent
    AVGGROSSRENT                     N4       84400               Average gross rent
    CASHRENTEROVER30PCT              N4       2500                Gross rent 30% or more of HH income
    PCTCASHRENTEROVER30PCT           N3       6000                pctCashRenterOver30Pct
    CASHRENTEROVER750                N4       1800                Gross rent of $750 or more
    PCTCASHRENTEROVER750             N3       4300                pctCashRenterOver750
    PCTNOTINSUREDUNDER18             N3       600                 pctNotInsuredUnder18


