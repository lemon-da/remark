# InfluxDB 2.0使用

## InfluxDB下载

登陆InfluxDB官网（[Home | InfluxData](https://www.influxdata.com/)）

根据所需版本进行下载

### Windows版本

> 直接选择windows版本下载，下载之后可以直接运行influx.exe文件
> 
> (注：2.0版本下的influx配置文件路径为：C:\Users\用户\\.influxdbv2)

## InfluxDB 2.0 使用

### InfluxDB各个属性含义

> ##### organization：用户的工作空间，同一个组织下可以创建多个bucket
> 
> ##### bucket：相当于一个数据库（database）的概念，同时结合了保存期限
> 
> ##### measurement：类似于数据库内一张表的概念
> 
> ##### fields：包括field key和field value两个数据属性
> 
> ##### field key：属性名称
> 
> ##### field value：属性值
> 
> ##### tag：相当于索引的作用
> 
> ##### tag key：属性名称
> 
> ##### tag value：属性值

## InfluxDB 2.0 数据写入

### Line protocol

> ```js
> // Syntax
> <measurement>[,<tag_key>=<tag_value>[,<tag_key>=<tag_value>]] <field_key>=<field_value>[,<field_key>=<field_value>] [<timestamp>]
> 
> // Example
> myMeasurement,tag1=value1,tag2=value2 fieldKey="fieldValue" 1556813561098000000
> ```

## InfluxDB 2.0 数据读取

> ```js
> from(bucket: "") //筛选对应bucket
> |> range(start: 开始时间, stop: 结束时间) //筛选对应范围
> |> filter(fn: (r) => r._measurement == "") //筛选对应条件（measurement/tag）
> |> filter(fn: (r) => r[""]== "") //筛选条件另一种写法
> |> pivot(rowKey: ["_time"], columnKey: ["_field"], valueColumn: "_value") //设置显示数据
> ```

### 数据筛选内置函数

> ```js
> range() //筛选时间
> last() //获取最新一条数据
> count() //获取数量
> pivot() //设置显示数据
> ```
