### 问题：

​	VS生成项目时报错：bin/xxxxx/xxxx/xxx.exe不存在（例如“bin/Roslyn/Rosly/ncsc.exe不存在”）

### 解决方案：

​	右键项目→生成事件→生成前事件命令行，添加如下格式代码：

```shell
del /f /s /q $(TargetDir)\*.*
md $(TargetDir)\WebSite\
xcopy $(ProjectDir)WebSite\*.* $(TargetDir)WebSite\  /s /e /c /y /h /r
```

​	例如：

```shell
del /f /s /q $(TargetDir)\*.*
md $(TargetDir)\Roslyn\
xcopy E:\Repos\LogCenter\packages\Microsoft.CodeDom.Providers.DotNetCompilerPlatform.2.0.1\tools\Roslyn45\*.* $(TargetDir)Roslyn\  /s /e /c /y /h /r
```





