<h1 align="center" style="margin: 30px 0 30px; font-weight: bold;">IMC软件二次开发文档</h1>
<h4 align="center">基于 Vue/Element UI 和 Spring Boot/Spring Cloud & Alibaba 前后端分离的分布式微服务架构</h4>
<p align="center">
    <a href="http://imc.smartsolutions.com.cn/login"></a>
    <a href="http://imc.smartsolutions.com.cn/login"><img src="https://img.shields.io/badge/IMC-v3.5.0-brightgreen.svg"></a>	
</p>


[TOC]




## 概述

在这份文档中，您将了解如何进行IMC软件的二次开发。IMC是一个软件继承平台，提供了多个引擎用于不同的功能扩展和自定义开发。本文档将重点介绍以下六个主要引擎：

1. **权限引擎**
2. **工作流引擎**
3. **检索引擎**
4. **报告&报表引擎**
4. **基座引擎**
4. **缓存引擎**



## 权限引擎

<h4 align="center" >敬请期待</h4>



## 工作流引擎

### ServerAPI调用示例

```java
//api参数entity
WFHandleDTO handleDTO;
//调用ServerApi
R<Object> execute = Context.Instance.getDynamicApiEngine().execute(handleDTO);在这里，我们将展示一些常用的工作流引擎API调用示例，包括：
```

- **AttachWorkflow**

  用于附加item到工作流

  * API传参：

  | 参数名称       | 参数类型             | 参数描述   |
  | -------------- | -------------------- | ---------- |
  | contextObj     | ICIMWorkflowTemplate | 工作流模板 |
  | contextObjs(0) | ICIMWorkflowItem     | 工作流对象 |

  * 返回值：
    `null`

- **Approve**

  用于批准工作流

  * API传参：

  | 参数名称                | 参数类型               | 参数描述           |
  | ----------------------- | ---------------------- | ------------------ |
  | requestParam：handleMsg | String                 | 审批消息           |
  | requestParam：toNextMsg | String                 | 下一步消息         |
  | contextObj              | ICIMWorkflowStepObject | 工作流步骤实例对象 |

  * 返回值：
    `null`

- **Reject**

  用于拒绝工作流

  * API传参：

  | 参数名称                | 参数类型               | 参数描述           |
  | ----------------------- | ---------------------- | ------------------ |
  | requestParam：handleMsg | String                 | 审批消息           |
  | requestParam：toNextMsg | String                 | 下一步消息         |
  | contextObj              | ICIMWorkflowStepObject | 工作流步骤实例对象 |

  * 返回值：
    `null`

- **Claim**

  用于申领工作流

  * API传参：

  | 参数名称                | 参数类型               | 参数描述           |
  | ----------------------- | ---------------------- | ------------------ |
  | requestParam：handleMsg | String                 | 审批消息           |
  | contextObj              | ICIMWorkflowStepObject | 工作流步骤实例对象 |

  * 返回值：
    `null`

- **Assign**

  用于分配工作流负责人

  * API传参：

  | 参数名称       | 参数类型               | 参数描述           |
  | -------------- | ---------------------- | ------------------ |
  | contextObjs(0) | IObject                | 人员或角色         |
  | contextObj     | ICIMWorkflowStepObject | 工作流步骤实例对象 |

  * 返回值：
    `null`

- **Reassign**

  用于重新分配工作流负责人

  * API传参：

  | 参数名称       | 参数类型               | 参数描述           |
  | -------------- | ---------------------- | ------------------ |
  | contextObjs(0) | IObject                | 人员或角色         |
  | contextObj     | ICIMWorkflowStepObject | 工作流步骤实例对象 |

  * 返回值：
    `null



## 检索引擎

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

    

## 报告&报表引擎

### 常用API调用

在这里，我们将展示一些常用的报表引擎API调用示例，包括：

- **获取报表参数列表**

  获取指定报表生成时，需要输入的参数列表

  * API传参：

  | 参数名称 | 参数类型 | 参数描述   |
  | -------- | -------- | ---------- |
  | uid      | String   | 报表模板ID |

  * 返回值：

  ```json
  [
    {
      "name": "IMC内置数据源",
      "datasets": [
        {
          "name": "数据集A",
          "sql": "select * from cim_user where uid=:uid",
          "parameters": [
            {
              "id": "TMPJNHHGCD1123",
              "label": "UserId:",
              "name": "uid",
              "type": "String",
              "defaultValue": "USR.sauser"
            }
          ]
        }
      ],
      "type": "buildin"
    }
  ]
  ```

- **生成报表**

  生成指定格式报表文件下载

  * API传参：

  | 参数名称 | 参数类型               | 参数描述                   |
  | -------- | ---------------------- | -------------------------- |
  | uid      | String                 | 报表模版ID                 |
  | type     | String                 | 文档格式（pdf,word,excel） |
  | data     | List&lt;JSONObject&gt; | 参数列表                   |

  * 返回值：
    `文件流`



## 基座引擎

<h4 align="center" >敬请期待</h4>



## 缓存引擎

<h4 align="center" >敬请期待</h4>



## 结论

本文档提供了IMC软件中工作流引擎、检索引擎和报告&报表引擎二次开发示例和操作指南。通过按照文档中的步骤进行操作，您可以扩展和自定义IMC软件以满足特定的业务需求。

请注意，以上仅为一个大体框架示例，您可以根据具体的需求和文档内容进行调整和扩展。希望这可以帮助您开始编写IMC软件的Markdown文档！如有任何进一步的问题，请随时向我提问。
