﻿# Expression:表达式

它们都是对表达式的记法，因此也被称为前缀记法、中缀记法和后缀记法。它们之间的区别在于运算符相对与操作数的位置不同：前缀表达式的运算符位于与其相关的操作数之前；中缀和后缀同理。

举例：

```
(3 + 4) × 5 - 6 就是中缀表达式

- × + 3 4 5 6 前缀表达式

3 4 + 5 × 6 - 后缀表达式
```

## Reference

* [前缀、中缀、后缀表达式  ](http://blog.csdn.net/antineutrino/article/details/6763722)

# 中缀表达式

中缀表达式是一种通用的算术或逻辑公式表示方法，操作符以中缀形式处于操作数的中间。中缀表达式是人们常用的算术表示方法。虽然人的大脑很容易理解与分析中缀表达式，但对计算机来说中缀表达式却是很复杂的，因此计算表达式的值时，通常需要先将中缀表达式转换为前缀或后缀表达式，然后再进行求值。对计算机来说，计算前缀或后缀表达式的值非常简单。

# 前缀表达式(前缀记法、波兰式)

前缀表达式的运算符位于操作数之前。从右至左扫描表达式，遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数，用运算符对它们做相应的计算（栈顶元素 op 次顶元素），并将结果入栈；重复上述过程直到表达式最左端，最后运算得出的值即为表达式的结果。

例如前缀表达式“- × + 3 4 5 6”：

(1) 从右至左扫描，将 6、5、4、3 压入堆栈；

(2) 遇到+运算符，因此弹出 3 和 4（3 为栈顶元素，4 为次顶元素，注意与后缀表达式做比较），计算出 3+4 的值，得 7，再将 7 入栈；

(3) 接下来是 × 运算符，因此弹出 7 和 5，计算出 7×5=35，将 35 入栈；

(4) 最后是-运算符，计算出 35-6 的值，即 29，由此得出最终结果。

可以看出，用计算机计算前缀表达式的值是很容易的。

## 中缀转化为前缀

遵循以下步骤：

(1) 初始化两个栈：运算符栈 S1 和储存中间结果的栈 S2；

(2) 从右至左扫描中缀表达式；

(3) 遇到操作数时，将其压入 S2；

(4) 遇到运算符时，比较其与 S1 栈顶运算符的优先级：

(4-1) 如果 S1 为空，或栈顶运算符为右括号“)”，则直接将此运算符入栈；

(4-2) 否则，若优先级比栈顶运算符的较高或相等，也将运算符压入 S1；

(4-3) 否则，将 S1 栈顶的运算符弹出并压入到 S2 中，再次转到(4-1)与 S1 中新的栈顶运算符相比较；

(5) 遇到括号时：

(5-1) 如果是右括号“)”，则直接压入 S1；

(5-2) 如果是左括号“(”，则依次弹出 S1 栈顶的运算符，并压入 S2，直到遇到右括号为止，此时将这一对括号丢弃；

(6) 重复步骤(2)至(5)，直到表达式的最左边；

(7) 将 S1 中剩余的运算符依次弹出并压入 S2；

(8) 依次弹出 S2 中的元素并输出，结果即为中缀表达式对应的前缀表达式。

例如，将中缀表达式“1+((2+3)×4)-5”转换为前缀表达式的过程如下：

| 扫描到的元素 | S2(栈底->栈顶)         | S1 (栈底->栈顶) | 说明                             |
| ------------ | ---------------------- | --------------- | -------------------------------- |
| 5            | 5                      | 空              | 数字，直接入栈                   |
| -            | 5                      | -               | S1 为空，运算符直接入栈          |
| )            | 5                      | - )             | 右括号直接入栈                   |
| 4            | 5 4                    | - )             | 数字直接入栈                     |
| ×            | 5 4                    | - ) ×           | S1 栈顶是右括号，直接入栈        |
| )            | 5 4                    | - ) × )         | 右括号直接入栈                   |
| 3            | 5 4 3                  | - ) × )         | 数字                             |
| +            | 5 4 3                  | - ) × ) +       | S1 栈顶是右括号，直接入栈        |
| 2            | 5 4 3 2                | - ) × ) +       | 数字                             |
| (            | 5 4 3 2 +              | - ) ×           | 左括号，弹出运算符直至遇到右括号 |
| (            | 5 4 3 2 + ×            | -               | 同上                             |
| +            | 5 4 3 2 + ×            | - +             | 优先级与-相同，入栈              |
| 1            | 5 4 3 2 + × 1          | - +             | 数字                             |
| 到达最左端   | 5 4 3 2 + × 1 + -      | 空              | S1 中剩余的运算符                |

因此结果为“- + 1 × + 2 3 4 5”。

# 后缀表达式