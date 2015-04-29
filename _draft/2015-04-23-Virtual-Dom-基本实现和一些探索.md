---
layout: default
title: Virtual-Dom-基本实现和一些探索
comments: true
tags: notes
---

# Virtual-Dom-基本实现和一些探索

实际上，Virtual DOM包含：
1. Javascript DOM模型树（VTree），类似文档节点树（DOM）
2. DOM模型树转节点树方法（VTree -> DOM）
3. 两个DOM模型树的差异算法（diff(VTree, VTree) -> PatchObject）
4. 根据差异操作节点方法（patch(DOMNode, PatchObject) -> DOMNode）
