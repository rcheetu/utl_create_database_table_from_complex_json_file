# utl_create_database_table_from_comples_json_file
Without hardcoding convert a complex json file to a database table

    ```   Create database table from comples json file                                                                                                                ```
    ```                                                                                                                                                               ```
    ```     Without hardcoding (Should work for many nested json files)                                                                                               ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```     INPUT                                                                                                                                                     ```
    ```     =====                                                                                                                                                     ```
    ```        {                                                                                                                                                      ```
    ```          "eventsLimited":false,                                                                                                                               ```
    ```          "events":[                                                                                                                                           ```
    ```            {                                                                                                                                                  ```
    ```              "_id":"1",                                                                                                                                       ```
    ```              "userId":"1",                                                                                                                                    ```
    ```              "timestamp":"2017-05-07T21:37:39.037Z",                                                                                                          ```
    ```              "detailedEvents":[                                                                                                                               ```
    ```                {                                                                                                                                              ```
    ```                  "eventType":"taskChanged",                                                                                                                   ```
    ```                  "taskId":"111",                                                                                                                              ```
    ```                  "changedProperties":[                                                                                                                        ```
    ```                    {                                                                                                                                          ```
    ```                      "property":"totalSecondsSpent",                                                                                                          ```
    ```                      "oldValue":0,                                                                                                                            ```
    ```                      "newValue":3600                                                                                                                          ```
    ```                    },                                                                                                                                         ```
    ```                    {                                                                                                                                          ```
    ```                      "property":"totalSecondsEstimate",                                                                                                       ```
    ```                      "oldValue":0,                                                                                                                            ```
    ```                      "newValue":144000                                                                                                                        ```
    ```                    }                                                                                                                                          ```
    ```                  ]                                                                                                                                            ```
    ```                }                                                                                                                                              ```
    ```              ]                                                                                                                                                ```
    ```            },                                                                                                                                                 ```
    ```          ...                                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```      PROCESS                                                                                                                                                  ```
    ```      =======                                                                                                                                                  ```
    ```        jsn <- as.data.frame(fromJSON(paste(readLines("d:/json/sample.json"), collapse="")));                                                                  ```
    ```        import r=jsn data=wrk.jsn;   ** jsn is SAS dataset                                                                                                     ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```         retain grp 0;                                                                                                                                         ```
    ```         set jsn(firstobs=2);                                                                                                                                  ```
    ```         v1=prxchange('s/\.\d//',1,v1);                                                                                                                        ```
    ```         if mod(_n_-1,11)=0 then grp=grp+1;                                                                                                                    ```
    ```         name=scan(v1,-1,'.');                                                                                                                                 ```
    ```         v1=compress(v1,'.');                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```         GRP         NAME          V2                                                                                                                          ```
    ```                                                                                                                                                               ```
    ```          1          _id           1                                                                                                                           ```
    ```          1          userId        1                                                                                                                           ```
    ```          1          timestamp     2017-05-07T21:37:39.037Z                                                                                                    ```
    ```          1          eventType     taskChanged                                                                                                                 ```
    ```          1          taskId        111                                                                                                                         ```
    ```          1          property      totalSecondsSpent                                                                                                           ```
    ```          1          oldValue      0                                                                                                                           ```
    ```          1          newValue      3600                                                                                                                        ```
    ```          1          property      totalSecondsEstimate                                                                                                        ```
    ```          1          oldValue      0                                                                                                                           ```
    ```          1          newValue      144000                                                                                                                      ```
    ```                                                                                                                                                               ```
    ```          2          _id           2                                                                                                                           ```
    ```          2          userId        1                                                                                                                           ```
    ```          2          timestamp     2017-05-07T22:31:30.037Z                                                                                                    ```
    ```          2          eventType     taskChanged                                                                                                                 ```
    ```          2          taskId        111                                                                                                                         ```
    ```          2          property      totalSecondsSpent                                                                                                           ```
    ```          2          oldValue      3600                                                                                                                        ```
    ```          2          newValue      5400                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```         * from here it is a couple of                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```      OUTPUT                                                                                                                                                   ```
    ```      ======                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```      WORK.WANT total obs=3                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```        USERID  _ID         PROPERTY           EVENTTYPE     TASKID           TIMESTAMP          NEWVALUE    OLDVALUE                                          ```
    ```                                                                                                                                                               ```
    ```          1440   1    totalSecondsEstimate    taskChanged     111      2017-05-07T21:37:39.037Z   144000       0                                               ```
    ```          1600   1    totalSecondsSpent       taskChanged     111      2017-05-07T21:37:39.037Z   3600         0                                               ```
    ```                                                                                                                                                               ```
    ```          1400   2    totalSecondsSpent       taskChanged     111      2017-05-07T22:31:30.037Z   5400         3600                                            ```
    ```                                                                                                                                                               ```
    ```  see                                                                                                                                                          ```
    ```  https://stackoverflow.com/questions/47076473/reading-json-file-without-the-sas-json-libname-engine                                                           ```
    ```                                                                                                                                                               ```
    ```  *                _              _       _                                                                                                                    ```
    ```   _ __ ___   __ _| | _____    __| | __ _| |_ __ _                                                                                                             ```
    ```  | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |                                                                                                            ```
    ```  | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |                                                                                                            ```
    ```  |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  data _null_;                                                                                                                                                 ```
    ```   file "d:/json/sample.json";                                                                                                                                 ```
    ```   input;                                                                                                                                                      ```
    ```   put _infile_;                                                                                                                                               ```
    ```  cards4;                                                                                                                                                      ```
    ```  {                                                                                                                                                            ```
    ```    "eventsLimited":false,                                                                                                                                     ```
    ```    "events":[                                                                                                                                                 ```
    ```      {                                                                                                                                                        ```
    ```        "_id":"1",                                                                                                                                             ```
    ```        "userId":"1",                                                                                                                                          ```
    ```        "timestamp":"2017-05-07T21:37:39.037Z",                                                                                                                ```
    ```        "detailedEvents":[                                                                                                                                     ```
    ```          {                                                                                                                                                    ```
    ```            "eventType":"taskChanged",                                                                                                                         ```
    ```            "taskId":"111",                                                                                                                                    ```
    ```            "changedProperties":[                                                                                                                              ```
    ```              {                                                                                                                                                ```
    ```                "property":"totalSecondsSpent",                                                                                                                ```
    ```                "oldValue":0,                                                                                                                                  ```
    ```                "newValue":3600                                                                                                                                ```
    ```              },                                                                                                                                               ```
    ```              {                                                                                                                                                ```
    ```                "property":"totalSecondsEstimate",                                                                                                             ```
    ```                "oldValue":0,                                                                                                                                  ```
    ```                "newValue":144000                                                                                                                              ```
    ```              }                                                                                                                                                ```
    ```            ]                                                                                                                                                  ```
    ```          }                                                                                                                                                    ```
    ```        ]                                                                                                                                                      ```
    ```      },                                                                                                                                                       ```
    ```      {                                                                                                                                                        ```
    ```        "_id":"2",                                                                                                                                             ```
    ```        "userId":"1",                                                                                                                                          ```
    ```        "timestamp":"2017-05-07T22:31:30.037Z",                                                                                                                ```
    ```        "detailedEvents":[                                                                                                                                     ```
    ```          {                                                                                                                                                    ```
    ```            "eventType":"taskChanged",                                                                                                                         ```
    ```            "taskId":"111",                                                                                                                                    ```
    ```            "changedProperties":[                                                                                                                              ```
    ```              {                                                                                                                                                ```
    ```                "property":"totalSecondsSpent",                                                                                                                ```
    ```                "oldValue":3600,                                                                                                                               ```
    ```                "newValue":5400                                                                                                                                ```
    ```              }                                                                                                                                                ```
    ```            ]                                                                                                                                                  ```
    ```          }                                                                                                                                                    ```
    ```        ]                                                                                                                                                      ```
    ```      }                                                                                                                                                        ```
    ```    ]                                                                                                                                                          ```
    ```  }                                                                                                                                                            ```
    ```  ;;;;                                                                                                                                                         ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  *          _       _   _                                                                                                                                     ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __                                                                                                                          ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                                                         ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                                                        ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  %utl_submit_wps64(resolve('                                                                                                                                  ```
    ```  options set=R_HOME "C:/Program Files/R/R-3.3.2";                                                                                                             ```
    ```  libname wrk "%sysfunc(pathname(work))";                                                                                                                      ```
    ```  proc r;                                                                                                                                                      ```
    ```  submit;                                                                                                                                                      ```
    ```  library("rjson");                                                                                                                                            ```
    ```  source("c:/Program Files/R/R-3.3.2/etc/Rprofile.site",echo=T);                                                                                               ```
    ```  jsn <- as.data.frame(fromJSON(paste(readLines("d:/json/sample.json"), collapse="")));                                                                        ```
    ```  jsnxpo<-cbind(names(jsn),t(jsn));                                                                                                                            ```
    ```  endsubmit;                                                                                                                                                   ```
    ```  import r=jsnxpo data=wrk.simpler;                                                                                                                            ```
    ```  run;quit;                                                                                                                                                    ```
    ```  '));                                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  WORK.SIMPLER total obs=20                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  Obs    V1                                                    V2                                                                                              ```
    ```                                                                                                                                                               ```
    ```    1    eventsLimited                                         FALSE                                                                                           ```
    ```    2    events._id                                            1                                                                                               ```
    ```    3    events.userId                                         1                                                                                               ```
    ```    4    events.timestamp                                      2017-05-07T21:37:39.037Z                                                                        ```
    ```    5    events.detailedEvents.eventType                       taskChanged                                                                                     ```
    ```    6    events.detailedEvents.taskId                          111                                                                                             ```
    ```    7    events.detailedEvents.changedProperties.property      totalSecondsSpent                                                                               ```
    ```    8    events.detailedEvents.changedProperties.oldValue      0                                                                                               ```
    ```    9    events.detailedEvents.changedProperties.newValue      3600                                                                                            ```
    ```   10    events.detailedEvents.changedProperties.property.1    totalSecondsEstimate                                                                            ```
    ```   11    events.detailedEvents.changedProperties.oldValue.1    0                                                                                               ```
    ```   12    events.detailedEvents.changedProperties.newValue.1    144000                                                                                          ```
    ```   13    events._id.1                                          2                                                                                               ```
    ```   14    events.userId.1                                       1                                                                                               ```
    ```   15    events.timestamp.1                                    2017-05-07T22:31:30.037Z                                                                        ```
    ```   16    events.detailedEvents.eventType.1                     taskChanged                                                                                     ```
    ```   17    events.detailedEvents.taskId.1                        111                                                                                             ```
    ```   18    events.detailedEvents.changedProperties.property.2    totalSecondsSpent                                                                               ```
    ```   19    events.detailedEvents.changedProperties.oldValue.2    3600                                                                                            ```
    ```   20    events.detailedEvents.changedProperties.newValue.2    5400                                                                                            ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  data addgrp;                                                                                                                                                 ```
    ```     retain grp 0;                                                                                                                                             ```
    ```     set simpler(firstobs=2);                                                                                                                                  ```
    ```     v1=prxchange('s/\.\d//',1,v1);                                                                                                                            ```
    ```     if mod(_n_-1,11)=0 then grp=grp+1;                                                                                                                        ```
    ```      name=scan(v1,-1,'.');                                                                                                                                    ```
    ```      v1=compress(v1,'.');                                                                                                                                     ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  proc sort data=addgrp  out=addgrpsrt;                                                                                                                        ```
    ```  by grp v1 v2;                                                                                                                                                ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  data addgrpfix(drop=v1);                                                                                                                                     ```
    ```    retain grp mnr 0;                                                                                                                                          ```
    ```    set addgrpsrt;                                                                                                                                             ```
    ```    by grp v1;                                                                                                                                                 ```
    ```    mnr=mnr+1;                                                                                                                                                 ```
    ```    output;                                                                                                                                                    ```
    ```    if last.v1 then mnr=0;                                                                                                                                     ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  proc sort data=addgrpfix  out=addgrpsrx;                                                                                                                     ```
    ```  by grp mnr;                                                                                                                                                  ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  /*                                                                                                                                                           ```
    ```  WORK.ADDGRPSRX total obs=19                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```  bs    GRP    MNR      NAME        V2                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```   1     1      1       _id         1                                                                                                                          ```
    ```   2     1      1       newValue    144000                                                                                                                     ```
    ```   3     1      1       oldValue    0                                                                                                                          ```
    ```   4     1      1       property    totalSecondsEstimate                                                                                                       ```
    ```   5     1      1       eventType   taskChanged                                                                                                                ```
    ```   6     1      1       taskId      111                                                                                                                        ```
    ```   7     1      1       timestamp   2017-05-07T21:37:39.037Z                                                                                                   ```
    ```   8     1      1       userId      1                                                                                                                          ```
    ```   9     1      2       newValue    3600                                                                                                                       ```
    ```  10     1      2       oldValue    0                                                                                                                          ```
    ```  11     1      2       property    totalSecondsSpent                                                                                                          ```
    ```  12     2      1       _id         2                                                                                                                          ```
    ```  13     2      1       newValue    5400                                                                                                                       ```
    ```  14     2      1       oldValue    3600                                                                                                                       ```
    ```  15     2      1       property    totalSecondsSpent                                                                                                          ```
    ```  16     2      1       eventType   taskChanged                                                                                                                ```
    ```  17     2      1       taskId      111                                                                                                                        ```
    ```  18     2      1       timestamp   2017-05-07T22:31:30.037Z                                                                                                   ```
    ```  19     2      1       userId      1                                                                                                                          ```
    ```  */                                                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  proc transpose data=addgrpsrx out=addgrpfxx;                                                                                                                 ```
    ```  by grp mnr;                                                                                                                                                  ```
    ```  var v2;                                                                                                                                                      ```
    ```  id name;                                                                                                                                                     ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  /*                                                                                                                                                           ```
    ```  _ID    NEWVALUE    OLDVALUE          PROPERTY           EVENTTYPE     TASKID           TIMESTAMP            USERID                                           ```
    ```                                                                                                                                                               ```
    ```   1      144000       0         totalSecondsEstimate    taskChanged     111      2017-05-07T21:37:39.037Z      1                                              ```
    ```          3600         0         totalSecondsSpent                                                                                                             ```
    ```   2      5400         3600      totalSecondsSpent       taskChanged     111      2017-05-07T22:31:30.037Z      1                                              ```
    ```  */                                                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  data                                                                                                                                                         ```
    ```  * all this to just retain character values;                                                                                                                  ```
    ```  data want;                                                                                                                                                   ```
    ```    if _n_=0 then do;                                                                                                                                          ```
    ```       %let rc=%sysfunc(dosubl('                                                                                                                               ```
    ```          data _null_;                                                                                                                                         ```
    ```            set addgrpfxx;                                                                                                                                     ```
    ```            array chrs _character_;                                                                                                                            ```
    ```            call symputx('dim',dim(chrs));                                                                                                                     ```
    ```          run;quit;                                                                                                                                            ```
    ```          %put &=dim;                                                                                                                                          ```
    ```       '));                                                                                                                                                    ```
    ```    end;                                                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```    set addgrpfxx;                                                                                                                                             ```
    ```    array chrs _character_;                                                                                                                                    ```
    ```    array savs[&dim] $32 _temporary_;                                                                                                                          ```
    ```    do i=1 to dim(chrs);                                                                                                                                       ```
    ```      if chrs[i] ne "" then savs[i]=chrs[i];                                                                                                                   ```
    ```    end;                                                                                                                                                       ```
    ```    do i=1 to dim(chrs);                                                                                                                                       ```
    ```      if chrs[i] eq "" then chrs[i]=savs[i];                                                                                                                   ```
    ```    end;                                                                                                                                                       ```
    ```    drop grp mnr _name_  i;                                                                                                                                    ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  * shouls optimize lengths;                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```  %utl_optlen(inp=want,out=want);                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```  /*                                                                                                                                                           ```
    ```   Variables in Creation Order                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```  #    Variable     Type    Len                                                                                                                                ```
    ```                                                                                                                                                               ```
    ```  1    _ID          Char      1                                                                                                                                ```
    ```  2    NEWVALUE     Char      6                                                                                                                                ```
    ```  3    OLDVALUE     Char      4                                                                                                                                ```
    ```  4    PROPERTY     Char     20                                                                                                                                ```
    ```  5    EVENTTYPE    Char     11                                                                                                                                ```
    ```  6    TASKID       Char      3                                                                                                                                ```
    ```  7    TIMESTAMP    Char     24                                                                                                                                ```
    ```  8    USERID       Char      1                                                                                                                                ```
    ```  */                                                                                                                                                           ```

