<h1 align="center">NLog</h1>
<p align="center">
</p><br/>




>[!WARNING|style: flat|label: 简要说明 ]
>
>[<span style='color:#008B00'>[👓 官方文档 ]</span>](https://github.com/NLog/NLog/wiki ':target=_blank') 
>
>[<span style='color:#008B00'>[👓 NuGet -  NLog.Extensions.Logging ]</span>](https://github.com/NLog/NLog.Extensions.Logging ':target=_blank')
>
>| <span style='color:Blue'>[ 日志等级 ]</span> | [ 事件描述 ]                                                 |
>| -------------------------------------------- | ------------------------------------------------------------ |
>| `Trace`                                      | 辅助开发人员针对某个问题进行 [ 代码跟踪调试 ] <span style='color:red'>[ 通常包含一些敏感信息 - 开发环境 ]</span> |
>| `Debug`                                      | [ 具有较短的失效性 ] 记录一些辅助调试日志(`EG：某方法调用, 方法返回值`) |
>| -                                            |                                                              |
>| `Info`                                       | [`具有较长的失效性`]向管理员传达非关键信息( 如仅供参考之类的注释 ) (`EG: 信息可以用来跟踪一个完整的处理流程`) |
>| `Warn`                                       | [ 应用出现不正常行为或非预期的结果 ] <span style='color:red'>[ 不对实际错误做出响应，警告应用程序未处于理想状态 → 进一步操作将导致关键性错误 ]</span> (`EG: 用户登陆未认证通过`) |
>| `Error`                                      | [ 应用因出现未被处理异常而终止，但整个应用不至于崩溃 - 非系统级别 ] (`EG: 当前操作代码异常`) |
>| `Fatal`                                      | [ 致命错误 ] 可能会导致系统或应用程序崩溃(`需要引起足够重视的`) |
>
><br/>

<!-- tabs:start -->

#### **[ Nlog.config ]**

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
    <targets async="true" />
    <!-- 日志路由规则 -->
    <rules />
    
</nlog>


```



#### **[ appsettings.json ]**

[<span style='color:#008B00'>[👓 官方说明 ]</span>](https://github.com/NLog/NLog.Extensions.Logging/wiki/NLog-configuration-with-appsettings.json ':target=_blank')

```json
{

     "NLog": {
          "autoReload": true,
          "internalLogLevel": "Info",
          "internalLogFile": "${basedir}/internal-nlog.txt",
          "throwConfigExceptions": true,

          "targets": { 
              "async": true,
                 .
                 .
                 .
              
           }, 
          "rules": [ ] 

     }
     
}


```



<!-- tabs:end -->



