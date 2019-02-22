# CSharpFrame

---

* [CSharpFrame](#csharpframe)
* [Log](#log)
  * [NLog](#nlog)
  * [Log4Net](#log4net)
    * [自定义日志输出](#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%97%A5%E5%BF%97%E8%BE%93%E5%87%BA)
    * [调试](#%E8%B0%83%E8%AF%95)
  * [Elmah(艾尔玛)](#elmah%E8%89%BE%E5%B0%94%E7%8E%9B)
    * [安全](#%E5%AE%89%E5%85%A8)
    * [输出到DB](#%E8%BE%93%E5%87%BA%E5%88%B0db)
    * [Emall\-&gt;elmah](#emall-elmah)
    * [不记录404](#%E4%B8%8D%E8%AE%B0%E5%BD%95404)
    * [Web\.config](#webconfig)
    * [<a href="file/elmah\.sql">SQL</a>](#sql)
  * [Serilog](#serilog)
* [ORM](#orm)
  * [IBatisNet](#ibatisnet)
    * [QueryHelper](#queryhelper)
* [DB](#db)
  * [MongoDB\-CSharp\-Driver](#mongodb-csharp-driver)
    * [Entity](#entity)
    * [使用](#%E4%BD%BF%E7%94%A8)
    * [Select](#select)
* [Policy](#policy)
  * [Polly(弹性和瞬时故障处理框架)](#polly%E5%BC%B9%E6%80%A7%E5%92%8C%E7%9E%AC%E6%97%B6%E6%95%85%E9%9A%9C%E5%A4%84%E7%90%86%E6%A1%86%E6%9E%B6)
    * [指定处理异常/故障的类型](#%E6%8C%87%E5%AE%9A%E5%A4%84%E7%90%86%E5%BC%82%E5%B8%B8%E6%95%85%E9%9A%9C%E7%9A%84%E7%B1%BB%E5%9E%8B)
    * [指定处理的异常返回值](#%E6%8C%87%E5%AE%9A%E5%A4%84%E7%90%86%E7%9A%84%E5%BC%82%E5%B8%B8%E8%BF%94%E5%9B%9E%E5%80%BC)
    * [Polly上下文](#polly%E4%B8%8A%E4%B8%8B%E6%96%87)
    * [Retry(重试)](#retry%E9%87%8D%E8%AF%95)
    * [Circuit\-breake(断路器)](#circuit-breake%E6%96%AD%E8%B7%AF%E5%99%A8)
    * [Fallback(回退)](#fallback%E5%9B%9E%E9%80%80)
    * [Timeout(超时)](#timeout%E8%B6%85%E6%97%B6)
    * [Bulkhead Isolation(隔板隔离)(限制最大并发请求数)](#bulkhead-isolation%E9%9A%94%E6%9D%BF%E9%9A%94%E7%A6%BB%E9%99%90%E5%88%B6%E6%9C%80%E5%A4%A7%E5%B9%B6%E5%8F%91%E8%AF%B7%E6%B1%82%E6%95%B0)
    * [Cache (缓存)](#cache-%E7%BC%93%E5%AD%98)
    * [PolicyWrap(策略包装)](#policywrap%E7%AD%96%E7%95%A5%E5%8C%85%E8%A3%85)
* [Web](#web)
  * [ServiceStack](#servicestack)
    * [WebConfig](#webconfig-1)
    * [AppHost\.cs](#apphostcs)
    * [Global\.asax](#globalasax)
  * [Swagger](#swagger)
    * [Starpup\.ConfigureServices()](#starpupconfigureservices)
    * [Starpup\.Configure()](#starpupconfigure)
    * [Properties &gt; launchSettings\.json](#properties--launchsettingsjson)
* [Windows](#windows)
  * [Topshelf](#topshelf)
    * [Program\.cs(示例)](#programcs%E7%A4%BA%E4%BE%8B)
* [性能测试](#%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95)
  * [BenchmarkDotNet](#benchmarkdotnet)
    * [修改\.csproj以测试对应版本](#%E4%BF%AE%E6%94%B9csproj%E4%BB%A5%E6%B5%8B%E8%AF%95%E5%AF%B9%E5%BA%94%E7%89%88%E6%9C%AC)

# Log
## NLog
> Install-Package Nlog
> 必须在主项目中安装
> 在根目录下添加：NLog.config
```xml
<?xml version="1.0"?>
<nlog autoReload="true" xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<!--
autoReload=true:在不重启的情况下修改配置文件有效
-->
<targets>
<target xsi:type="File" name="info" fileName="${basedir}/nlog/info_${date:format=yyyy-MM-dd}.log"
layout="${longdate} ${uppercase:${level}} ${message}" />
<target xsi:type="File" name="error" fileName="${basedir}/nlog/error_${date:format=yyyy-MM-dd}.log"
layout="${longdate} ${uppercase:${level}} ${message}" />
</targets>
<rules>
<logger name="info" level="Info" writeTo="info" />
<logger name="error" level="Error" writeTo="error" />
</rules>
</nlog>
```
```csharp
namespace xxx
{
    public static class NLogHelper
    {
        public static void Info(string message)
        {
            NLog.LogManager.GetLogger("info").Info(message);
        }
        public static void Error(string message)
        {
            NLog.LogManager.GetLogger("error").Error(message);
        }
    }
}
```
## Log4Net
> Install-Package Log4Net
> `可以在其他项目中安装`
> `在安装的项目的AssemblyInfo文件中添加：`
> **[assembly: log4net.Config.XmlConfigurator(ConfigFile = "log4Net.config", Watch = true)]**
> `在根目录下添加：log4Net.config`
```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler, log4net"/>
  </configSections>
  <log4net debug="true">
    <appender name="LogFileAppenderError" type="log4net.Appender.RollingFileAppender">
      <param name="File" value="log4net/error_" />
      <param name="AppendToFile" value="true" />
      <param name="MaxSizeRollBackups" value="100" />
      <param name="MaximumFileSize" value="5MB" />
      <param name="RollingStyle" value="Size" />
      <param name="StaticLogFileName" value="false" />
      <param name="DatePattern" value="yyyy-MM-dd&quot;.txt&quot;" />
      <param name="RollingStyle" value="Date" />
      <layout type="log4net.Layout.PatternLayout">
        <param name="ConversionPattern" value="%d [%t] %-5p %c [%x] - %m%n" />
      </layout>
    </appender>
    <appender name="LogFileAppenderInfo" type="log4net.Appender.RollingFileAppender">
      <param name="File" value="log4net/info_" />
      <param name="AppendToFile" value="true" />
      <param name="MaxSizeRollBackups" value="100" />
      <param name="MaximumFileSize" value="5MB" />
      <param name="RollingStyle" value="Size" />
      <param name="StaticLogFileName" value="false" />
      <param name="DatePattern" value="yyyy-MM-dd&quot;.txt&quot;" />
      <param name="RollingStyle" value="Date" />
      <layout type="log4net.Layout.PatternLayout">
        <param name="ConversionPattern" value="%d [%t] %-5p %c [%x] - %m%n" />
      </layout>
    </appender>

    <appender name="ADONetAppenderError" type="log4net.Appender.ADONetAppender">
      <bufferSize value="0" />
      <connectionType value="System.Data.SqlClient.SqlConnection" />
      <connectionString value="server=.;database=LogDB;uid=sa;pwd=S197346825x0."/>
      <commandText value="INSERT INTO dbo.LogError([LogDate],[LogThread],[LogLevel],[LogLogger],[Exception],[Message],[UserIp]) VALUES(@LogDate,@LogThread,@LogLevel,@LogLogger,@Exception,@Message,@UserIp)"/>
      <parameter>
        <parameterName value="@LogDate" />
        <dbType value="DateTime" />
        <layout type="log4net.Layout.RawTimeStampLayout" />
      </parameter>
      <!--线程号-->
      <parameter>
        <parameterName value="@LogThread" />
        <dbType value="String" />
        <size value="255" />
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%t" />
        </layout>
      </parameter>
      <!--日志类型LogLevel -->
      <parameter>
        <parameterName value="@LogLevel" />
        <dbType value="String" />
        <size value="255" />
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%p" />
        </layout>
      </parameter>
      <!--日志名称-->
      <parameter>
        <parameterName value="@LogLogger" />
        <dbType value="String" />
        <size value="255" />
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%LogLogger" />
        </layout>
      </parameter>
      <parameter>
        <parameterName value="@Message" />
        <dbType value="String" />
        <size value="4000" />
        <layout type="Shaoxing.Repository.Utility.Log4Net.CustomPatternLayout, Shaoxing.Repository.Utility">
          <conversionPattern value="%property{Message}" />
        </layout>
      </parameter>
      <parameter>
        <parameterName value="@UserIp" />
        <dbType value="String" />
        <size value="20" />
        <layout type="Shaoxing.Repository.Utility.Log4Net.CustomPatternLayout, Shaoxing.Repository.Utility" >
          <conversionPattern value = "%property{UserIp}"/>
        </layout>
      </parameter>
      自定义 字段 
      <parameter>
        <parameterName value="@Exception" />
        <dbType value="String" />
        <size value="2000" />
        <layout type="Shaoxing.Repository.Utility.Log4Net.CustomPatternLayout, Shaoxing.Repository.Utility" >
          <conversionPattern value = "%property{Exception}"/>
        </layout>
      </parameter>
    </appender>
    <appender name="ADONetAppenderInfo" type="log4net.Appender.ADONetAppender">
      <bufferSize value="0" />
      <connectionType value="System.Data.SqlClient.SqlConnection" />
      <connectionString value="server=.;database=LogDB;uid=sa;pwd=S197346825x0."/>
      <commandText value="INSERT INTO dbo.LogInfo([LogDate],[LogThread],[LogLevel],[LogLogger],[Exception],[Message],[UserIp]) VALUES(@LogDate,@LogThread,@LogLevel,@LogLogger,@Exception,@Message,@UserIp)"/>
      <parameter>
        <parameterName value="@LogDate" />
        <dbType value="DateTime" />
        <layout type="log4net.Layout.RawTimeStampLayout" />
      </parameter>
      <!--线程号-->
      <parameter>
        <parameterName value="@LogThread" />
        <dbType value="String" />
        <size value="255" />
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%t" />
        </layout>
      </parameter>
      <!--日志类型LogLevel -->
      <parameter>
        <parameterName value="@LogLevel" />
        <dbType value="String" />
        <size value="255" />
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%p" />
        </layout>
      </parameter>
      <!--日志名称-->
      <parameter>
        <parameterName value="@LogLogger" />
        <dbType value="String" />
        <size value="255" />
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%LogLogger" />
        </layout>
      </parameter>
      <parameter>
        <parameterName value="@Message" />
        <dbType value="String" />
        <size value="4000" />
        <layout type="Shaoxing.Repository.Utility.Log4Net.CustomPatternLayout, Shaoxing.Repository.Utility">
          <conversionPattern value="%property{Message}" />
        </layout>
      </parameter>
      <parameter>
        <parameterName value="@UserIp" />
        <dbType value="String" />
        <size value="20" />
        <layout type="Shaoxing.Repository.Utility.Log4Net.CustomPatternLayout, Shaoxing.Repository.Utility" >
          <conversionPattern value = "%property{UserIp}"/>
        </layout>
      </parameter>
      自定义 字段
      <parameter>
        <parameterName value="@Exception" />
        <dbType value="String" />
        <size value="2000" />
        <layout type="Shaoxing.Repository.Utility.Log4Net.CustomPatternLayout, Shaoxing.Repository.Utility" >
          <conversionPattern value = "%property{Exception}"/>
        </layout>
      </parameter>
    </appender>

    <logger name="error">
      <level value="ALL" />
      <appender-ref ref="ADONetAppenderError" />
    </logger>
    <logger name="info">
      <level value="ALL" />
      <appender-ref ref="ADONetAppenderInfo" />
    </logger>
  </log4net>
</configuration>
```
```csharp
namespace xxx
{
    public static class NLogHelper
    {
        public static void Info(string message)
        {
            NLog.LogManager.GetLogger("info").Info(message);
        }
        public static void Error(string message)
        {
            NLog.LogManager.GetLogger("error").Error(message);
        }
    }
}
```
### 自定义日志输出
```csharp
using System.IO;
using log4net.Core;
using log4net.Layout.Pattern;
using log4net.Layout;
namespace xxx.Log4Net
{
    public class CustomPatternLayout : PatternLayout
    {
        public CustomPatternLayout()
        {
            AddConverter("property", typeof(CustomPatternLayoutConverter));
        }
    }
}
public class CustomPatternLayoutConverter: PatternLayoutConverter
{
    protected override void Convert(TextWriter writer, LoggingEvent loggingEvent)
    {
        if (Option != null)
        {
            WriteObject(writer, loggingEvent.Repository, LookupProperty(Option, loggingEvent));
        }
        else
        {
            WriteDictionary(writer, loggingEvent.Repository, loggingEvent.GetProperties());
        }
    }
    /// <summary>
    /// 通过反射获取传入的日志对象的某个属性的值
    /// </summary>
    /// <param name="property"></param>
    /// <param name="loggingEvent"></param>
    /// <returns></returns>
    private object LookupProperty(string property, LoggingEvent loggingEvent)
    {
        object propertyValue = string.Empty;
        var propertyInfo = loggingEvent.MessageObject.GetType().GetProperty(property);
        if (propertyInfo != null)
        {
            propertyValue = propertyInfo.GetValue(loggingEvent.MessageObject, null);
        }
        return propertyValue;
    }
```
### 调试
```xml
<appSettings>
  <add key="log4net.Internal.Debug" value="true"/>
</appSettings> 
<system.diagnostics>
  <trace autoflush="true">
    <listeners>
      <add
        name="textWriterTraceListener"
        type="System.Diagnostics.TextWriterTraceListener"
        initializeData="log4net/Log4NetDebug1.txt" />
    </listeners>
  </trace>
</system.diagnostics>
```
## Elmah(艾尔玛)
> install-package elmah
> install-package elmah.mvc
> 这里如果程序报错，可在//domain/elmah.axd中查看信息
> Asp.Net Mvc中默认是不允许查看axd的，只不过在本地可以查看
### 安全
```xml
<!--是否自定义路由-->
<add key="elmah.mvc.IgnoreDefaultRoute" value="true" />
<!--自定义路由为:sxelmah-->
<add key="elmah.mvc.route" value="sxelmah" />
```
### 输出到DB
> install-package elmah.xml
> install-package elmah.sqlserver
> - 卸载elmah.xml
> - 执行App_Readme文件夹中的sql文件
> - 增加连接字符串elmah
```xml
<elmah>
	<errorLog type="Elmah.SqlErrorLog, Elmah" connectionStringName="elmah"/>
</elmah>
```
### Emall->elmah
```xml
<configuration>
	<elmah>
		<errorMail from="fromMail@..com" to="to@.com" subject="Elmah-Error" useSsl="false"/><!--需要ssl才将useSsl=true-->
	</elmah>  <system.net>
    <mailSettings>
      <smtp deliveryMethod="Network">
        <network host="smtp.gmail.com" port="587" userName="xxxx" password="pwd"/>
      </smtp>
    </mailSettings>
  </system.net>
</configuration>
```
### 不记录404
```xml
<elmah>
    <errorFilter>
      <test>
        <and>
          <equal binding="HttpStatusCode" value="404" type="Int32"/>
        </and>
      </test>
    </errorFilter>
</elmah>
```
### Web.config
```xml
<configuration>
  <configSections>
    <sectionGroup name="elmah">
      <section name="security" requirePermission="false" type="Elmah.SecuritySectionHandler, Elmah" />
      <section name="errorLog" requirePermission="false" type="Elmah.ErrorLogSectionHandler, Elmah" />
      <section name="errorMail" requirePermission="false" type="Elmah.ErrorMailSectionHandler, Elmah" />
      <section name="errorFilter" requirePermission="false" type="Elmah.ErrorFilterSectionHandler, Elmah" />
    </sectionGroup>
  </configSections>
  <system.web>
    <compilation debug="true" targetFramework="4.5.2" />
    <httpRuntime targetFramework="4.5.2" />
    <httpModules>
      <add name="ApplicationInsightsWebTracking" type="Microsoft.ApplicationInsights.Web.ApplicationInsightsHttpModule, Microsoft.AI.Web" />
      <add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" />
      <add name="ErrorMail" type="Elmah.ErrorMailModule, Elmah" />
      <add name="ErrorFilter" type="Elmah.ErrorFilterModule, Elmah" />
    </httpModules>
  </system.web>
  <system.webServer>
    <validation validateIntegratedModeConfiguration="false" />
    <modules>
      <remove name="ApplicationInsightsWebTracking" />
      <add name="ApplicationInsightsWebTracking" type="Microsoft.ApplicationInsights.Web.ApplicationInsightsHttpModule, Microsoft.AI.Web" preCondition="managedHandler" />
      <add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" preCondition="managedHandler" />
      <add name="ErrorMail" type="Elmah.ErrorMailModule, Elmah" preCondition="managedHandler" />
      <add name="ErrorFilter" type="Elmah.ErrorFilterModule, Elmah" preCondition="managedHandler" />
    </modules>
  </system.webServer>
  <connectionStrings>
    <add name="elmah" connectionString="server=.;database=ElmahLog;uid=sa;pwd=xxxx." providerName="System.Data.SqlServer"/>
  </connectionStrings>
  <appSettings>
	<!--其他配置-->
    <!--是否关闭Elmah.Mvc处理程序-->
    <add key="elmah.mvc.disableHandler" value="false" />
    <!--是否关闭全局过滤-->
    <add key="elmah.mvc.disableHandleErrorFilter" value="false" />
    <!--是否需要身份才能访问Elmah-->
    <add key="elmah.mvc.requiresAuthentication" value="false" />
    <!--是否自定义Elmah路由-->
    <add key="elmah.mvc.IgnoreDefaultRoute" value="true" />
    <!--任何角色可进入路由-->
    <add key="elmah.mvc.allowedRoles" value="*" />
    <!--任何用户可查询-->
    <add key="elmah.mvc.allowedUsers" value="*" />
    <!--Elmah访问路由-->
    <add key="elmah.mvc.route" value="sxelmah" />
    <add key="elmah.mvc.UserAuthCaseSensitive" value="true" />
  </appSettings>
  <elmah>
    <security allowRemoteAccess="false" />
    <errorLog type="Elmah.SqlErrorLog, Elmah" connectionStringName="elmah"/>
    <errorFilter>
      <test>
        <and>
          <equal binding="HttpStatusCode" value="404" type="Int32"/>
        </and>
      </test>
    </errorFilter>
  </elmah>
  <system.net>
    <mailSettings>
      <smtp deliveryMethod="Network">
        <network host="smtp.gmail.com" port="587" userName="xxxx" password="pwd"/>
      </smtp>
    </mailSettings>
  </system.net>
</configuration>
```
### [SQL](file/elmah.sql)
## Serilog
> install-package Serilog.Sinks.RollingFile
```csharp
Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Debug()
    .Enrich.FromLogContext()
    .WriteTo.RollingFile(System.IO.Path.Combine("logs", "app-{Date}.txt"))
    .CreateLogger();
try
{
    Log.Logger.Information("Starting web host");
    BuildWebHost(args).Run();
    return 0;
}
catch (Exception ex)
{
    Log.Logger.Fatal(ex, "Host terminated unexpectedly");
    return 1;
}
finally
{
    Log.CloseAndFlush();
}
```

# ORM
## IBatisNet
> Install-Package IBatisNet
* 在根目录增加[providers.config](file/providers.config)、[sqlmap.config](file/sqlmap.config)
* 在DAL层增加[data.xml](file/data.xml)，并将其设为嵌入的资源
### QueryHelper
```csharp
using IBatisNet.Common;
using IBatisNet.DataMapper;
using IBatisNet.DataMapper.Configuration;
namespace DAL
{
    public class QueryHelper
    {
        private static ISqlMapper _sqlMapper;
        private static readonly object _asyncObj = new object();

        public static ISqlMapper Instance()
        {
            if (null == _sqlMapper)
            {
                lock (_asyncObj)
                {
                    if (null == _sqlMapper)
                    {
                        var builder = new DomSqlMapBuilder();
                        var fileName = ConfigurationManager.AppSettings["SqlMapFileName"];//Config/sqlmap.config
                        _sqlMapper = builder.Configure(fileName);
                    }
                }
            }
            return _sqlMapper;
        }
    }
}
```

# DB
## MongoDB-CSharp-Driver
> Install Package MongoDB.Driver
> Install Package MongoDB.Bson
### Entity
```csharp
[BsonIgnoreExtraElements]//忽略对不上的属性
public class MsgEntity
{
	public ObjectId Id { get; set; }
	public string AppNo { get; set; }
	public string Event { get; set; }
	public MsgContent MsgContent { get; set; }
	public string Extra { get; set; }
	public string Identification { get; set; }
	//选择时区，否则时间会显示错误
	[BsonDateTimeOptions(Kind = DateTimeKind.Local)]
	public DateTime CreateTime { get; set; }
}
```
### 使用
```csharp
private static readonly MongoClient _client = new MongoClient(_connectionString);
private static readonly IMongoDatabase _database = _client.GetDatabase("MsgDB");
var collection = _database.GetCollection<MsgEntity>("CollectionName");
```
### Select
```csharp
var sort = Builders<MsgEntity>.Sort.Descending("CreateTime");
var filterBuilder = Builders<MsgEntity>.Filter;
var filter = filterBuilder.Empty;
if (null != dto)
{
    if (false == string.IsNullOrEmpty(dto.AppNo))
        filter &= filterBuilder.Eq(x => x.AppNo, dto.AppNo);
    if (false == string.IsNullOrEmpty(dto.Identification))
        filter &= filterBuilder.Eq(x => x.Identification, dto.Identification);
    if (dto.StartOn.HasValue)
        filter &= filterBuilder.Gte(x => x.CreateTime, dto.StartOn.Value);
    if (dto.EndOn.HasValue)
        filter &= filterBuilder.Lte(x => x.CreateTime, dto.EndOn.Value);
}
return collection
    .Find(filter)
    .Sort(sort)
    .Skip((dto.Index - 1) * dto.Length)
    .Limit(dto.Length)
    .ToListAsync().Result;
```

# Policy
## Polly(弹性和瞬时故障处理框架)
> Install-Package Polly
### 指定处理异常/故障的类型
```csharp
Policy.Handle<DivideByZeroException>();//单个异常
Policy.Handle<SqlException>(ex => ex.Number == 1205)//带条件的异常
Policy.Handle<DivideByZeroException>().Or<ArgumentException>()//多个异常
Policy//多个带条件的异常
   .Handle<SqlException>(ex =ex.Number == 1205)
   .Or<ArgumentException>(ex =ex.ParamName == "example")
Policy//内部异常
    .HandleInner<HttpResponseException>()
    .OrInner<OperationCanceledException>(ex => ex.CancellationToken == myToken)
```
### 指定处理的异常返回值
```csharp
Policy//指定触发异常/故障处理策略的返回值
.HandleResult<HttpResponseMessage>(r => r.StatusCode == HttpStatusCode.NotFound)
Policy//多个返回值
    .HandleResult<HttpResponseMessage>(r => r.StatusCode == HttpStatusCode.InternalServerError)
    .OrResult<HttpResponseMessage>(r => r.StatusCode == HttpStatusCode.BadGateway)
//同时指定异常和返回值
HttpStatusCode[] httpStatusCodesWorthRetrying = {
    HttpStatusCode.RequestTimeout, // 408
    HttpStatusCode.InternalServerError, // 500
    HttpStatusCode.BadGateway, // 502
    HttpStatusCode.ServiceUnavailable, // 503
    HttpStatusCode.GatewayTimeout // 504
}; 
HttpResponseMessage result = Policy
    .Handle<HttpResponseException>()
    .OrResult<HttpResponseMessage>(r => httpStatusCodesWorthRetrying.Contains(r.StatusCode))
```
### Polly上下文
```csharp
var policy = Policy
    .Handle<DivideByZeroException>()
    .Retry(3, (exception, retryCount, context) =>
    {
        var methodThatRaisedException = context["methodName"];
        Log(exception, methodThatRaisedException);
});
policy.Execute(
    () => DoSomething(),
    new Dictionary<string, object>() {{ "methodName", "some method" }}
);
```
### Retry(重试)
```csharp
Policy//重试多次
    .Handle<DivideByZeroException>()
    .Retry(3, (exception, retryCount) =>
    {
        // do something 
    });
Policy//永久重试
    .Handle<DivideByZeroException>()
    .RetryForever(exception =>
    {
            // do something       
    });
Policy//等待并重试
    .Handle<DivideByZeroException>()
    .WaitAndRetry(new[]
    {
        TimeSpan.FromSeconds(1),
        TimeSpan.FromSeconds(2),
        TimeSpan.FromSeconds(3)
    });
```
### Circuit-breake(断路器)
```csharp
Policy//出现两次错误，断开回路
    .Handle<DivideByZeroException>()
    .CircuitBreaker(2, TimeSpan.FromMinutes(1));
//发生2次异常之后，断开回路1分钟
Action<Exception, TimeSpan> onBreak = (exception, timespan) => { ... };
Action onReset = () => { ... };
CircuitBreakerPolicy breaker = Policy
    .Handle<DivideByZeroException>()
    .CircuitBreaker(2, TimeSpan.FromMinutes(1), onBreak, onReset);
```
### Fallback(回退)
```csharp
Policy//当程序触发异常/故障后，返回一个备用值
   .Handle<Whatever>()
   .Fallback<UserAvatar>(UserAvatar.Blank, onFallback: (exception, context) => 
    {
        // do something
    });
```
### Timeout(超时)
```csharp
var timeout = Policy.Timeout(1, TimeoutStrategy.Pessimistic, (context, timespan, task) =>
{
    Console.WriteLine("超时事件！");
});
try
{
    timeout.Execute(token =>
    {
        Thread.Sleep(TimeSpan.FromSeconds(5));
        Console.WriteLine("正常执行！");
    }, CancellationToken.None);
}
catch (TimeoutRejectedException e)
{
    Console.WriteLine(e.Message);
}
```
### Bulkhead Isolation(隔板隔离)(限制最大并发请求数)
```csharp
Policy//最大请求数：12，等待时会调用回调
  .Bulkhead(12, context =>
    {
        // do something
    });
//最大请求数：12，等待队列长度2
Policy
  .Bulkhead(12, 2)
```
### Cache (缓存)
```csharp
var memoryCacheProvider = new Polly.Caching.MemoryCache.MemoryCacheProvider(MemoryCache.Default);
var cachePolicy = Policy.Cache(memoryCacheProvider, TimeSpan.FromMinutes(5));
// 设置一个绝对的过期时间
var cachePolicy = Policy.Cache(memoryCacheProvider, new AbsoluteTtl(DateTimeOffset.Now.Date.AddDays(1));
// 设置一个滑动的过期时间，即每次使用缓存的时候，过期时间会更新
var cachePolicy = Policy.Cache(memoryCacheProvider, new SlidingTtl(TimeSpan.FromMinutes(5));
// 我们用Policy的缓存机制来实现从缓存中读取一个值，如果该值在缓存中不存在则从提供的函数中取出这个值放到缓存中。
// 借且于Polly Cache  这个操作只需要一行代码即可。
TResult result = cachePolicy.Execute(() => getFoo(), new Context("FooKey")); // "FooKey" is the cache key used in this execution.
// 定义缓存策略，并捕获用于日志记录的任何缓存提供程序错误。
var cachePolicy = Policy.Cache(myCacheProvider, TimeSpan.FromMinutes(5),
   (context, key, ex) => {
       logger.Error($"Cache provider, for key {key}, threw exception: {ex}."); // (for example)
   }
);
```
### PolicyWrap(策略包装)
```csharp
//定义多个，直接组合
var policyWrap = Policy
  .Wrap(fallback, cache, retry, breaker, timeout, bulkhead);
policyWrap.Execute(...)
//用链式语法组合
Avatar avatar = Policy
   .Handle<Whatever>()
   .Fallback<Avatar>(Avatar.Blank)
   .Wrap(commonResilience)
   .Execute(() => { /* get avatar */ });
```

# Web
## ServiceStack
> Install-Package ServceStack
### WebConfig
```xml
<system.web>
  <httpHandlers>
    <add path="*" type="ServiceStack.HttpHandlerFactory, ServiceStack" verb="*"/>
  </httpHandlers>
</system.web>
<system.webServer>
  <modules runAllManagedModulesForAllRequests="true" />
  <validation validateIntegratedModeConfiguration="false" />
  <urlCompression doStaticCompression="true" doDynamicCompression="false" />
  <handlers>
    <add path="*" name="ServiceStack.Factory" type="ServiceStack.HttpHandlerFactory, ServiceStack" verb="*" preCondition="integratedMode" resourceType="Unspecified" allowPathInfo="true" />
  </handlers>
</system.webServer>
```
### AppHost.cs
```csharp
public class AppHost : AppHostBase
{
    public AppHost() : base("xxx.Repository.Service", typeof(BaseService).Assembly)
    { }
    public override void Configure(Container container)
    {
        this.Plugins.Add(new CorsFeature("*", "GET, POST, PUT, DELETE, OPTIONS", "Content-Type,cityen"));
    } 
    public override void OnUncaughtException(IRequest httpReq, IResponse httpRes, string operationName, Exception ex)
    {
        httpRes.StatusCode = 200;
        httpRes.Write(JsonSerializer.SerializeToString(AopResult.Fail<object>(ex.Message)));
        NLog.LogManager.GetLogger("error").Error("{0}{1}{2}{3}{4}", httpReq.OperationName, Environment.NewLine, httpReq.Dto.ToJson(), Environment.NewLine, ex.InnerException + "\r\n" + ex.Message + "\r\n" + ex.StackTrace);
    }
    public override object OnPostExecuteServiceFilter(IService service, object response, IRequest httpReq, IResponse httpRes)
    {
    	//记录错误日志
        return base.OnPostExecuteServiceFilter(service, response, httpReq, httpRes);
    }
}
```
### Global.asax
```csharp
protected void Application_Start(object sender, EventArgs e)
{
    //初始化ServiceStack服务
    new AppHost().Init();
}
```
## Swagger
> Install-Package Swashbuckle.AspNetCore
> 可使用NSwagStudio生成Client代码
### Starpup.ConfigureServices()
```csharp
services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new Info() { Title = "Test API", Version = "v1" });
    c.IncludeXmlComments(Path.Combine(Env.WebRootPath, "App_Data", "MvcWebSite.xml"));//属性>生成>XML文档文件:App_Data/MvcWebSite.xml
});
``` 
### Starpup.Configure() 
```csharp
//...
app.UseSwagger();
app.UseSwaggerUI(c =>
{
    c.SwaggerEndpoint("/swagger/v1/swagger.json", "My Test API");
});
app.UseMvc(routes =>
{
    routes.MapRoute(
        name: "default",
        template: "{controller=Home}/{action=Index}/{id?}");
});
```
### Properties > launchSettings.json
```json
"profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "launchUrl": "swagger",//默认
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "MvcWebSite": {
      "commandName": "Project",
      "launchBrowser": true,
      "launchUrl": "swagger",//默认
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:58325/"
    }
  }
```

# Windows
## Topshelf
> Install-Package Topshelf
> Install-Package Topshelf.Log4Net
### Program.cs(示例)
```csharp
using log4net;
using log4net.Config;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

using System.Timers;

namespace Topshelf
{
    public class TownCrier
    {
        private Timer _timer = null;
        readonly ILog _log = LogManager.GetLogger("info");
        public TownCrier()
        {
            _timer = new Timer(1000) { AutoReset = true };
            _timer.Elapsed += (sender, eventArgs) => _log.Info(DateTime.Now);
        }
        public void Start()
        {
            _log.Info("Service is Started");
            _timer.Start();
        }
        public void Stop()
        {
            _log.Info("Service is Stop");
            _timer.Stop();
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            var logCfg = new FileInfo(AppDomain.CurrentDomain.BaseDirectory + "log4net.config");
            XmlConfigurator.ConfigureAndWatch(logCfg);

            HostFactory.Run(x =>
            {
                x.Service<TownCrier>(s =>
                {
                    s.ConstructUsing(name => new TownCrier());
                    s.WhenStarted(tc => tc.Start());
                    s.WhenStopped(tc => tc.Stop());
                });
                x.RunAsLocalSystem();

                x.SetDescription("Sample Topshelf Host服务的描述");
                x.SetDisplayName("Stuff显示名称");
                x.SetServiceName("Stuff服务名称");
                x.UseLog4Net();
            });
        }
    }
}
```

# 性能测试
## BenchmarkDotNet
> Install-Package BenchmarkDotNet
```csharp
using System;
using System.Security.Cryptography;
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

namespace MyBenchmarks
{
    [ClrJob(isBaseline: true), CoreJob, MonoJob, CoreRtJob]
    [RPlotExporter, RankColumn]
    public class Md5VsSha256
    {
        private SHA256 sha256 = SHA256.Create();
        private MD5 md5 = MD5.Create();
        private byte[] data;

        [Params(1000, 10000)]
        public int N;

        [GlobalSetup]
        public void Setup()
        {
            data = new byte[N];
            new Random(42).NextBytes(data);
        }

        [Benchmark]
        public byte[] Sha256() => sha256.ComputeHash(data);

        [Benchmark]
        public byte[] Md5() => md5.ComputeHash(data);
    }

    public class Program
    {
        public static void Main(string[] args)
        {
            var summary = BenchmarkRunner.Run();
        }
    }
}
```
### 修改.csproj以测试对应版本
> <TargetFrameworks>netcoreapp2.0;net461</TargetFrameworks>