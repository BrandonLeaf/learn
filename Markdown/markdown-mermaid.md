# Markdown-mermaid
更新时间: 2018.06.05
## 甘特图

```mermaid

gantt
dateFormat  YYYY-MM-DD
title 一键投保人物规划

section 单点登录
数据库设计 :active, p1, 2018-05-28, 4d
后端编码 :p2,after p1,5d
前端设计 :p3,after p2,1d
前端编码 :p4,after p3,5d
测试 :p5,after p4,5d

section 管理端
数据库设计 :active, p6,after p5, 4d
后端编码 :p2,after p1,5d
前端设计 :p3,after p2,1d
前端编码 :p4,after p3,5d
测试 :p5,after p4,5d

section 里程碑 0.3
功能测试            :       p6, after p3, 5d
上线               :       p7, after p6, 2d
交付               :       p8, afterp7, 2d

```