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



## 报告&报表引擎



## 基座引擎



## 缓存引擎



## 结论

本文档提供了IMC软件中工作流引擎、检索引擎和报告&报表引擎二次开发示例和操作指南。通过按照文档中的步骤进行操作，您可以扩展和自定义IMC软件以满足特定的业务需求。

请注意，以上仅为一个大体框架示例，您可以根据具体的需求和文档内容进行调整和扩展。希望这可以帮助您开始编写IMC软件的Markdown文档！如有任何进一步的问题，请随时向我提问。
