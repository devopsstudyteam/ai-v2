# 集群
集群模块呈列了该监控应用中监控的所有集群信息，对于每个集群，从性能总览、拓扑、web事务、后台任务、数据库、远程服务、性能剖析、JVM（仅针对java应用）、错误信息、运行环境等方面进行了详细的分析。

##集群详情查看
###0. 集群列表
用户登录后，在集群列表可以查看所有集群相关信息，包括平均响应时间、每分钟调用次数、错误率等，对于集群名称可以进行重命名。点击集群名称或错误率、响应时间可以进入查看详情。
![](/images/tiers_10.png)

###1.总览
集群总览页面包括该集群内所有被调用组件/服务的响应时间叠加图、集群Apdex值、该集群内web事务的每分钟调用次数、慢web事务总览及该慢web事务下最大响应时间数据追踪、该集群内发生的事务错误率曲线图、服务器列表详情。  
![](/images/tier_01.png)  
点击某个响应时间较长的点，钻取该时间段内的trace列表，进行详情查看  
![](/images/tier_02.png)  
点击web事务中追溯的较长响应时间数据，可以查看该慢web事务的详情，包括trace和性能剖析  
![](/images/tier_03.png)    
当设置了基线后，还可以在响应时间图中查看基线数据，进行基线与平均响应时间的对比  
![](/images/tier_031.png)  

###2.拓扑
拓扑页面显示以该集群为起点所产生的调用拓扑，用户可点击右上角的图标，按拓扑图或列表进行显示。同时tier模块的拓扑图支持按域名、按网段来进行汇聚。其中对于域名的汇聚，如发生了jira.oneapm.me：8080的外部调用，系统会处理成*.oneapm.me；对于网段的汇聚，如发生了10.128.5.165::8080的外部调用，系统会处理成10.128.5.*。用户可以直接定位由网络或者某类外部服务而引起的应用问题。
同时在该拓扑中还增加了数据库的展开与合并，数据库默认展开，点击右上角的按钮可以进行合并，关系型数据库按Database类型合并，非关系型数据库按各自类型合并，nosql类型包括mongodb、redis、memcached等。
  
![](/images/tier_04.png)  

点击节点，可查看当前集群的总览（包括响应时间、apdex、吞吐量、每分钟错误次数）、节点、web事务入口、web事务。 
![](/images/tier_05.png)  


