# arp
arp 是一个可以实时抓取航空METAR和TAF报文的小型程序包，该程序包可以进行单个机场的报文查询，也可以进行实时的抓取和保存、归档。其数据源来自于美国航空天气中心[AWC](https://aviationweather.gov/)、[小飞象航空气象网](http://www.avt7.com/)、[中央气象台航空气象网](http://aviation.nmc.cn/)、[东北地区航空气象服务网](http://www.nemcaac.cn/dbinfo/app/common/index)。   
对于国内机场数据来说，小飞象航空气象网(avt7)、东北地区航空气象服务网(caac)的数据完整性要强于中央气象台航空气象网(nmc)，中央气象台航空气象网的完整性强于AWC(awc)，数据源需要在在调用程序的时候指定。

## 安装与配置
该程序包仅支持Python3.x版本，全部使用Python标准库，无需额外安装依赖库，可以直接下载或者使用`git`克隆代码库。文件中包含配置文件`config.json`, 该文件保存了实时抓取程序的配置, 包括保存路径、归档路径、日志路径以及所要抓取的机场列表信息。使用者可以对其内容进行自定义配置。其默认配置如下：
```json
{
  "METAR":{
    "log_path":"./data/metar/log/",
    "archive_path":"./data/metar/archive/",
    "buffer_path":"./data/metar/buffer/",
    "realtime_path":"./data/metar/realtime/"
  },
  "TAF":{
    "log_path":"./data/taf/log/",
    "archive_path":"./data/taf/archive/",
    "buffer_path":"./data/taf/buffer/",
    "realtime_path":"./data/taf/realtime/"
  },
  "ICAOS":[
    "ZBAA", "ZBAD", "ZBTJ", "ZBSJ", "ZBCD", "ZBHD", "ZBDH", "ZBSN",
    "ZBZJ", "ZBYN", "ZBCZ", "ZBDT", "ZBLF", "ZBLL", "ZBXZ", "ZBYC",
    "ZBHH", "ZBES", "ZBAL", "ZBAR", "ZBYZ", "ZBOW", "ZBCF", "ZBDS",
    "ZBEN", "ZBER", "ZBLA", "ZBHZ", "ZBMZ", "ZBTL", "ZBUH", "ZBUC",
    "ZBUL", "ZBXH", "ZBZL", "ZYTX", "ZYAS", "ZYCY", "ZYCH", "ZYTL",
    "ZYDD", "ZYJZ", "ZYYK", "ZYCC", "ZYBA", "ZYBS", "ZYSQ", "ZYTN",
    "ZYYJ", "ZYHB", "ZYDQ", "ZYFY", "ZYHE", "ZYJX", "ZYJD", "ZYJM",
    "ZYJS", "ZYMH", "ZYMD", "ZYQQ", "ZYDU", "ZYLD", "ZSSS", "ZSPD",
    "ZSNJ", "ZSCG", "ZSSH", "ZSLG", "ZSNT", "ZSWX", "ZSXZ", "ZSYN",
    "ZSYA", "ZSOF", "ZSAQ", "ZSJH", "ZSFY", "ZSTX", "ZSHC", "ZSNB",
    "ZSJU", "ZSLQ", "ZSWZ", "ZSYW", "ZSZS", "ZSFZ", "ZSLO", "ZSQZ",
    "ZSSM", "ZSWY", "ZSAM", "ZSCN", "ZSGZ", "ZSJD", "ZSGS", "ZSSR",
    "ZSYC", "ZSJN", "ZSDY", "ZSJG", "ZSLY", "ZSQD", "ZSRZ", "ZSWF",
    "ZSWH", "ZSYT", "ZHHH", "ZHES", "ZHSN", "ZHSY", "ZHXF", "ZHYC",
    "ZHCC", "ZHLY", "ZHNY", "ZGHA", "ZGCD", "ZGHY", "ZGCJ", "ZGSY",
    "ZGLG", "ZGDY", "ZGGG", "ZGFS", "ZGHZ", "ZGOW", "ZGMX", "ZGSZ",
    "ZGZJ", "ZGSD", "ZGNN", "ZGBS", "ZGBH", "ZGKL", "ZGHC", "ZGZH",
    "ZGWZ", "ZJHK", "ZJQH", "ZJSY", "ZJYX", "ZUCK", "ZUQJ", "ZUWX",
    "ZUUU", "ZUHY", "ZUDX", "ZUDC", "ZUKD", "ZUGU", "ZUJZ", "ZULZ",
    "ZUMY", "ZUNC", "ZUZH", "ZUXC", "ZUYB", "ZPPP", "ZPBS", "ZPCW",
    "ZPDL", "ZPMS", "ZPDQ", "ZPJM", "ZPLJ", "ZPLC", "ZPNL", "ZPSM",
    "ZUTC", "ZPWS", "ZPJH", "ZPZT", "ZULS", "ZUAL", "ZUBD", "ZUNZ",
    "ZURK", "ZLXY", "ZLHZ", "ZLYA", "ZLYL", "ZLLL", "ZLDH", "ZLXH",
    "ZLJQ", "ZLJC", "ZLLN", "ZLQY", "ZLTS", "ZLZY", "ZLIC", "ZLGY",
    "ZLZW", "ZWWW", "ZWAK", "ZWAT", "ZWBL", "ZWKN", "ZWFY", "ZWHM",
    "ZWTN", "ZWSH", "ZWKM", "ZWKC", "ZWKL", "ZWCM", "ZWSC", "ZWHZ",
    "ZWTC", "ZWTP", "ZWNL", "ZWYN", "VHHH", "VMMC", "ZUGY", "ZUAS",
    "ZUBJ", "ZUKJ", "ZULB", "ZUNP", "ZUPS", "ZUTR", "ZUYI", "ZUMT",
    "ZUZY", "ZLXN", "ZLDL", "ZLGM", "ZLGL", "ZLHX", "ZLYS", "ZUGH",
    "RCSS", "RCKH", "RCTP", "RCYU", "ZHXY", "ZUWS", "ZUBZ", "ZUGZ",
    "RCMQ", "ZGYY", "ZWRQ", "ZWTS", "RCNN", "RCKU", "RCFN", "RCQC",
    "RCBS"
  ]
}
```
其中log_path是日志保存路径；archive_path是归档路径；realtime_path是实时更新保存路径；buffer_path路径保存缓存文件用于对比更新；ICAOS是所要爬取的机场列表，机场名使用ICAO码。
四个路径可以随意配置，若路径不存在则程序会自动创建(有权限的情况下)。
机场列表默认为中国（包含港澳台）249个机场。

## 使用方法
### 单个机场查询
对于单个机场信息的查询，按以下示例查询   
`$ python collecter.py ZBAA METAR avt7`   
该命令是查询机场ZBAA（首都国际机场）的最新METAR报文，数据源选avt7   
同理，若要查询首都机场的最新TAF报文，可以执行   
`$ python collecter.py ZBAA TAF avt7`   
该命令是查询机场ZBAA（首都国际机场）的最新TAF报文，数据源选avt7   
几个数据源的代号分别为：avt7（小飞象）、caac（东北航空气象服务网）、nmc（中央气象台航空气象网）、awc（AWC）

### 自动化实时爬取
若想要自动化实时爬取并保存METAR报文数据，可以执行   
`$ python oparp.py METAR avt7`   
或者在服务器上后台运行   
`$ nohup python oparp.py METAR avt7 &`   
同理要爬取TAF报文只需将`METAR`换为`TAF`即可。   
执行该命令以后，程序将按照配置文件`config.json`中的设置，爬取机场列表里所有机场的METAR报文，并将结果以`.json`的格式保存在`realtime_path`、`buffer_path`和`archive_path`路径下。   

#### 实时文件与缓存文件
`realtime_path`路径中保存有一个文件：`updated_metars.json`，该文件保存的是最新一次查询相较于上一次查询所更新的机场的报文信息。
`buffer_path`路径中保存有一个文件：`all_metars.json`，该文件保存有所有机场的最新一次返回的报文。   
例如某个时次`all_metars.json`中保存有全部249个机场的报文信息(有些机场报文缺少则为空字符串)：   
```json
{
    "ZBAA": "METAR ZBAA 180200Z 32002MPS 240V020 CAVOK 05/M13 Q1022 NOSIG=",
    "ZBNY": "",
    "ZBTJ": "METAR ZBTJ 180200Z 35004MPS CAVOK 07/M11 Q1021 NOSIG=",
    ...
    "RCKH": "METAR RCKH 180200Z 20003KT 3500 HZ FEW014 23/16 Q1022 NOSIG RMK A3018=",
    "RCTP": "METAR RCTP 180200Z 09017KT CAVOK 20/09 Q1024 NOSIG RMK A3026=",
    "RCYU": "METAR RCYU 180200Z 07002KT 9999 FEW010 SCT060 20/10 Q1026 NOSIG RMK A3030="
}
```
`updated_metars.json`中保存的信息为   
```json
[
    {
        "NAME": "ZHES",
        "DATA": "SPECI ZHES 180220Z 00000MPS 0800 R01/0800V1300U FG FEW003 SCT010 OVC050 02/02 Q1027 BECMG FM0300 1500 BR="
    },
    {
        "NAME": "ZWHZ",
        "DATA": "SPECI ZWHZ 180227Z 00000MPS 1400 R27/1800N BR BKN030 M08/M10 Q1026 NOSIG="
    }
]
```
这也就是说在最新一次返回的数据中只有2个机场的报文更新了，`updated_metars.json`文件就只保存这2个更新的报文。而`all_metars.json`保存所有机场最新的报文，若最新一个时次某机场并未更新，则该机场在`all_metars.json`中会保存上一时次的报文。   
爬虫程序将每隔5分钟查询一次，若所有机场的报文都没有更新，则`all_metars.json`，`updated_metars.json`都不会被重写，否则两个文件都会被重写一遍。   
为了平衡数据库入库操作和爬虫时次对比的方便，`all_metars.json`和`updated_metars.json`数据采用不同的保存格式。   

#### 归档文件
最新报文除了会在`updated_metars.json`中不断重写，也会归档保存在`archive_path`目录中，该目录将以日期为文件夹来归档保存，每个文件为最新查询时间，例如`201812050135.json`

#### 日志文件
日志信息将保存在`log_path`中，每天自动保存为一个文件，当天实时更新的日志名为`TAF`或`METAR`，非当天的日志名为`METAR.20181203.log`形式。   
日志中   
`2018-12-05 02:11:43,612:INFO: sleeping`表示程序睡眠中   
`2018-12-05 02:15:03,676:INFO: start crawling`表示爬虫程序开始查询
开始查询以后，日志将记录每个机场的查询状态   
```
2018-12-05 02:15:04,595:INFO: (1/41) ZBAA finished
2018-12-05 02:15:07,564:INFO: (2/41) ZBTJ finished
2018-12-05 02:15:10,417:INFO: (3/41) ZBSJ finished
...
2018-12-05 02:16:52,113:INFO: (39/41) RCSS finished
2018-12-05 02:16:54,983:INFO: (40/41) RCKH finished
2018-12-05 02:16:57,848:INFO: (41/41) RCTP finished
```
若机场有报文更新，则日志中将提供记录   
```
2018-12-05 02:16:59,849:INFO: ZBAA is updated
2018-12-05 02:16:59,849:INFO: ZBTJ is updated
2018-12-05 02:16:59,849:INFO: ZBSJ is updated
...
2018-12-05 02:16:59,853:INFO: RCSS is updated
2018-12-05 02:16:59,853:INFO: RCKH is updated
2018-12-05 02:16:59,853:INFO: RCTP is updated
```
查询结束以后将记录保存和归档情况    
`2018-12-05 02:16:59,854:INFO: updated updated_metars.json`表示`updated_metars.json`文件已更新    
`2018-12-05 02:16:59,854:INFO: updated all_metars.json`表示`all_metars.json`文件已更新   
`2018-12-05 02:16:59,855:INFO: archived`表示新文件已归档     
若最新查询结果并无新的变化，则日志将提示   
`2018-12-05 02:22:06,138:INFO: last time is not updated`   
若发生错误信息，则日志会对错误信息做相应的记录

## 补充说明
由于SPECI报文是METAR报文的一种不定期的特殊天气报，它隶属于METAR。因此当用户爬取METAR报文的时候会不定期（如果有）获得SPECI报，且按METAR报文的路径保存和归档。
