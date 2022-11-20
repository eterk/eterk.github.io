### 环境

`reference from ` <http://adv-r.had.co.nz/Environments.html>

```R
# 查看当前依赖的环境 
search()
# 创建新的环境
n=new.env()
# add variable to new env
n$a=3
#查看 a 在那个环境中
where("a")
get("geom_line")
#
as.enverionment("env_name")


# 指定定义域
f2 <- function() {
  x <- 10
  function() {
    def <- get("x", environment())
    cll <- get("x", parent.frame())
    list(defined = def, called = cll)
  }
}
g2 <- f2()
x <- 20
str(g2())


The globalenv()
#or global environment, is the interactive workspace. This is the environment in which you normally work. The parent of the global environment is the last package that you attached with library() or require().

The baseenv()
#or base environment, is the environment of the base package. Its parent is the empty environment.

emptyenv()
#or empty environment, is the ultimate ancestor of all environments, and the only environment without a parent.

environment() 
#is the current environment.
```

### Rstudio 快捷键

```shell
#打开终端
ALT+SHIFT+R
#插入注释
CTRL+ALT+SHIFT+R
# goto function/file
CTRL+.
#加入分割线
CTRL+SHIFT+R
```



##### 获取代码执行时间

```R
#获取代码执行时间
system.time(f<-fread("a,csv"))
#获取当前时间
Sys.time()
```

#### 获取时间序列

```R
#方案1  优秀
start_date<-as.Date(as.character(date_start_num),"%Y%m%d")
end_date<-as.Date(as.character(date_end_num),"%Y%m%d")
date_series<-as.list(start_date,end_date,by=1)
#方案2  优秀
date_series<-seq（as.Date("2014/02/03"),as.Date("2014/03/01"),"days")
#方案3  优秀
date_start=as.Date(start)
date_end<-as.Date(end)
dif=abs(start-end)
date_series<-seq(date_start,by="day",length.out=dif)
#方案4  垃圾
  date_sequence <- as.Date(as.character(c(date_range[1]:c(date_range[2]))),"%Y%m%d")%>%
    as.data.table()%>%
    filter(!is.na(.))%>%
    as.vector()
```

#### 计算两个时间差
```R
startdate <- as.Date("1989-01-01")
enddate <- as.Date("1989-01-02")
days <-enddate - startdate
```

### R包管理
```R
R包相关函数

sudo R CMD INSTALL XX.tar.gz 

# 查看包的安装目录
.libPaths()
 
# 查看已经安装的包及归属目录
library()
 
# 查看已安装包信息
installed.packages()
 
# 载入myPackage包
library(myPackage)
require(myPackage)

# 查看当前载入的包
search()
 
#查看启动R时自动载入的包。
getOption('defaultPackages')

#R包下载地址
https://cran.r-project.org/web/packages
 
```

R包离线安装
```R

# install packs on server
dirs <- "~/Dlm_wk/R_zip_pack/"
packs <- list.files(dirs, full.names =T) 
pack_nam <- list.files(dirs) %>% str_replace_all("_.*", "")
for (i in 1:length(packs)) {
  tryCatch({install.packages(packs[i],  repos = NULL, type = "source")
# 如果可以加载，就把安装包删除
    if (library(pack_nam[i],logical.return = T,character.only=TRUE)) file.remove(packs[i])
    detach(paste0("package:", pack_nam[i]), unload=TRUE)
    })
```

```R
#plotly 画图
plot_ly(x=xset, y=yset, z=zset)%>% add_surface()%>%
  layout(
    title = "Estimated Value function",
    scene = list(
      xaxis = list(title = "X"),
      yaxis = list(title = "Y"),
      zaxis = list(title = "Value")
    ))

```

ile.info（） 参数是表示文件名称的字符串向量，函数会给出每个文件的大小、创建时间、是否为目录等信息。

