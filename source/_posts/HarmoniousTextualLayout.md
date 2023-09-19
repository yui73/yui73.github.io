---
title: Harmonious Textual Layout Generation Over Natural Images via Deep Aesthetics Learning
date: 2023-08-01 15:52:00
tags: 论文
excerpt: 提出一种用于生成自然图像上和谐文本布局的深度美学学习方法。1. 语义视觉显著性检测网络 + 文本区域提议算法 = 生成文本锚点。2. 深度美学评分模型评估文本布局的审美质量。3. 构造了新的布局美学数据集，设计了合理的评估指标。
---

{% note info %}
发表信息

收录于：IEEE Transactions on Multimedia

{% endnote %}

## 0 介绍 Introduction

过去对于问题的Solutions：

- Text to Viz : 有许多参数需要针对文本布局和大小选择进行调整

- Content-aware generative modeling of graphic design layouts : 需要大量的数据，在视觉设计领域收集和标记高质量的数据是昂贵的。

**以前的**：强调图像元素与文本的排列关系。

但其实图像上的文本问题存在更多约束。

**约束**：1. 背景图像上的元素关系几乎不会改变。2. 对于图像上元素的标注十分昂贵。3. 因为主观性，所以评判排版结果。

**我们的工作**：

- 显著性感知文本区域建议

  视觉感知原则下，考虑了：文本位置，文本大小，图像内容。

[![流程1](https://mermaid.ink/svg/pako:eNqNlFFv0lAUx79Kc5clW1IIFFqgDz6YaWJ00WzzRfHh0pbRCC0pJRsSElA3ZCBsbjDdpjgng0WcM_iAEvHL0Fv4Fl7o1gG6hZs-3HvO_5zfOff23jjgZF4ALPAH5RUuABWVWLrplQg8pqeJSNS3rMBwgFi6_8ArXZrVWFAgRJ4i_GIwyE75fH4yoiryU4Gd8jPM-dyyIvJqgKXCqyQnB2UF-_wXOgsPIximwBhL0ARtZDZper3eaSbR14ph7w9eVAROFWWJuLdwaYX2GfS23d3aRckqOkqickX_vaW3DmYJi-UGAamZx9p-W3tR6FbXu9VML_26e5zC0iezRgpB4v9D_pWbgOzD5MwJKh6h0k-9ljWAPgxEpTQ6qPdOvmsbNXT8XM-nr6c1MxPQOEwz8u7s6Ttl9GpTP91FjaKB5fp9Jt_1khlT9A_T3NJBxFib5tJ0nleFz9rMMP5LaJt5rXCmN1p6qzyiwB8qnWmfPizcvjM3yIjytV4ypRXXte1U588hSn0bCRhVo_0f2NBfaW9yRszi3Yf63st5GA6L0rK21sDQ0QQThQyVYgTgDrDmyrKu7BxlsiiX6p62tc_p0ToyWXzq3fa2tlZB7w_1jymDafY3pv6CCoUJdNeWbEQOnMN1dZobE_eGb7YKSBASlBAUefwWxPs-L1ADQkjwAhZPecEPo0HVC7xSAkthVJUXYxIHWFWJCiSIhnmoCnMixBsUGjXe4kVVVi5sYSg9kuXQ0BKwcbAKWLuDtnqcDGWnXIybpp0uEsQA62KsHoZmXLSDdtpsDk-CBM8G4TYrVjncdrvd4XRTlIdyk0AYkOaN52zwqiX-Am4BBDI)](https://mermaid.live/edit#pako:eNqNVN9P01AU_leaS0gg6cjWrd3WBx8MmhglGsAXnQ93_cEau3bp7gKTLNlUmANkIGwooBNxbEREzHxAifOfobfdf-HdygpDIWv6cM8533e-79zb3lkg6KIEeCCr-rQQgwaiJm9GNIo8g4NUMhWdMmAiRk3efxDRztMorUqUIjKUrKgqPxCNynQSGfpTiR-QOe5s7ZlWRBTjmcQMLeiqbpCa3MV5RJgkYgZM8xRLsU5nV806ODg9zuKvVSfffkTFkASk6Bp1b_w8C31D-G3TXt3A2RrezeJK1fq9ap1sD1Mezw0KMkOPza2m-aJo1-btWqGVf23v5Qj0ybDTQtLE_yj_WupDOUqUC_u4tIvLP636oiMYJYK4nMfbB6397-ZCHe89t5bz16sdF_pQE4ia03d901qv4Fcr1uEGbpQcWaE9Z_ZdK1twQf9oulvaYVwa0w3d4pkrctZuh8ufhLmybBaPrMaJdVLpQZAXl4_MTx_Gb98Z7XTEy_VWNmeW5s213OmfHZz71kPoReOtHyTRjsw3Sw5n4u5Da_PlGEwkFG3KnGsQ0d4GfVEuWHEIZAKCudLWlZPjwiJeytmHTfNzvtdHYZGcut1cM-eq-P2O9THnaLrzXUJ_wcViH7hrLTvMTvGir9Pjhb5nI382AjSIS0YcKiK5C2bbtQhAMSkuRQBPlqIkw5SKIiCiZQg0lRAhkm6JCtINwCMjJdEAppA-kdaEbuxgRhVINi0OeBmqSZJNQO2Rrse7IBICfhbMAN7nZ0fCAY7x-YIBrz_AhGiQBnyQGwlzLBfkvCwX8DNhf4YGzzp87wgXYv0hn88bDgbCoWCYoYHUMTTmXGidey3zF3eaBLc)

- 基于美学的文本布局选择

  - 用深度学习模型提取不同文本区域的显著性特征和组成特征

  - 构造了TLA数据集，包含了图像的密集注释
  
**主要贡献**：
  
  1. 显著性文本感知方案。

  2. 基于美学的深度评分网络→感知候选文本区域之间的视觉差异。

  3. 提出合理的评估方案用于检验TLA数据集方法的有效性。

## 1 相关工作 Related

三个相关方面：自动图形设计，显著性检测，图像美学评估

## 2 方法 Methods

## 3 实验 Experiments

## 4 结论 Conclusions
