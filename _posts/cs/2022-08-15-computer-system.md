---
layout: post
title:  "计算机漫游"
date:   2022-08-15 11:11:00 +0800
categories: 个人学习
tags : computer
---
1. Amdahl 定律
- 要想显著加速整个系统必须提升系统中相当大部分的速度  
- 加速比的表示方法  S = $T_{old}$ / $T_{new}$ =1/(1-α)+α/k)
- 推导
  - 系统执行应用时间为 $T_{old}$
  - 某部分执行时间占总执行应用的时间为 α
  - 该部分提升比例为 k,即经过优化后 
  - 该部分执行初始时间为 α*$T_{old}$
  - 优化后所需时间为 α*$T_{old}$/k
  - 优化后应用程序执行时间=未优化部分执行时间+优化后执行时间
  - $T_{new}$=(1-α)$T_{old}$+α*$T_{old}$/k