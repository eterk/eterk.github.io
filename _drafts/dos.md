### 需要在后台执行的进程
```shell
@echo off 
if "%1" == "h" goto begin 
mshta vbscript:createobject("wscript.shell").run("%~nx0 h",0)(window.close)&&exit 
:begin 
::
{代码}
```

###  关闭指定进程

```shell
/f force 
taskkill /im target.exe /f
```