```
> file.info("z.txt")
      size isdir mode               mtime
z.txt   15 FALSE  666 2017-09-17 19:40:15
                    ctime               atime exe
z.txt 2017-09-16 21:19:58 2017-09-16 21:19:58  no
> class(file.info("z.txt"))
[1] "data.frame"
```

dir（）返回一个字符向量，列出在其第一个参数指定的目录中所有文件的名称。

```
> dir("code", recursive = T)
[1] "The art of R.R"
> dir("data")
[1] "qq"    "z"     "z.txt"
> dir("data", recursive = T)
[1] "qq/s.txt" "z"       
[3] "z.txt" 
```

file.exists( ) 返回一个布尔向量，表示作为第一个参数的字符串向量中给定的每个文件名是否存在。

```
> file.exists("z")
[1] TRUE
```

file.path（a, b）把a与b用“/”连接起来形成一个路径。

```R
> file.path("data", "qq/s.txt")
[1] "data/qq/s.txt"
```

需要理解两种时间格式有什么区别，熟悉相关时间格式函数

需要搞清楚$和df['name']的区别

cumsum()  累加求和	

```R
#使用group_by arrange 与mutate(cumsum())可以实现累加求和
data<-df%>%group_by(date)%>%arrange(hour)%>%mutate(value_cum=cumsum(value))
```

###箭头赋值符和等号赋值的区别
```R
#作用域不同


```

总结2种替换字符串的方法：

```javascript
group<-data.frame(group=c("12357e", "12575e", "197e18", "e18947"))
```

1)使用`gsub`

```javascript
group$group.no.e <- gsub("e", "", group$group)
```

2)使用`stringr`包装

```javascript
group$group.no.e <- str_replace_all(group$group, "e", "")
```

### PerformanceAnalytics

```R
 library(PerformanceAnalytics)
a<-skewness(x) #偏度  偏度为正，正偏态，为负偏态
df <- history_data%>%group_by(time_range,od)%>%
summarise(skewness(up_num,na.rm = TRUE),mean(up_num),kurtosis(up_num,na.rm=TRUE))
#峰度描述峰值部位的尖细
```

### 获取排列组合

```R
combination <- combn(unique(data$train_code),2)#获取两两组合
twain <- t(combination)
```

#### 获取两个集合的重叠率

```R
library(overlapping)
pair<-list(c1,c2)
rate<-overlap(pair,plot=TRUE)
```

### apply函数

```R
function_name<-function(datatable,parm_1,parm_2){
#do something
}

apply(datatablr,1,function_name,parm_1=parm_1,parm_2=parm_2)
```

#### R面向对象

```R
attr()  设置属性

```

### 生成重复值

```R
rep（x,n)
rep(1:7,each=1)
c(1:7)
```

### R 并行运算

```R
    library(foreach)
library(doParallel)
library(iterators)

    no_of_cores <- detectCores()-1
    
    cluster <- makeCluster(no_of_cores)
    
    registerDoParallel(cluster)
    
    system.time(result_obj_list1 <-foreach(i=1:nrow(predict_date_list),.combine ="rbind")%dopar%predict_df_execute_paraler(predict_date_list[i,],predict_method_name,repAbData,holiday_data))
    
    stopImplicitCluster()
```

### RC  OOP

