---
title: "CompareData"
output: html_document
---
```{r}

library(data.table)
library(dplyr)
library(plyr)
library(reshape2)

data1<-read.csv("F:/Finance/CompareData/data4-renqian.csv")
data2<-read.csv("F:/Finance/CompareData/data4-renkun.csv")
data3<-read.csv("F:/Finance/CompareData/data4-server.csv")
names<-read.table("DESCRIPTION.txt",stringsAsFactors=F)
names<-names[,1]

names(data1)<-names
names(data2)<-names
names(data3)<-names

df1<-data1[,c(1,2,4,5)]
df2<-data2[,c(1,2,4,5)]
df3<-data3[,c(1,2,4,5)]
df1$Source<-"rq"
df2$Source<-"rk"
df3$Source<-"server"

my_func <- function(x) {
  paste0(x, collapse=",")
}

dt<-rbind(df1,df2,df3)%>%
  as.data.table()%>%
  melt(id=c(1,2,3,5))%>%
  dcast(TradingDay+UpdateTime+InstrumentID~Source+variable,fun.aggregate=my_func,value.var="value")
unconsistent<-dt[-with(dt,which(rk_LastPrice==rq_LastPrice&rk_LastPrice==server_LastPrice&rq_LastPrice==server_LastPrice)),]
head(unconsistent,10)
tail(unconsistent,10)
```


processing file: Preview-448445e6607.Rmd
  |......................                                           |  33%
  ordinary text without R code

  |...........................................                      |  67%
label: unnamed-chunk-1

Quitting from lines 6-40 (Preview-448445e6607.Rmd) 
Error in file(file, "rt") : cannot open the connection
Calls: <Anonymous> ... withCallingHandlers -> withVisible -> eval -> eval -> read.table -> file
Execution halted