###3.web事务
web事务页面显示了该集群下的top5web事务响应时间曲线图，web事务列表，点击某条具体web事务，可进行详细的分析，具体可看[web事务](http://docs-ai.oneapm.com/book/Web_trans/web_index.html)  
![](/images/tier_06.png)  

###4.后台任务
后台任务按平均响应时间、性能指数、每分钟调用次数、响应时间占比四个维度进行数据分析。 
OneAPM AI按平均响应时间最长/apdex值/每分钟最大调用次数/响应时间占比从大到小进行排序，选择某条具体后台任务后，可以查看该任务的执行时间、吞吐量、breakdown table、慢后台任务追踪，对于慢后台任务，可以点击进行详情追踪。
  ![](/images/tier_07.png) 
  慢后台任务详情追踪
  ![](/images/tier_08.png) 

###5.数据库
数据库列表显示了该集群下所调用的数据库，包括关系型数据库和NoSQL，OneAPM AI从平均响应时间、总响应时间、每分钟调用次数三个维度进行划分，点击某个具体的语句，可以查看该语句的响应时间和吞吐量、调用事务时间占比、调用事务列表、慢sql追踪。 
![](/images/tier_09.png)
慢sql详情追踪：
![](/images/tier_10.png)  

###6.远程任务
远程服务包括应用内部服务和第三方服务两大模块。分别从服务平均响应时间、每分钟调用次数两方面进行性能分析。
  
*  远程服务列表  
   列表统计了类型、平均响应时间、每分钟调用次数、调用次数等数据，用户可以进行概览。
![](/images/tier_11.png) 

*  具体远程服务详情查看
点击相应的远程服务，可以查看该服务的响应时间和吞吐量、调用事务时间占比、调用事务列表，点击事务列表的某条事务，可以进行详情分析。  
![](/images/tier_12.png)

###7.性能剖析
在性能分析板块，用户可以自定义持续时间、采样周期、是否仅包含http请求、服务器名称等参数，进行性能剖析，剖析产生结果后，点击查看即可。
![](/images/tier_13.png)


###8.主机
总览页面右上角默认为全部实例，显示内容包括 cpu使用率最高的top5主机、内存使用率的top5主机、磁盘空间使用率最高的top5主机分区、磁盘IO使用率最高的top5主机分区。Cpu使用率、内存使用率。用户点击右上角的全部实例切换成单个实例，在页面中显示该探针实例对应的主机相关信息。
磁盘空间使用率、磁盘IO使用率选定的五个主机指标为选定时间窗口内平均值最高的top5。  
![](/images/tier_host1.png)  
在筛选页面，用户可以选择主机相关性能指标、其它指标进行筛选过滤    
![](/images/tier_host2.png)  
点击主机名称可以进行具体主机的详情查看，包括总览、磁盘、网络三部分。在一个主机上，用户可能部署一个集群的多个探针，也可能只部署一个探针，应用可能部署在主机上，也部署在容器上，但在主机模块均进行了处理。
主机总览页面为七图联动：  

1）web事务图，web事务图以折线图加柱状图的方式进行呈现，左侧会显示用户选定时间窗口内的最后一个时间颗粒度的平均响应时间（即当前平均响应时间）和每分钟调用次数（当前平均调用次数）。web事务根据右上角的探针实例情况、应用部署情况而定。当右上角为全部实例时，web事务进行主机维度的聚合，比如主机或该主机的容器上部署了A探针和B探针，调用次数为A探针的调用次数+B探针的调用次数，平均响应时间为A探针的响应时间+B探针的响应时间/A探针调用次数+B探针调用次数；当右上角为单个实例时，web事务图显示该探针实例对应的web事务平均响应时间和调用次数。  

2）CPU图，cpu图以面积图方式呈现，由cpu使用率、system、user三部分构成，cpu使用率为system和user的求和，左侧显示主机当前的cpu使用率、当前system使用率、当前user使用率。  

3）Load Average图，load average有load average1/5/15min，由曲线图构成，Windows系统则不显示load average相关数据。  

4）内存，内存图由内存使用率、swap使用率构成，以面积图形式表现，不同颜色表示内存、swap使用率。同时左侧显示当前内存使用率、当前swap使用率、当前内存已使用量、当前swap已使用量，内存/swap使用量默认单位为B，当内存使用量或swap使用量过大时，按1024B=1KB，1024KB=1MB进行换算。  

5）磁盘，磁盘由磁盘空间使用率、磁盘IO使用率组成，以面积图形式表现，不同颜色表示磁盘空间、磁盘IO使用率，默认显示该主机磁盘空间使用率最高的磁盘信息，点击可以进行磁盘的切换。同时左侧显示当前磁盘空间/IO使用率、磁盘当前占用/空闲大小。  

6）文件，文件由曲线图表示。同时左侧显示当前文件打开数、平均文件打开数、最大文件打开数。  

7）TCP连接，TCP连接数由曲线图表示，tcp连接数为所有状态（listen、syn-sent、syn-received、establish、fin-wait-1、fin-wait-2、close-wait、closing、last-ack、time-wait）值的求和。同时左侧显示当前/平均/最大tcp连接数。  
![](/images/tier_host3.png)   

**具体主机-磁盘**  
磁盘页面显示该主机上不同磁盘分区的相关数据，默认显示平均磁盘占用率最高的分区，显示内容包括磁盘空间使用率、磁盘IO使用率、磁盘IO读写次数、磁盘IO吞吐量。  
1）磁盘空间使用率，以面积图形式展现，同时显示当前磁盘空间使用率、当前磁盘分区占用大小、该磁盘分区的总容量数据；  

2）磁盘IO使用率，面积图形式展现。其中磁盘IO平均使用率为选定时间窗口内的所有磁盘IO使用率求和/总个数，同时图形上侧显示当前磁盘io使用率、磁盘IO平均使用率。 
 
3）磁盘IO读写次数，以曲线图表示，不同颜色代表读与写，鼠标悬停某个点时，同时显示当前读与写的次数，并且读/写次数一行加深（悬停读/写线时）。同时图形上侧显示当前磁盘IO读写次数。  

4）磁盘IO吞吐量，以曲线图表示，不同颜色代表读与写，鼠标悬停某个点时，同时显示当前读与写的速度，并且读/写次数一行变亮（悬停读/写线时）。同时图形上侧显示当前磁盘IO读写速度。
 
![](/images/tier_host4.png)

**具体主机-网络**  
网络页面显示该主机不同网卡上的相关数据，默认显示当前发送包数较大的网卡，显示内容包括网络发送接收包数、网络吞吐量。  
1）网络发送接收包数，以曲线图表示，不同曲线分别代表发送与接收。同时在上方显示当前发送/接收包数、平均发送/接收包数。鼠标悬停时，同时显示发送与接收包个数，悬停线代表的指标（如网络发送曲线）加深。  
2）网络发送接收速度，以曲线图表示，不同曲线分别代表发送与接收。同时在上方显示当前发送/接收速度、平均发送/接收速度。鼠标悬停时，同时显示发送与接收包速度，悬停线代表的指标（如网络发送速度）加深。

![](/images/tier_host5.png)

###9.容器
在容器页面包括按cpu使用率、内存使用率、磁盘读（写）速度、网络发送（接收）速度的top5容器，该tier下的所有容器列表。容器默认展示信息为容器名称、宿主机名称、探针实例、容器ID、镜像名称、镜像ID、容器创建时间、镜像创建时间、cpu使用率、内存使用率。
点击容器名称弹框显示容器相关详情，点击宿主机名称跳转该探针实例对应的主机总览页面，点击探针实例名称跳转该探针实例对应的总览页面。
当应用没有部署在容器时，页面显示暂无数据。  
![](/images/tier_container.png)

容器总览页面七图联动，鼠标悬停某时间点时便可知道在该时间点时对应的容器相关数据。
1）web事务图，web事务显示该容器上探针实例对应的web事务的平均响应时间和调用次数
2）cpu图，面积图形式展现，显示该容器耗费的cpu率图及当前cpu使用率。
3）内存，面积图形式展现，显示该容器的内存使用率图及当前内存使用率、当前内存使用量/限制量。
4）磁盘读写速度和磁盘读写次数。以曲线图和柱状图展现磁盘读写速度与读写次数，同时显示当前的磁盘读写次数与读写速度。
5）网络发送接收速度与发送接收量。以曲线图和柱状图展现网络发送速度与发送接收量，同时显示选定时间窗口内当前的发送与接收速度、发送与接收量。  
![](/images/tier_container2.png)  
![](/images/tier_container3.png)


###10. JVM
对于java语言的应用，OneAPM AI提供了JVM功能，可供用户了解JVM的内存、线程、会话的相关情况。
![](/images/tier_14.png)

###11.错误信息
OneAPM AI将错误信息分为两大类，web事务和后台任务错误信息。点击“错误信息”，可以看到每个分类下的相应错误，包括错误出现的第一次时间、最后一次时间、请求地址、错误消息、出现次数。
![](/images/tier_15.png)  

点击相应的请求地址，可以查看错误详情  
![](/images/tier_16.png)  

###12.运行环境
在运行环境模块，AI列出了整个集群的运行环境，包括每个参数的值、包文件的版本号等，用户可详细了解自己应用集群的相关情况。
![](/images/tier_17.png)  

