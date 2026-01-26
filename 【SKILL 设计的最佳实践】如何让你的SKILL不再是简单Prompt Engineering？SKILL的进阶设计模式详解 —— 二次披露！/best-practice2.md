<!-- vscode-markdown-toc -->
* 1. [引子](#)
* 2. [渐进式披露 & 二次披露](#-1)
	* 2.1. [模式一：附参考资料的高级指南](#-1)
	* 2.2. [模式二：领域特定组织的加载](#-1)
	* 2.3. [模式三：条件细节](#-1)
	* 2.4. [反模式：避免深度嵌套引用](#-1)
* 3. [工作流以及反馈循环](#-1)
	* 3.1. [一、使用工作清单处理复杂任务](#-1)
	* 3.2. [二、构建反馈回路](#-1)
	* 3.3. [三、设置条件工作流](#-1)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->
# 【SKILL 设计的最佳实践】如何让你的SKILL不再是简单Prompt Engineering？SKILL的进阶设计模式详解 ———— 二次披露！
之前写了一篇关于SKILL设计最佳实践的话题，感受到佬们的热情，感觉好多佬们都对SKILL设计还蛮感兴趣的，加上最近一直在对自己的一个SKILL进行调优，研究跟看了不少官方文档以及实践示例，对SKILL的设计略有心得，所以就在之前的基础上，把最近关于SKILL的进阶设计模式简单分享一下自己的理解吧，内容绝大多数参考了官方的最佳实践文档，结合自身调优的一些体验。

还是一如既往的大纲 + 一图流，方便佬们快速了解文章的大致内容以及组织形式。

【此处插入大纲】

【此处插入一图流】
##  1. <a name=''></a>引子
为了方便对SKILL还不太了解，或者没看过上期的佬们快速理解，我举一个例子来说明一下什么是SKILL。
假设你是一名喜欢到世界各地旅行，你需要准备很多装备以应对不同情况，例如登山需要（登山包、山地靴、登山杖...）, 去极地需要（防寒服、手套、护目镜），那么对于特定场景我们将所需的东西都打包起来叫做"登山套装"或者"极地套装"。这种为了应对特殊场景需求的工具套装，Claude官方把他叫做SKILL,SKILL中还包含了一些工作流，也可以理解为在特殊场景的手册，告诉你在遇到突发情况时应该怎么处理，所以通过安装SKILL的方式来使得模型在特定场景发挥强大的作用。

##  2. <a name='-1'></a>渐进式披露 & 二次披露
那么很自然的想法，SKILL这种将现成工具打包起来的做法是不是多余的呢，为什么我们不能把所有工具跟手册都带上，那么就能处理所有情况呢？
很遗憾的是目前大模型的上下文是有限的，就像如果你带上所有的工具去旅行，那么不仅很笨重（有效上下文受限），而且遇到突发情况时，从一大堆工具中找到适合的工具的难度也上升了（上下文混淆）

所以SKILL的设计理念的核心在于 **渐进式披露**，可以理解为我将所有的工具以及手册，先放到四次元口袋中，并不随身携带，但是为了能快速加载所需工具，我需要一个类似目录一样的东西来进行快速查找，而这种目录结构加载资源的方式就称为 **渐进式披露**，也就是上期讲的YAML元信息。那么你只需要带着一张记录可用工具套装的纸条（包含name以及description），在遇到特定场景的时候就可以加载对应的工具（MCP tools、脚本）以及手册（项目文档、工作流规范）

而二次披露实在渐进式披露的基础上的进阶应用，进一步提高上下文的有效利用，为此Claude 官方文档中提出三种模式来指导二次披露的使用

###  2.1. <a name='-1'></a>模式一： 附参考资料的高级指南
同样以登山包为例，在一个SKILL中可以只包含基础工具以及新的索引，当基础工具不能应对情况的时候，可以再根据索引的信息加载高级工具来处理问题。
以官方示例为例：
````markdown
---
name: pdf-processing
description: Extracts text and tables from PDF files, fills forms, and merges documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
---

# PDF Processing

## Quick start

Extract text with pdfplumber:
```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

## Advanced features

**Form filling**: See [FORMS.md](FORMS.md) for complete guide
**API reference**: See [REFERENCE.md](REFERENCE.md) for all methods
**Examples**: See [EXAMPLES.md](EXAMPLES.md) for common patterns

````
这里对于一些进阶的需求，并没有直接披露到SKILL.md的body中（就是 See xxx这些内容），因此在SKILL调用的时候，这些额外的进阶内容并不会直接加载到上下文中挤占有限的上下文空间，而当模型结合SKILL以及用户问题的时候，感知到可能需要API reference的时候，他也具备进一步读取REFFERENCE.md文档的能力，来补足上下文，这种二次披露的能力可以进一步使得SKILL的上下文得到更有效的利用，在我的实践中还发现这种能力实际上可以诞生一种能力，我暂时将其称之为sub-skill，具体的设计以及实现可以等之后发布了我的SKILL之后再详细介绍一下 嘻嘻

###  2.2. <a name='-1'></a>模式二：领域特定组织的加载
官方文档的说法是涉及多个领域的技能，应按领域组织内容，避免加载无关的上下文。例如，当用户询问销售指标时，Claude 只需要读取与销售相关的模式，而无需读取财务或市场营销数据。这样可以降低令牌使用量，并专注于上下文。
实际上就是通过文件组织的形式，实现不同场景参考资料的分离，某种程度上也是一种二次披露，每次只披露与使用者问题相关的场景上下文来补足信息。

````markdown
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)

# BigQuery Data Analysis

## Available datasets

**Finance**: Revenue, ARR, billing → See [reference/finance.md](reference/finance.md)
**Sales**: Opportunities, pipeline, accounts → See [reference/sales.md](reference/sales.md)
**Product**: API usage, features, adoption → See [reference/product.md](reference/product.md)
**Marketing**: Campaigns, attribution, email → See [reference/marketing.md](reference/marketing.md)

## Quick search

Find specific metrics using grep:

```bash
grep -i "revenue" reference/finance.md
grep -i "pipeline" reference/sales.md
grep -i "api usage" reference/product.md
```
````

预先将相对独立的上下文进行分开存储，并且使用关键词来进行索引以及引导，当需要特定领域的上下文细节的时候 使用 grep等工具实现上下文的全量加载

###  2.3. <a name='-1'></a>模式三：条件细节
这一点就更好理解了，就是在引用资源前添加条件，类似通过使用if xxx， then xxx的方式来让模型显示的知道何时该主动二次披露相关内容

###  2.4. <a name='-1'></a>反模式：避免深度嵌套引用
既然有二次披露，那么三次四次甚至更多次披露是不是能更省上下文呢，Claude的回答是：不！
原因在于虽然Claude具备嵌套引用，但是本身对于嵌套引用采取了额外的限制，例如指读取前100行的内容，因此嵌套引用可能会导致信息缺失以及逻辑结构混乱的现象，官方推荐的做法是最好指调用到二次披露，即在SKILL.md中进行索引，不要在更深的文件结构下进行索引的创建以及链接

同时，对于二次披露的参考文献应当限制长度（行数小于100行，因为claude可能只读前100行），如果超过100行，建立目录来让claude具备感知全文内容的能力（但如果目录超100行那就无话可说了 哈哈哈

##  3. <a name='-1'></a>工作流以及反馈循环
这一点我认为是进阶SKILL设计的关键，也是让SKILL能按照设计者理想情况运行的保障，首先工作流构建的重要性不言而喻，而反馈循环则体现在控制领域有个术语叫做闭环控制，说的就是当对一个复杂系统进行调整的时候需要设计反馈机制来判断此次调整是否符合预期，如果缺少闭环控制则可能导致控制系统在长时间的运转后，脱离原先的设计目标，在SKILL中也是类似。
###  3.1. <a name='-1'></a>一、使用工作清单处理复杂任务
将复杂的操作分解成清晰的、循序渐进的步骤。对于特别复杂的流程，提供一份清单，克劳德可以将其复制到回复中，并在流程进行过程中逐项**勾选**。

````markdown
## Research synthesis workflow

Copy this checklist and track your progress:

```
Research Progress:
- [ ] Step 1: Read all source documents
- [ ] Step 2: Identify key themes
- [ ] Step 3: Cross-reference claims
- [ ] Step 4: Create structured summary
- [ ] Step 5: Verify citations
```

**Step 1: Read all source documents**

Review each document in the `sources/` directory. Note the main arguments and supporting evidence.

**Step 2: Identify key themes**

Look for patterns across sources. What themes appear repeatedly? Where do sources agree or disagree?

...
````
这里非常推荐使用这种工作清单+勾选的方式，可以有效避免Claude偷懒，因为他必须做完了才能勾选下一步，这一步虽然对用户不可见，但是通过添加这个强制性的操作，会使得模型严格按照工作流组织的方式进行运行

###  3.2. <a name='-1'></a>二、构建反馈回路
这个更是关键中的关键，Claude官方也说明这种模式可以显著提高输出质量，推荐的运行模式是：运行验证器 → 修复错误 → 重复

```markdown
## Content review process

1. Draft your content following the guidelines in STYLE_GUIDE.md
2. Review against the checklist:
   - Check terminology consistency
   - Verify examples follow the standard format
   - Confirm all required sections are present
3. If issues found:
   - Note each issue with specific section reference
   - Revise the content
   - Review the checklist again
4. Only proceed when all requirements are met
5. Finalize and save the document
```
这里在每个工作流潇湘的基础上，通过添加额外的检测机制，来确保当前该步骤的完成是符合预期的，相当于强制让Claude在完成一些具有不确定性任务的时候进行二次校验。

而另一种更为有效且高明的做法是构建循环检测
```markdown
## Document editing process

1. Make your edits to `word/document.xml`
2. **Validate immediately**: `python ooxml/scripts/validate.py unpacked_dir/`
3. If validation fails:
   - Review the error message carefully
   - Fix the issues in the XML
   - Run validation again
4. **Only proceed when validation passes**
5. Rebuild: `python ooxml/scripts/pack.py unpacked_dir/ output.docx`
6. Test the output document
```
这里在第4步的时候设置了关键检查点，当不满足条件时必须重新校验

###  3.3. <a name='-1'></a>三、设置条件工作流
如果说上面构建返回回路是 for 或者 while的话，条件工作流就相当于是 if 了
```markdown
## Document modification workflow

1. Determine the modification type:

   **Creating new content?** → Follow "Creation workflow" below
   **Editing existing content?** → Follow "Editing workflow" below

2. Creation workflow:
   - Use docx-js library
   - Build document from scratch
   - Export to .docx format

3. Editing workflow:
   - Unpack existing document
   - Modify XML directly
   - Validate after each change
   - Repack when complete
```
通过这种方式，可以实现工作流的切换，也就是在上面的线性（循环）工作流的基础上添加了分支选项，也算是小小的微创新了一下