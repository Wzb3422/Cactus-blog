---
title: ❓「原码」「反码」「补码」你真的弄清了吗？
date: 2019-10-02 12:19:01
tags: Basics
---

在计算机的世界中，只有 `0` 和 `1`。那么数的存储与运算是如何在二进制的世界中进行的呢？

# 法律声明

**警告**：本作品遵循 **署名-非商业性使用-禁止演绎 4.0 未本地化版本（CC BY-NC-ND 4.0）** 协议发布。你应该明白与本文有关的一切行为都应该遵循此协议。[这是什么？](https://creativecommons.org/licenses/by-nc-nd/4.0/)

# 写在前面

阅读本文所需要的前置知识：
+ 进制转换基础
+ 基本的算术知识

# 核心内容

在对带符号数进行算术运算时，必然涉及数的符号问题。在日常书写时，我们通常在一个数的前面用 `+` 或 `-` 来表示数的正负。在数字系统中，数的正负同样是由 `0` 和 `1` 来进行表示。一般将数的最高位作为符号位，`0` 代表正 `1` 代表负，其他位作为数值位。

在计算机中，将符号和数值一起编码表示的二进制数称为**机器数**或**机器码**。常见的机器码有**原码**、**反码**和**补码**三种。

## 原码

在使用原码表示带符号二进制数时，符号位用 `0` 代表正，`1` 代表负，数值位保持不变。原码表示法又称 **符号 - 数值表示法**。

### 小数原码

例如：
+ `0.1011` 的原码是 `0.1011`
+ `-0.1011` 的原码是 `1.1011`

> 代码中的小数点 `.` 是在书写时为了清晰起见加上去的，在机器中**并不出现**。

### 整数原码

例如：
+ `1011` 的原码是 `01011`
+ `-1011` 的原码是 `11011`

容易得出，`0` 的原码有两种形式，即 `00…0` 和 `10…0` 。

### 原码的优劣

采用原码进行带符号二进制数的表示**简单易懂**，但是实现加减运算**并不方便**。

当进行两数加减运算时，需要根据两个数的符号来确定是加还是减。如果是减法，还需要根据两数的大小来确定减数与被减数，以及运算结果的符号。显然，这将增加运算的复杂性。

## 反码

使用反码进行带符号二进制数的表示时，符号位与原码相同。数值位与符号位相关。正数的数值位与原码相同，负数的数值位是真值的数值位按位取反。

### 小数反码

例如：
+ `0.1011` 的反码是 `0.1011`
+ `-0.1011` 的反码是 `1.0100`

### 整数反码

例如：
+ `1011` 的反码是 `01011`
+ `-1011` 的反码是 `10100`

同样能够得出，`0` 的反码也有两种形式，即`00…0``11…1`。

### 反码的优劣

采用反码进行加减运算时，无论是两数的加法还是减法，都可通过加法运算实现。

运算时，符号位和数值位一起参加运算。需要注意的是，**当符号位有进位产生时，应将进位加到运算结果的最低位，才能得到最终**结果。如下例：

![carbon](https://assets.wzbspace.top/img/Ones'-complement.png)

## 补码

用补码表示带符号二进制数时，符号位与原码、反码相同。数值位与符号位相关。正数的补码的数值位与真值相同，负数补码的数值位是真值数值位按位取反，再在最低位上加 `1`。

## 小数补码

例如：
+ `0.1011` 的补码是 `0.1011`
+ `-0.1011` 的补码是 `1.0101`

## 整数补码

例如：
+ `1011` 的补码是 `01011`
+ `-1011` 的补码是 `10101`

因此，整数 `0` 的补码表示法只有一种表示形式，即 `00…0`。

### 补码的优劣

采用补码进行加减运算时，均可以通过加法实现。

运算时，符号位与数值位一起参与运算，若符号位有进位产生（溢出）时，应该将进位**丢弃**，以得到正确结果。

**显然，采用补码进行加减运算最为方便。**

## 参考资料

1. 《数字逻辑（第四版） - 华中科技大学出版社》
2. [Signed number representations - Wikipedia](https://en.wikipedia.org/wiki/Signed_number_representations#Ones'_complement)