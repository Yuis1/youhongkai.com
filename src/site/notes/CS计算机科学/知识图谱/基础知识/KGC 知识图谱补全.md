---
{"dg-publish":true,"permalink":"/CS计算机科学/知识图谱/基础知识/KGC 知识图谱补全/","noteIcon":"","created":"2024-08-28T17:26:35.000+08:00","updated":"2024-04-24T00:28:49.000+08:00"}
---


基于结构的KGC和基于描述的KGC是两种主要的方法，分别侧重于利用知识图谱中的结构信息和文本描述信息来预测缺失的事实。

### 基于结构的KGC

基于结构的KGC方法主要侧重于利用知识图谱中的图结构信息。这些方法通常使用实体和关系之间的已知三元组（例如，(头实体, 关系, 尾实体)）来学习实体和关系的嵌入表示。这种方法的核心思想是将实体和关系映射到一个连续的向量空间中，通过这些向量的几何关系来推断可能的链接。

典型的基于结构的KGC方法包括：

- **TransE**：将关系视为头实体和尾实体向量之间的翻译，即寻求\( \textbf{h} + \textbf{r} \approx \textbf{t} \)。
- **DistMult**：使用简单的双线性模型，专注于三元组的对称性。
- **ConvE**：使用卷积神经网络来学习实体和关系之间复杂的交互模式。
- **RotatE**：通过在复数域中进行旋转操作来捕捉实体和关系之间的对称性、反对称性和反转性。

这些模型的优势在于能够直接从图结构中学习，并且通常计算效率较高。然而，它们的主要局限性在于难以处理丰富的文本信息，如实体的描述或属性，这可能限制了模型对复杂关系的理解能力。

### 基于描述的KGC

与基于结构的方法不同，基于描述的KGC方法利用实体和关系的文本描述来学习它们的嵌入表示。这些方法通常依赖于预训练的语言模型（如BERT、GPT等），以捕获文本数据中的语义信息。

例如：

- **DKRL**：使用卷积神经网络处理实体描述，以融合结构化和非结构化知识。
- **KG-BERT**：使用BERT模型来处理实体和关系的文本描述，并通过计算文本嵌入之间的相似度来预测缺失的链接。
- **KEPLER**：结合知识嵌入和语言预训练，同时优化知识图谱的表示和语言模型的目标。

基于描述的KGC方法的优势在于能够利用丰富的文本信息，这有助于模型更好地理解实体和关系的含义，尤其是在实体描述信息丰富时。然而，这类方法的挑战在于处理大量文本数据的计算成本较高，且对于描述质量高度敏感。

总体来说，选择哪种方法取决于特定任务的需求和可用数据的特点。在实际应用中，结合这两种方法的混合模型通常能提供更全面的性能。