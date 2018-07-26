# thymeleaf模板引擎
更新时间：2018.07.10

目录
---

## 常用标签

* 修改标签属性 th:attr

    ```html
    <!- 设置Style样式 ->
    th:attr="style='background:url('+${l.qiniuIcon}+');background-size:cover;'"
    <!- 设置ID ->
    th:attr="id='id'+${l.id}"
    ```