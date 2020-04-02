---
layout: post
title: JXLS+模版 导出excel
subtitle: 'java导出excel'
date: 2020-03-24
categories: test
tags: java 
---

在用java开发类似后台管理系统时，基本上都会涉及到excel导出数据的业务功能，大部分开发者都会选择poi或jxls，两者都有优点和缺点这里不做详细陈述。做按固定的模版进行导出时，这种情况用jxls做导出是最高效也是最方便的。  
jxls官网:（http://jxls.sourceforge.net/)  
下面是实际代码，只提供思路，一些相关配置不做说明，直接放关键代码：  
<br/>
**JAVA代码**
```java
// 表格使用的数据
List<Map<String, Object>> list = null;
//...这里获取数据给list，因人而异
Map<String, Object> map = new HashMap<String, Object>();
map.put("data",list);

// 获取模板文件
InputStream is = this.getClass().getClassLoader().getResourceAsStream("../template/register.xlsx");
XLSTransformer xlsTransformer = new XLSTransformer();

// 获取 Workbook ，传入 模板 和 数据  
Workbook workbook =  xlsTransformer.transformXLS(is,map);

// 写出文件
OutputStream os = null;
response.addHeader("Content-Disposition", "attachment;filename="+ URLEncoder.encode("外来人员健康登记表.xlsx", "UTF-8"));
os= new BufferedOutputStream(response.getOutputStream());
response.setContentType("application/vnd.ms-excel;charset=utf-8");
//输出
workbook.write(os);
is.close();
os.flush();
os.close();
```
<br/>
**excel模版设置**  
![excel01](https://tva1.sinaimg.cn/large/00831rSTly1gddynhj4dyj31nv0u0npe.jpg)
表头批注：`jx:area(lastCell="U6")`    U6是最末位单元格
  
数据批注：`jx:each(items="list" var="item" lastCell="U6")`
  
放数据格式：`${data.bodyTemperature}`  
<br/>END 以上

