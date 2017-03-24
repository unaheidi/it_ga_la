认识 Sonar   
===============

#### 背景：

2015年和2016年做好了 gitlab 和 gerrit 的 HA，恰好公司各个业务部门有较强的意愿提高代码质量，因此，sonar服务被逐日重视。
我们认为：在gitlab上既可以查看代码又能查看sonar分析结果，这才是开发人员所期望的。
因此我先到github上查阅了有无现成的项目来对 gitlab和sonar进行整合的工具。令人高兴的是，果真有这样一个开源项目：
* sonar gitlab plugin https://github.com/gabrie-allaigre/sonar-gitlab-plugin。
本文把在搭建gitla和sonar的整合环境时，遇到的问题做个记录，一方面可以供今后翻阅，另一方通过这种形式可以更好地掌握sonar，从而更好地解决实际问题。


#### sonar点滴

##### 运行 mvn sonar:sonar（后面带其他参数）时，有时候能把报告发给sonar服务器，有时却不能。

原因：sonar.analysis.mode 参数在搞鬼。
*  当 sonar.analysis.mode=preview 时，不向 sonar 服务器传；
*  如果不指定该参数，默认  sonar.analysis.mode=public，则向 sonar 服务器传。

