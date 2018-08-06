# IIS

---

## 跨域(HTPP响应头)
* Access-Control-Allow-Headers：Content-Type
* Access-Control-Allow-Methods：GET,POST,PUT,DELETE,OPTIONS
* Access-Control-Allow-Origin：*
### Web.config
```xml
<system.webServer>
   　　<httpProtocol>
     　　　　<customHeaders>
       　　　　　　<add name="Access-Control-Allow-Methods" value="OPTIONS,POST,GET"/>
       　　　　　　<add name="Access-Control-Allow-Headers" value="x-requested-with,Content-Type"/>
       　　　　　　<add name="Access-Control-Allow-Origin" value="*" />
     　　　　</customHeaders>
   　　</httpProtocol>
 </system.webServer>
```

## MVC-403.14
### Web.config增加
```xml
<system.webServer>
	<modules runAllManagedModulesForAllRequests="true" />
    <validation validateIntegratedModeConfiguration="false" />  
</system.webServer>
```