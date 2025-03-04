---
title: 'Chapter 3: Working Code Isn’t Enough'
date: '2025-02-25 22:29:14'
updated: '2025-03-04 23:58:15'
permalink: /post/chapter-3-working-code-isn-t-enough-2wnvo7.html
comments: true
toc: true
---

# Chapter 3: Working Code Isn’t Enough

（战略编程 vs. 战术编程）

---

#### **1. 战术编程（Tactical Programming）**

* **定义**：以快速完成任务（如新功能、Bug修复）为唯一目标，忽略长期代码质量。
* **问题**：

  * **短视性**（Short-sighted）：通过牺牲设计换取短期速度，导致**复杂性累积**（Complexity Accumulation）。
  * **技术债**（Technical Debt）：快速补丁（Quick Patches）使系统逐渐僵化，最终陷入“代码泥潭”。
  * **典型案例**：

    * **战术龙卷风（Tactical Tornado）** ：高产但破坏性强的程序员，代码难以维护。
    * Facebook早期文化“Move fast and break things”导致代码库混乱，最终被迫转型。

---

#### **2. 战略编程（Strategic Programming）**

* **核心理念**：代码不仅要“能用”，更要**促进长期演进**。
* **投资思维（Investment Mindset）** ：

  * **主动投资**：设计时探索多种方案、编写文档、预见扩展性。
  * **反应性投资**：持续修复设计问题而非掩盖。
* **回报曲线**：

  * 短期速度略慢（+10-20%时间），但长期效率提升（见图3.1）。
  * 例如Google、VMware通过战略编程建立高质量代码文化，吸引顶尖人才。

---

#### **3. 如何平衡投资？（How Much to Invest?）**

* **建议比例**：总开发时间的10-20%用于设计优化。
* **误区**：

  * 初创公司常误认为“快速上线”优先，忽视技术债代价。
  * 实际：代码质量直接影响招聘能力（优秀工程师倾向高质量代码环境）。

---

#### **4. 结论（Conclusion）**

* **持续投资**：设计改进需每日实践，而非推迟到“未来”。
* **文化影响**：

  * 战术编程导致恶性循环（复杂度 → 效率下降 → 更多补丁）。
  * 战略编程通过小步优化实现长期效率提升。
* **关键句**：

  > “Good design doesn’t come for free. It has to be invested in continually.”
  > （优秀设计非凭空而来，需持续投入。）
  >

---

### 核心对比表

|**维度**|**战术编程（Tactical）**|**战略编程（Strategic）**|
| --| ----------------| -------------------------|
|**目标**|快速完成任务|代码质量 + 长期可维护性|
|**复杂度管理**|累积技术债|持续简化设计|
|**开发速度**|短期快，长期慢|短期稍慢，长期更快|
|**典型案例**|早期Facebook|Google、VMware|

---

**Takeaway**：优秀软件设计的核心是**战略性投资**——宁可牺牲短期速度，换取长期灵活性与团队可持续性。
