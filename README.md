> [!NOTE]
> The following content takes about 8 minutes to read. It is prepared for internal sharing and can be used freely.

## Background

理想情况下，每次版本发布时，所有配套材料都应在同一天正式上线。然而，DBR JS 的发布流程较长，前后任务相互阻塞。根据以往的发布经验，当流程进行到最后几步时，当天剩余的工作时间往往已所剩无几。

![发布流程示意图](https://cdn.nlark.com/yuque/0/2026/jpeg/22760206/1782439043768-e1a2b4ed-e135-4b3e-98bd-cf89ad66fcc5.jpeg)

## Agent Skills

### 1. Sample Deploy Helper

<!-- 痛点示意图 -->
![](https://cdn.nlark.com/yuque/0/2026/png/22760206/1782439065560-96254353-c598-41ad-8796-7c3b86ab9936.png)

**痛点：**

1. **依赖手动操作** — 存在出错风险。
2. **需提前准备部署分支** — 可能遗漏测试过程中的提交。
3. **操作耗时** — 涉及多个步骤，效率低。
4. **其他不可预见的问题** — 如合并冲突。

**现在的做法：**

GitHub samples 正式发布后，从 `main` 新建发布分支，使用 Skill 并输入提示词，**1 分钟内**即可准备好部署分支。

| **Coding Agent** | Github Copilot |
| --- | --- |
| **Model** | deepseek-v4-flash |
| **Cost** | <font style="background:#DBF1B7;color:#2A4200">0.03 CNY</font> |
| **Prompt** | <font style="color:#117CEE;">/update-sample-html-tracking-and-license</font>  prepare deploy branch for this repo. |

### 2. API Docs Auto-Updater

文档是 PM 最重要的工作之一，我们每天都在和各种文档打交道。官方文档经历过几次大型调整，也因历史原因形成了如今相对复杂的组织形态。

**痛点：**

1. **渲染差异** — 官网的网页渲染方式、IDE 预览插件、甚至 GFM（GitHub Flavored Markdown）之间都存在差异。API 文档的质量很大程度上依赖多次「上传 → 预览 → 修改 → 预览」的来回拉扯。
2. **风格不统一** — 维护人员的变更导致不同时期的文档有着不同的撰写风格和规范。

**现在的做法：**

[API docs auto-updater (screen_recording)](https://dynamsoft.sharepoint.com/sites/DynamsoftEnglishOnly/_layouts/15/stream.aspx?id=%2Fsites%2FDynamsoftEnglishOnly%2FShared%20Documents%2FPT%2DDBR%5FWeb%5FJS%2FAIAgentTool%5F260626%5Fscreen%5Frecording%2Emp4&referrer=StreamWebApp%2EWeb&referrerScenario=AddressBarCopied%2Eview%2E9dfc1c77%2D8a8b%2D4e82%2Daf90%2D0697a48de499)

| **Coding Agent** | Github Copilot |
| --- | --- |
| **Model** | deepseek-v4-flash |
| **Cost** | <font style="background:#DBF1B7;color:#2A4200">0.04 CNY</font> |
| **Prompt** | <font style="color:#117CEE;">/update-api-documentation</font>  help me update docs for this function. |

<!-- 对比示意图 -->
![](https://cdn.nlark.com/yuque/0/2026/png/22760206/1782380282705-be90684e-1fa2-4d37-a425-82e13ac87409.png)

## Discussion：为什么不进行更高级的自动化？

### 1. 快就是慢，慢就是快

> 理想的自动化流程：
> 1. 自动 fetch 最新发布版与上一个正式发布版的 `.d.ts` 文件
> 2. 自动 diff `.d.ts` type definition file
> 3. 提取全部 diff 内容，批量更新文档
> 4. 自动上传 Preview 编译

💡 **观察到的问题：**

1. **不希望对外暴露的内部方法**
   ![](https://cdn.nlark.com/yuque/0/2026/png/22760206/1782380073666-28c87dda-3a5b-413e-a82c-2ab54a97ebda.png)

2. **错误的文件 / 分支 / 仓库** — Agent 可能指向错误的上下文。
3. **幻想的内容和不存在的链接** — 模型可能生成不存在的 API、URL 或功能描述。

### 2. 去债化处理

> - 不破坏现有的组织方式，不引入新的流程和预期之外的内容
> - 理论上不会产生新债
> - 通过多次更新，过程中有机会逐步消化旧债

### 3. 可控型生成 & 可用型生成

> Skill 是约束吗？

## 平衡

+ **对"思维"的约束** — 提供确定性
+ **对"能力"的解放** — 降低复杂度
+ **核心是"对齐"**

## Reference

> - [[SKILL] update-api-documentation](https://github.com/Justin-dynamsoft/agentSkills/tree/main/skills/update-api-documentation)
> - [[SKILL] update-sample-html-tracking-and-license](https://github.com/Justin-dynamsoft/agentSkills/tree/main/skills/update-sample-html-tracking-and-license)
> - [[Github] mattpocock/skills](https://github.com/mattpocock/skills/tree/main)
