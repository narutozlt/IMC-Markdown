<h1 align="center" style="margin: 30px 0 30px; font-weight: bold;">检索引擎API手册</h1>
<h4 align="center">基于 Vue/Element UI 和 Spring Boot/Spring Cloud & Alibaba 前后端分离的分布式微服务架构</h4>
<p align="center">
    <a href="http://imc.smartsolutions.com.cn/login"></a>
    <a href="http://imc.smartsolutions.com.cn/login"><img src="https://img.shields.io/badge/IMC-v3.5.0-brightgreen.svg"></a>	
</p>
[TOC]

### API调用示例

通用查询的接口直接和前端集成

- 1.根据 UID ClassDefUID ClassDef实现的对应接口class查询到相关数据： 

  ```java
  IObject objectByUidAndDefinitionUid = Context.Instance.getQueryHelper().getObjectByUidAndDefinitionUid("UID", "ClassDefUID", IObject.class);
  ```

- 2.根据 UID集合 ClassDefUID ClassDef实现的对应接口class批量查询到相关数据

  ``` java 
  List<IObject> objectsByUIDsAndClassDefinitionUID = Context.Instance.getQueryHelper().getObjectsByUIDsAndClassDefinitionUID(new ArrayList<>(), "ClassDefUID", IObject.class);
  ```

- 3.自定义条件查询

  ```java
  QueryRequest queryRequest = new QueryRequest();
  queryRequest.addClassDefForQuery("ClassDefUID");
  ```

  - 3.1 根据关联关系查询条件演示1,ClassDefUID为二端时,添加一端对象属性条件

    ```java
    queryRequest.addQueryCriteria(RelDirection.From2To1.getPrefix() + "RelDefUID", PropertyDefinitions.name1.toString(), SqlKeyword.EQ, "Not Empty Value");
    ```

  - 3.2 根据关联关系查询条件演示2,ClassDefUID为一端时,添加二端对象属性条件

    ```java
    queryRequest.addQueryCriteria(RelDirection.From1To2.getPrefix() + "RelDefUID", PropertyDefinitions.name2.toString(), SqlKeyword.EQ, "Not Empty Value");
    ```

  - 3.3 模糊查询条件演示

    ```java
    queryRequest.addQueryCriteria(null, PropertyDefinitions.name.toString(), SqlKeyword.LIKE, "*Not Empty Value*");
    ```

  - 3.4 IN/NOT IN 查询条件演示,可以使用String.join()方法使用Splitters.T_COMMA.getMsg()拼接集合

    ```java
    ArrayList<String> objects = new ArrayList<>();
            objects.add("a");
            objects.add("b");
            objects.add("c");
            objects.add("d");
            queryRequest.addQueryCriteria(null, PropertyDefinitions.name.toString(), SqlKeyword.IN, String.join(Splitters.T_COMMA.getMsg(), objects));
    ```

  - 3.5 IS NULL/IS NOT NULL 查询条件演示

    ```java
    List<IObject> iObjects = Context.Instance.getQueryHelper().query(queryRequest, IObject.class);
    ```

  - 3.6 自定义条件实际查询操作

    ```java
    List<IObject> iObjects = Context.Instance.getQueryHelper().query(queryRequest, IObject.class);
    ```
