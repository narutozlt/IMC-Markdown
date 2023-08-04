<h1 align="center" style="margin: 30px 0 30px; font-weight: bold;">报告&报表API手册</h1>
<h4 align="center">基于 Vue/Element UI 和 Spring Boot/Spring Cloud & Alibaba 前后端分离的分布式微服务架构</h4>
<p align="center">
    <a href="http://imc.smartsolutions.com.cn/login"></a>
    <a href="http://imc.smartsolutions.com.cn/login"><img src="https://img.shields.io/badge/IMC-v3.5.0-brightgreen.svg"></a>	
</p>

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
