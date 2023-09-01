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
>---
>
><span style='color:Blue'>[ 配置介绍`Nlog.config`]</span>
>
>```xml
><?xml version="1.0" encoding="utf-8" ?>
><!-- XSD manual extracted from package NLog.Schema: https://www.nuget.org/packages/NLog.Schema-->
><nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xsi:schemaLocation="NLog NLog.xsd"
>      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>      autoReload="true"
>      internalLogLevel="Info" >
>     <extensions>
>    
>     </extensions>
>     <!-- 日志目标输出 -->
>     <targets async="true">
>          <!-- write logs to file -->
>          <target xsi:type="File" name="logfile" fileName="${currentdir}\Log\${date:format=yyyy-MM-dd}.json">
>               <layout xsi:type="JsonLayout">
>                    <attribute name="Time" layout="${date:format=yyyy-MM-dd HH\:mm\:ss}" />
>                    <attribute name="Level" layout="${level}"/>
>                    <attribute name="Logger" layout="${logger}"/>
>                    <attribute name="Message" layout="${message}" />
>                    <attribute name="Exception" layout="${exception:format=tostring}" />
>                    
>               </layout>
>          </target>
>
>          <target xsi:type="ColoredConsole" name="Default_Console"
>                  layout="[${date:format=yyyy-MM-dd HH\:mm\:ss}] ${MicrosoftConsoleLayout}">
>               <!--<highlight-row condition="level == LogLevel.Info" foregroundColor="Green"/>
>               <highlight-row condition="level == LogLevel.Warn" foregroundColor="Yellow"/>
>               <highlight-row condition="level == LogLevel.Error" foregroundColor="Red"/>-->
>               <highlight-row condition="level == LogLevel.Error" backgroundColor="NoChange" foregroundColor="NoChange"/>
>               <highlight-row condition="level == LogLevel.Fatal" backgroundColor="NoChange" foregroundColor="NoChange"/>
>               <highlight-row condition="level == LogLevel.Warn" backgroundColor="NoChange" foregroundColor="NoChange"/>
>
>               <highlight-word text="info" condition="level == LogLevel.Info" backgroundColor="NoChange" foregroundColor="Green" ignoreCase="true" regex="info" wholeWords="true" compileRegex="true"/>
>               <highlight-word text="warn" condition="level == LogLevel.Warn" backgroundColor="NoChange" foregroundColor="Yellow" ignoreCase="true" regex="warn" wholeWords="true" compileRegex="true"/>
>               <highlight-word text="fail" condition="level == LogLevel.Error" backgroundColor="NoChange" foregroundColor="Red" ignoreCase="true" regex="fail" wholeWords="true" compileRegex="true"/>
>               <highlight-word text="crit" condition="level == LogLevel.Fatal" backgroundColor="NoChange" foregroundColor="DarkRed" ignoreCase="true" regex="crit" wholeWords="true" compileRegex="true"/>
>
>          </target>
>     </targets>
>
>     <!-- 日志路由规则 -->
>     <rules>
>          <logger name="*" minlevel="Trace" writeTo="Default_Console, logfile" />
>     </rules>
></nlog>
>
>
>```
>
>```csharp
>IServiceProvider provider = new ServiceCollection()
>                            .AddLogging(builder => {
>                                  builder.AddNLog("Nlog.config");
>                                 
>                             })
>                            .BuildServiceProvider();
>
>
>```
>
>
>
><br/>
