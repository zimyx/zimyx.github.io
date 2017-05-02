---
title:  "SQLite数据库"
---



[官方手册](https://sqlite.org/cli.html)

[中文整理](http://www.cnblogs.com/songsh96/archive/2011/10/14/2211079.html)

删除表   ```DROP TABLE xx_tmp;```

work库

建表               w*l*_data
```sql lite
CREATE TABLE w*l*_yxs(
yyyymm  number ,        -- 年月
mkt integer ,          -- 门店编号
mktname nvarchar(20),  -- 门店名称
xssr    number,        -- 销售收入
hsml    number,        -- 含税毛利
mll     number,        -- 毛利率
lks     number,        -- 来客数
kdj     number,         -- 客单价
 constraint pk_w*l*_data primary key (yyyymm,mkt)
);
```

转码  ```enca -L zh_CN -x UTF-8 /home/z/Desktop/201602.csv```

导入CVS
```sql lite
.separator ','
.import  201603.csv tmp_0?                   
```
--导入CVS数据
```sql lite
INSERT INTO w*l*_yxs
(yyyymm, mkt, mktname, xssr, hsml, mll, lks, kdj)
SELECT  201701 ,门店编码, 门店名称, "销售收入(单位：万元)", "含税毛利(单位：万元)", 含税毛利率, 来客数, "客单价(单位：元)"
FROM tmp_701  where  门店编码 >0 ;
```
合计
```sql lite
SELECT yyyymm,count(yyyymm), round(SUM(xssr),2),SUM(hsml), round( SUM(hsml)/SUM(xssr),4) ,SUM(lks),round(SUM(xssr*10000)/SUM(lks),2) FROM w*l*_yxs GROUP BY yyyymm ;
```
关店情况查询
```sql lite
select mkt,mktname  from  w*l*_yxs where yyyymm=201401  and mkt not in (select mkt from w*l*_yxs where yyyymm =201405 )
```
统计店数
```sql lite
select * from w*l*_yxs where yyyymm=201401
```
截取毛利率小数点2位
```sql lite
update w*l*_data set mll=round(mll,2) ;    
```

建表       月度人效KPI表
```sql lite
CREATE TABLE it_area_kpi (
	yyyymm INTEGER NOT NULL,        --年月
	area TEXT(10) NOT NULL, 	--区域
	employee INTEGER NOT NULL,      --实际人数
	yxs REAL NOT NULL,		--月度实绩
	yxs_rate REAL NOT NULL,		--产出系数	
	pos REAL NOT NULL,		--收银台
	pos_rate REAL NOT NULL,		--人均台数
	CONSTRAINT it_area_kpi_pk PRIMARY KEY (yyyymm,area)
);
```

导入CVS数据
```
INSERT INTO it_area_kpi
(yyyymm, area, employee, yxs, yxs_rate, pos, pos_rate)
SELECT yyyymm, 区域, 实际人数, "12月实绩", 产出系数, 收银台数量, 人均台数
FROM tmp   ;
```
合计
```sql lite
SELECT yyyymm,'小计',SUM(employee), SUM(yxs), ROUND(SUM(yxs)/SUM(employee),2), SUM(pos), ROUND(SUM(pos)/SUM(employee),2)
FROM it_area_kpi GROUP BY yyyymm ;
```

建表 it_org
```sql lite
CREATE TABLE it_org (
	ID INTEGER NOT NULL,    --组织编号  （1组23区域45区组67人）
	dept TEXT, 			--组别名称
	area TEXT,		        --区域名称		
	area_group TEXT,		--区组名称
	duty TEXT,				--岗位
	store_id INTEGER,		--店号		
	store TEXT,			--门店名称
	gh INTEGER,			--工号
	name TEXT,			--姓名
	tel NUMBER,			--电话
	idno TEXT,			--身份证 
	post_id TEXT,   		--职位层级
	post_rank REAL,       	--职位层级标准			
	CONSTRAINT it_org_PK PRIMARY KEY (ID)
)
```