# DotNetCore

---

* [DotNetCore](#dotnetcore)
  * [CMD](#cmd)
  * [EF Core](#ef-core)
    * [CMD](#cmd-1)
    * [Nuget](#nuget)
  * [Programe\.cs](#programecs)
  * [DI](#di)
  * [Startup\.Configure(静态文件、目录浏览)](#startupconfigure%E9%9D%99%E6%80%81%E6%96%87%E4%BB%B6%E7%9B%AE%E5%BD%95%E6%B5%8F%E8%A7%88)
  * [Middleware](#middleware)
  * [TagHelpers(标签助手)](#taghelpers%E6%A0%87%E7%AD%BE%E5%8A%A9%E6%89%8B)
    * [定义](#%E5%AE%9A%E4%B9%89)
    * [cshtml](#cshtml)
  * [ViewComponent(视图组件)](#viewcomponent%E8%A7%86%E5%9B%BE%E7%BB%84%E4%BB%B6)
    * [定义](#%E5%AE%9A%E4%B9%89-1)
    * [cshtml](#cshtml-1)

## CMD
* **`dotnet new mvc -n MvcApplication`** ：生成MvcApplication 模板项目
* **`dotnet restore`** ：生成依赖项
* **`dotnet build`** ：生成调试
* **`dotnet publish -c Release`** ：创建发布版本

## EF Core
### CMD
- **`dotnet ef migrations add InitNoteDB`** ：生成迁移文件
- **`dotnet ef database update`** ：生成数据库
### Nuget
- **`Add-Migration InitNoteDB`** ：生成迁移文件
- **`Update-Database`** ：生成数据库


###EF Core
- **`dotnet ef migrations add InitNoteDB`** ：生成迁移文件
- **`dotnet ef database update`** ：生成数据库


###EF Core Nuget
- **`Add-Migration InitNoteDB`** ：生成迁移文件
- **`Update-Database`** ：生成数据库

## Programe.cs
```csharp
public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
				.UseKestrel()
				.UseContentRoot(Directory.GetCurrentDirectory())
				.UseIISIntegration()
				.UseUrls("http://*:5000")
				.UseStartup<Startup>()
				.Build();
```

## DI
* **services.AddTransient** ： *（瞬时）每次申请对象时创建*
* **services.AddScoped** ： *（作用域）每个请求创建一次*
* **services.AddSingleton** ： *（单例 ）仅在第一次申请时创建*
###注入视图
```csharp
//注册服务
services.AddTransient<StatisticService>();
//声明
public class StatisticService 
{
	public int GetCount()
	{ return 10; }
}
//view页
//...
@inject StatisticService StatsService
//...
@StatsService.GetCount();
//...
```

## Startup.Configure(静态文件、目录浏览)
```csharp
//配置可访问静态文件
app.UseStaticFiles();
//配置静态文件的访问和目录浏览
app.UseFileServer(new FileServerOptions()
{
    FileProvider = new PhysicalFileProvider(Path.Combine(Directory.GetCurrentDirectory() + @"/MyStaticFile")),
    RequestPath = "/StaticFile",
    EnableDirectoryBrowsing = true
});
```

## Middleware
> **请求会经过多个中间件，每个中间件可以选择是否将请求交给下一个组件，中间件会按注册的顺序执行**
```csharp
public class RequestIPMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger _logger;    
    public RequestIPMiddleware(RequestDelegate requestDelegate, ILoggerFactory loggerFactory)
    {
        this._next = requestDelegate;
        this._logger = loggerFactory.CreateLogger<RequestIPMiddleware>();
    }    public async Task Invoke(HttpContext context)
    {
        _logger.LogInformation($"User IP :{context.Connection.RemoteIpAddress}");
        await _next.Invoke(context);
    }
}
public static class RequestIPExtensions
{
    public static IApplicationBuilder UseRequestIP(this IApplicationBuilder builder)
    {
        return builder.UseMiddleware<RequestIPMiddleware>();
    }
} 
public void Configure(IApplicationBuilder app, IHostingEnvironment env, IApplicationLifetime lifetime)
{
	//....
	loggerFactory.AddConsole(minLevel: LogLevel.Information);
	app.UseRequestIP();
	//....
}
```

## TagHelpers(标签助手)
### 定义
```csharp
public class EmailTagHelpers:TagHelper
{
	public string MailTo{get;set;}
	public override async void ProcessAsync(TagHelperContext context,TagHelperOutput output)
	{
		output.TagName = "a";
		var content = await output.GetChildContentAsync();
		var target = $"{content.GetContent()}@{MailTo}";
		output.Attrbutes.SetAttrbute("href",target);
		output.Content.SetContent(target);
	}
}
```
### cshtml
```csharp
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper *, WebApplication1 //导入自己的标签助手

<email mail-to="centa.com">Support</email>
=>
<a href="Support@centa.com">Support@centa.com</a>
```

## ViewComponent(视图组件)
> Views/Shared/Components/TopicRankLink/Default.cshtml
### 定义
```csharp
public class TopicRankLink:ViewComponent
{
	public IViewComponentResult Invoke()
	{
		return View(new List(){"top1","top2","top3"});
	}
}
```
### cshtml
```csharp
<div>
@await Component.InvokeAsync("TopicRankLink")
</div>
```