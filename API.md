<h1 align="center" style="margin: 30px 0 30px; font-weight: bold;">IMC软件二次开发文档</h1>
<h4 align="center">基于 Vue/Element UI 和 Spring Boot/Spring Cloud & Alibaba 前后端分离的分布式微服务架构</h4>
<p align="center">
    <a href="https://gitee.com/y_project/IMC-Cloud/stargazers"></a>
    <a href="https://gitee.com/y_project/IMC-Cloud"><img src="https://img.shields.io/badge/IMC-v3.5.0-brightgreen.svg"></a>
</p>

## 概述

在这份文档中，您将了解如何进行IMC软件的二次开发。IMC是一个软件继承平台，提供了多个引擎用于不同的功能扩展和自定义开发。本文档将重点介绍以下五个主要引擎：

1. **工作流引擎**
2. **分析引擎**
3. **报表引擎**
4. **QMS质量管理引擎**
5. 

## 工作流引擎

在这一部分，我们将介绍如何使用工作流引擎进行二次开发，并提供一些API调用示例和操作步骤。

### ServerAPI调用示例

```java
//api参数entity
WFHandleDTO handleDTO;
//调用ServerApi
R<Object> execute = Context.Instance.getDynamicApiEngine().execute(handleDTO);
```



在这里，我们将展示一些常用的工作流引擎API调用示例，包括：

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

### ProcessStep的调用

在这一部分，我们将指导您如何进行工作流引擎的ProcessStep二次开发操作，

可以通过`ICIMWorkflowStepDefinition`接口的以下属性，来运行自定义Handler

| 接口  | 属性  | 描述  |
| --- | --- | --- |
| ICIMWorkflowStepDefinition | CIMWorkStepDefinitionPostHandler | 后处理Hander |
| ICIMWorkflowStepDefinition | CIMWorkStepDefinitionPreHandler | 预处理      Handler |
| ICIMWorkflowStepDefinition | CIMWorkStepAutoCommitAfterHandler | 自动异步处理Handler |

自定义Handler示例：

```java
package com.imc.modules.workflow.procesStepHandler;

import com.imc.framework.collections.impl.ObjectCollection;
import com.imc.framework.handlers.wf.impl.WorkflowProcessStepHandlerBase;
import com.imc.modules.workflow.args.ProcessStepArgs;
import com.imc.modules.workflow.enums.WorkflowStepStatus;
import com.imc.schema.interfaces.ICIMWorkflowStep;
import com.imc.schema.interfaces.ICIMWorkflowStepObject;
import com.imc.schema.interfaces.IObject;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class Processhandler extends WorkflowProcessStepHandlerBase {
    @Override
    public String getHandlerId() {
        return "Processhandler";
    }
    @Override
    public boolean isSupported(String s){
        return s.equals(getHandlerId());
    }

    @Override
    public ObjectCollection execute(ObjectCollection objectCollection, IObject pobjStep, Object o) {
        try {
            ProcessStepArgs processStepArgs = (ProcessStepArgs)o;
            ICIMWorkflowStep lobjISPFWorkflowStep = pobjStep.toInterface(ICIMWorkflowStepObject.class).toInterface(ICIMWorkflowStep.class);
            Integer lintAcceptPercentage = lobjISPFWorkflowStep.getStepAcceptPercentage();
            if (lintAcceptPercentage == 0) {
                lintAcceptPercentage = 100;
            }
            double ldblAcceptPercentage = (double) lintAcceptPercentage / 100.0;
            List<ICIMWorkflowStep> lobjPreviousSteps = lobjISPFWorkflowStep.getPreviousDependentSuccessfulSteps();
            int lintApprovedSteps = 0;
            int lintTotalSteps = 0;
            if (lobjPreviousSteps!=null) {
                for (ICIMWorkflowStep stepObject : lobjPreviousSteps) {
                    ICIMWorkflowStepObject lobjICIMWorkflowStepObject = stepObject.toInterface(ICIMWorkflowStepObject.class);
                    if (lobjICIMWorkflowStepObject.getStepClaimedInd())//(lobjICIMWorkflowStepObject.getStepComments() == null )//|| lobjICIMWorkflowStepObject.getStepComments().IndexOf(SPFResources.get_GetDisplayAs(11001L)) < 0)
                    {
                        lintTotalSteps++;
                        if (lobjICIMWorkflowStepObject.getStepStatus().equals(WorkflowStepStatus.ELT_Pass_WorkflowStepStatus.name())) {
                            lintApprovedSteps++;
                        }
                    }
                }
            }
            double ldblActualPercent = (double) lintApprovedSteps / (double) lintTotalSteps;
            if (ldblActualPercent == 0.0) {
                processStepArgs.setRejectStep(true);
            } else if (ldblActualPercent < ldblAcceptPercentage) {
                processStepArgs.setRejectStep(true);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
        return null;
    }
}
```

## 分析引擎

这一部分将介绍如何使用分析引擎进行二次开发，并提供一些API调用示例和操作步骤。

### API调用示例

这里将展示一些常用的分析引擎API调用示例，包括：

- **创建数据分析任务**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |

  * 返回值：


- **获取分析结果**

  参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |



- **导出分析报告**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |
  
  * 返回值：


### 操作步骤

这一部分将指导您如何进行分析引擎的二次开发操作，包括：

- **安装分析引擎插件**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |
  
  * 返回值：



- **配置分析任务参数**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |
  
  * 返回值：



- **定制分析算法**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |
  
  * 返回值：


## 报表引擎

在这一部分，我们将介绍如何使用报表引擎进行二次开发，并提供一些API调用示例和操作步骤。

### API调用示例

这里将展示一些常用的报表引擎API调用示例，包括：

- **创建报表模板**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |
  
  * 返回值：



- **填充数据到报表**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |
  
  * 返回值：



- **导出报表**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |
  
  * 返回值：

### 操作步骤

这一部分将指导您如何进行报表引擎的二次开发操作，包括：

- **安装报表引擎插件**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |
  
  * 返回值：



- **设计报表模板**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |
  
  * 返回值：



- **生成报表数据源**

  * 参数：
  
  | 参数名称 | 参数类型 | 参数描述 |
  | -------- | -------- | -------- |
  |          |          |          |
  |          |          |          |
  
  * 返回值：


## QMS质量管理引擎

在这一部分，我们将介绍如何使用QMS质量管理引擎进行二次开发，并提供一些API调用示例和操作步骤。

### API调用示例

这里将展示一些常用的QMS质量管理引擎API调用示例，包括：

- 

### 操作步骤

这一部分将指导您如何进行QMS质量管理引擎的二次开发操作，包括：

- 

## 结论

本文档提供了IMC软件中工作流引擎、分析引擎、报表引擎和QMS质量管理引擎的二次开发示例和操作指南。通过按照文档中的步骤进行操作，您可以扩展和自定义IMC软件以满足特定的业务需求。

请注意，以上仅为一个大体框架示例，您可以根据具体的需求和文档内容进行调整和扩展。希望这可以帮助您开始编写IMC软件的Markdown文档！如有任何进一步的问题，请随时向我提问。
