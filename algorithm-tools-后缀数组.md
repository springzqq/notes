后缀数组以及其排序方法
=====

## 后缀数组的定义
>给定一个序列A={a0,a1,...,an-1}, 它有n个子序列{a0,a1,...,an-1}, {a1,a2,...,an-1},...,{an-1}。也就是分别以a0,a1,...,an开头，以an结尾的子序列。

>这n个序列被称为序列A的 **后缀数组**。
分别以0, 1, 2,...,n-1（也就是序列的第一个元素下标）来表示它们。

### 后缀数组的排序。
>对于两个后缀数组i, j, 当满足以下条件之一时，称作 **i的排序在j之前**：
- 存在k，使得k<=len(i), k<=len(j), a[i+k] = a[j+k] 且有 a[i+l] < a[j+l] (l<k);
- len(i) < len(j);
But I have to admit, tasks lists are my favorite:

- [x] This is a complete item
- [ ] This is an incomplete item
And, of course emoji! :sparkles: :camel: :boom:
### 后缀数组的k-前序排序
