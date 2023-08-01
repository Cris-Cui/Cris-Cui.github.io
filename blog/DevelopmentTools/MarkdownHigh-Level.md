# Markdown 高阶语法

# 警告框语法

## 几种显示用法[¶](https://lifeinzucc.github.io/help/markdown/admonition/#_1)

### 普通用法[¶](https://lifeinzucc.github.io/help/markdown/admonition/#_2)

代码:

```
!!! note
    今天风和日丽。
```

结果:

Note

今天风和日丽。

### 带标题用法[¶](https://lifeinzucc.github.io/help/markdown/admonition/#_3)

代码:

```
!!! note "Jaycee Chow的日记"
    今天风和日丽。
```

结果:

Jaycee Chow的日记

今天风和日丽。

### 无辩题用法[¶](https://lifeinzucc.github.io/help/markdown/admonition/#_4)

代码:

```
!!! note ""
    今天风和日丽。
```

结果:

今天风和日丽。

### 可折叠用法[¶](https://lifeinzucc.github.io/help/markdown/admonition/#_5)

代码:

```
??? note
    你看不见我。
```

结果:

<details class="note" style="box-sizing: inherit; box-shadow: rgba(0, 0, 0, 0.14) 0px 2px 2px 0px, rgba(0, 0, 0, 0.12) 0px 1px 5px 0px, rgba(0, 0, 0, 0.2) 0px 3px 1px -2px; position: relative; margin: 1.5625em 0px; padding: 0px 0.6rem; border-left: 0.2rem solid rgb(68, 138, 255); border-radius: 0.1rem; font-size: 0.64rem; overflow: auto; display: block; color: rgba(0, 0, 0, 0.87); font-family: Roboto, &quot;Helvetica Neue&quot;, Helvetica, Arial, sans-serif; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary style="box-sizing: inherit; display: block; outline: none; cursor: pointer; margin: 0px -0.6rem; padding: 0.4rem 2rem; border-bottom: none; background-color: rgba(68, 138, 255, 0.1); font-weight: 700;">Note</summary><p style="box-sizing: inherit; margin: 1em 0px 0.6rem;"></p></details>

## 几种显示类型[¶](https://lifeinzucc.github.io/help/markdown/admonition/#_6)

### Note[¶](https://lifeinzucc.github.io/help/markdown/admonition/#note)

代码:

```
!!! note
    今天风和日丽。
```

结果:

Note

今天风和日丽。

note也可以用seealso替换。

### Abstract[¶](https://lifeinzucc.github.io/help/markdown/admonition/#abstract)

代码:

```
!!! abstract
    很久很久以前，巨龙突然出现，带来灾难，带走了公主又消失不见。王国十分危险，世间谁最勇敢，一位勇者赶来，大声喊，我要带上最好的剑，翻过最高的山，闯进最深的森林，把公主带回到面前。
```

结果:

Abstract

很久很久以前，巨龙突然出现，带来灾难，带走了公主又消失不见。王国十分危险，世间谁最勇敢，一位勇者赶来，大声喊，我要带上最好的剑，翻过最高的山，闯进最深的森林，把公主带回到面前。

abstract也可以用summary或者tldr替换。

### Info[¶](https://lifeinzucc.github.io/help/markdown/admonition/#info)

代码:

```
!!! info
    操作系统作业没做完。
```

结果:

Info

操作系统作业没做完。

info也可以用todo替换。

### Tip[¶](https://lifeinzucc.github.io/help/markdown/admonition/#tip)

代码:

```
!!! tip
    程序员务必多喝芝麻，这样头发才会茂密。
```

结果:

Tip

程序员务必多喝芝麻，这样头发才会茂密。

tip也可以用hint或者important替换。

### Success[¶](https://lifeinzucc.github.io/help/markdown/admonition/#success)

代码:

```
!!! success
    今日份的崩坏已肝完。
```

结果:

Success

今日份的崩坏已肝完。

success也可以用check或者done替换。

### Question[¶](https://lifeinzucc.github.io/help/markdown/admonition/#question)

代码:

```
!!! question
    先有鸡还是先有蛋？
```

结果:

Question

先有鸡还是先有蛋？

question也可以用help或者faq替换。

### Warning[¶](https://lifeinzucc.github.io/help/markdown/admonition/#warning)

代码:

```
!!! warning
    warning: ignoring return value of ‘scanf’, declared with attribute warn_unused_result
```

结果:

Warning

warning: ignoring return value of ‘scanf’, declared with attribute warn_unused_result

warning也可以用caution或者attention替换。

### Failure[¶](https://lifeinzucc.github.io/help/markdown/admonition/#failure)

代码:

```
!!! failure
    Exception thrown  :java.lang.ArrayIndexOutOfBoundsException: 3
```

结果:

Failure

Exception thrown :java.lang.ArrayIndexOutOfBoundsException: 3

failure也可以用fail或者missing替换。

### Danger[¶](https://lifeinzucc.github.io/help/markdown/admonition/#danger)

代码:

```
!!! danger
    高压电
```

结果:

Danger

高压电

danger也可以用error替换。

### Bug[¶](https://lifeinzucc.github.io/help/markdown/admonition/#bug)

代码:

```
!!! bug
    404 GET
```

结果:

Bug

404 GET

无其他关键词可替换

### Example[¶](https://lifeinzucc.github.io/help/markdown/admonition/#example)

代码:

```
!!! example
    Admonition扩展语法关键词包括：

    Note，Abstract，Info，Tip，Success，Question，Warning，Failure，Danger，Bug，Example，Quote
```

结果:

Example

Admonition扩展语法关键词包括：

Note，Abstract，Info，Tip，Success，Question，Warning，Failure，Danger，Bug，Example，Quote

example也可以用snippet替换。

### Quote[¶](https://lifeinzucc.github.io/help/markdown/admonition/#quote)

代码:

```
!!! quote
    Jeff Atwood: 一切能被JavaScript实现的终将会被JavaScript实现。 
```

结果:

Quote

Jeff Atwood: 一切能被JavaScript实现的终将会被JavaScript实现。