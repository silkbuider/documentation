# SilkBuilder Service Structure

SilkBuilder can received a JSON Service Structure to run operations. This structure can have one or many operations. In the last case the operations are executed as a batch. The service structure is the following:

```json
{
  "authorizationList":"",
  "lang":"",
  "debugLevel":"",
  "rights":"",
  "parameterList" : [
    {"column":"", "value":"", "type":"", "secure":"" },
    {"column":"", "value":"", "type":"", "secure":"" }
  ],
  "operationList":[
    {
      "action":"",
      "operation":"",
      "columnList":[
         {"column":"", "value":"" },
         {"column":"", "value":"" }
       ]
     },
     {
       "action":"",
       "operation":"",
       "columnList":[
         {"column":"", "value":"" },
         {"column":"", "value":"" }
       ]
     }
   ]
}
```

