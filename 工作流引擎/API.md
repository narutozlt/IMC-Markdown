<h1 align="center" style="margin: 30px 0 30px; font-weight: bold;">工作流API手册</h1>
<h4 align="center">基于 Vue/Element UI 和 Spring Boot/Spring Cloud & Alibaba 前后端分离的分布式微服务架构</h4>
<p align="center">
    <a href="http://imc.smartsolutions.com.cn/login"></a>
    <a href="http://imc.smartsolutions.com.cn/login"><img src="https://img.shields.io/badge/IMC-v3.5.0-brightgreen.svg"></a>	
</p>


## ServerAPI调用示例

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
    `null`
