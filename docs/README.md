<h1 align="center">NLog</h1>
<p align="center">
</p><br/>




>[!WARNING|style: flat|label: 简要说明 ]
>
>[<span style='color:#008B00'>[👓 官方文档 ]</span>](https://github.com/NLog/NLog/wiki ':target=_blank') [<span style='color:#008B00'>[👓 NuGet - NLog.Extensions.Logging ]</span>](https://github.com/NLog/NLog.Extensions.Logging ':target=_blank') 
>
>| 日志等级 | 事件描述                                                     |
>| -------- | ------------------------------------------------------------ |
>| `Trace`  | 辅助开发人员针对某个问题进行[ 代码跟踪调试 ] <span style='color:red'>[ 通常包含一些敏感信息 - 开发环境 ]</span> |
>| `Debug`  | [ 具有较短的失效性 ]记录一些辅助调试日志(`EG：某方法调用, 方法返回值`) |
>| `Info`   | [`具有较长的失效性`]向管理员传达非关键信息( 如仅供参考之类的注释 ) (`EG: 信息可以用来跟踪一个完整的处理流程`) |
>| `Warn`   | [ 应用出现不正常行为或非预期的结果 ] <span style='color:red'>[ 不对实际错误做出响应，警告应用程序未处于理想状态 → 进一步操作将导致关键性错误 ]</span> (`EG: 用户登陆未认证通过`) |
>| `Error`  | [ 应用因出现未被处理异常而终止，但整个应用不至于崩溃 - 非系统级别 ] (`EG: 当前操作代码异常`) |
>| `Fatal`  | [ 致命错误 ]可能会导致系统或应用程序崩溃(`需要引起足够重视的`) |
>
><br/>

<!-- tabs:start -->

#### **Nlog.config**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!-- XSD manual extracted from package NLog.Schema: https://www.nuget.org/packages/NLog.Schema-->
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    autoReload="true"             <!-- NLog 配置文件发生变化,是否自动加载 -->
    internalLogLevel="Info"       <!-- NLog 内部日志用于 [ 调试 NLog 本身的问题 ] -->
    internalLogFile="${basedir}/internal-nlog.txt"
    throwConfigExceptions="true"> <!-- NLog 配置文件出错时是否抛出异常 -->

    <!-- 配置：输出目标 -->
    <targets async="true">
       <target xsi:type="File" name="logfile" fileName="${currentdir}\Log\${date:format=yyyy-MM-dd}.json">
	        <layout xsi:type="JsonLayout">
			    <attribute name="Time" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}" />
			    <attribute name="Level" layout="${level:uppercase=true}"/>
			    <attribute name="Logger" layout="${logger}"/>
			    <attribute name="Message" layout="${message}" />
			    <attribute name="Exception" layout="${exception:format=tostring}" />

	       </layout>
      </target>

   </targets>

   <!-- 日志路由规则 -->
   <rules>
      <logger name="*" minlevel="Trace" writeTo="Default_Console" />
   </rules>
    
</nlog>



```

>```csharp
>using Microsoft.Extensions.DependencyInjection;
>using Microsoft.Extensions.Logging;
>using NLog.Extensions.Logging;
>
>IServiceProvider provider = new ServiceCollection()
>                           .AddLogging(builder => {
>                                builder.ClearProviders();
>                                builder.AddNLog("Nlog.config");
>
>                            })
>                           .BuildServiceProvider();
>
>ILogger<Program> logger = provider.GetRequiredService<ILogger<Program>>();
>logger.Log(LogLevel.Information,"{@Name} :Hello World", new { Name = "张三", Age = 20 });
>
>
>```
>
>
>
>



#### **Appsettings.json**

[<span style='color:#008B00'>[👓 配置说明 ]</span>](https://github.com/NLog/NLog.Extensions.Logging/wiki/NLog-configuration-with-appsettings.json ':target=_blank')

```json
{

	 "Logging": {
		  "LogLevel": {
			   "Default": "Debug"
		  },
		  "NLog": {
			   "IncludeScopes": true,
			   "RemoveLoggerFactoryFilter": true
		  }
	 },
	 "NLog": {
		  "autoReload": true,
		  "internalLogLevel": "Info",
		  "internalLogFile": "${basedir}/internal-nlog.txt",
		  "throwConfigExceptions": true,

		  "targets": {

			   "Default_Console": {
					"type": "Console",
					"layout": "[${date:format=yyyy-MM-dd HH\\:mm\\:ss}] ${MicrosoftConsoleLayout}"
			   }
		  },

		  "rules": [
			   {
					"logger": "*",
					"minLevel": "Trace",
					"writeTo": "Default_Console"
			   }
		  ]

	 }
	 
}


```

>```csharp
>using Microsoft.Extensions.Configuration;
>using Microsoft.Extensions.DependencyInjection;
>using Microsoft.Extensions.Logging;
>using NLog.Extensions.Logging;
>
>var config = new ConfigurationBuilder()
>                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
>                .Build();
>
>IServiceProvider provider = new ServiceCollection()
>                           .AddLogging(builder => {
>                                builder.ClearProviders();
>                                builder.SetMinimumLevel(LogLevel.Trace);
>                                //builder.AddNLog("Nlog.config");
>                                builder.AddNLog(config);
>                            })
>                           .BuildServiceProvider();
>
>ILogger<Program> logger = provider.GetRequiredService<ILogger<Program>>();
>using (logger.BeginScope("ScopeId:{ScopeId}", Guid.NewGuid()))
>{
>     logger.Log(LogLevel.Information, "{@Name} :Hello World", new { Name = "张三", Age = 20 });
>}
>
>
>```
>
>
>
>





<!-- tabs:end -->