```R
PredictObject <- setRefClass("PredictObject",
                           fields = list(date="POSIXct",
                                         holiday_flag="logical",
                                         holiday_name="character",
                                         furture_flag="logical",
                                         pre_day="numeric",
                                         real_value="numeric",
                                         predict_result_df="data.frame",
                                         predict_result_value="numeric",
                                         output="data.frame"
                                         ),
                           methods = list(
                             predict=function(){
                               print("开始预测")
                             },
                             set_output=function(){
                               export("PredictObject")
                             },
                             get_output=function(){
                               return(output)
                             }
                           ))

LoessPredict <- function(data,holiday_data,prePoints,date,holiday_flag,holiday_name,pre_day){
  if(holiday_flag){
    fx <- str_sub(data$peak_name,-2)==holiday_name
    data <- data%>%mutate(flow_num=ifelse(fx&holiday==1,number,flow_num))
    result <- train_OD_of_festival(date,data,holiday_data,pre_day,holiday_name)
  }
  else{
    result<- WR_withloess_local(data,date,pre_day)
  }
  if(is.null(result)|nrow(result)==0){
    prePoints <- ifelse(prePoints>pre_day,prePoints,NA)
    prePoints <- na.omit(prePoints)
    if (length(prePoints)<=0) {
      return(NULL)
    }else{
      if(holiday_flag){
        result <-train_OD_of_festival(date,data,holiday_data,prePoints,holiday_name)
      }else{
        result<- WR_withloess_local(data,date,pre_day)
      }
    }
  }
  return(result)
}
```

### R6 class

```R
library(R6)
library(dplyr)
PredictObjectR6 <- R6Class(
  "PredictObjectR6",
  public = list(
    date="POSIXct",
    holiday_flag="logical",
    holiday_name="character",
    furture_flag="logical",
    pre_day="numeric",
    real_value="numeric",
    predict_result_df="data.frame",
    predict_result_value="numeric",
    output="data.frame",
    predict=function(){
      print("开始预测")
    },
    set_output=function(){
      export("PredictObject")
    },
    get_output=function(){
      return(self$output)
    }
  )
)

LoessPredictObjectR6 <- R6Class("LoessPredictObjectR6",
                                  inherit = PredictObjectR6,
                                  public = list(
                                    predict=function(data,holiday_data,prePoints){
                                      data = data%>%select(everything(),train_date=date)
                                      holiday_data = holiday_data%>%select(everything(),train_date=date)
                                      self$predict_result_df=LoessPredict(data,holiday_data,prePoints,self$date,self$holiday_flag,self$holiday_name,self$pre_day)
                                      predict_value=self$predict_result_df$predictValue
                                      if(is.null(predict_value)){
                                        self$predict_result_value=-100
                                      }else if(predict_value<0){
                                        self$predict_result_value=-0
                                      }else{
                                        self$predict_result_value=predict_value
                                      }
                                      self$set_output()
                                      return(self)
                                    },
                                    set_output=function(){
                                      self$output=data.table(date=self$date,pre_day=self$predict_result_df$preday,
                                                          predict_value=self$predict_result_value,
                                                          real_value=self$real_value
                                      )
                                    }
                                  )
)

```

### 常用函数时间消耗比较	

```R
#seq  and sql_len
a <- func_time_spend_compare_one_time(function_list = list(seq="seq",seq_len="seq_len"),1000,1000)

```

### 时间比较

```R
#19951017 转为 1995-10-17
 start_date<- 19951017
 start_date_2 <- paste(substring(start_date,c(1,5,7),c(4,6,8)),collapse = "-")
```





````
paste(1,2,3,4)
paste(1,2,3,4)
# 大数值格式化
format(movie$BoxOffice, big.mark = ",", scientific = FALSE)

#DT::datatable(iris, options = list(pageLength = 25))
````

# 2020/02/15 学习笔记

1. stats::formula  什么用 没搞懂

2. sliderbarpanel

3. eventReactive 

4. htmlTemplate

5. fileInput

6. download

   ```R
    UI:downloadButton("downloadData", "Download")
   SERVER:  output$downloadData <- downloadHandler(
       filename = function() {
         paste(input$dataset, ".csv", sep = "")
       },
       content = function(file) {
         write.csv(datasetInput(), file, row.names = FALSE)
       }
     )
   ```

7. .延迟 invalidateLater(1000, session)

8. conditionalPanel

	```R
	 conditionalPanel(
	        'input.dataset === "iris"',
	        helpText("Display 5 records by default.")
	      )
	```

9. onFlushed 没太搞懂