后缀数组以及其排序方法
=====

## 后缀数组的定义
> 给定一个序列A={a<sub>0</sub>, a<sub>1</sub>, ..., a<sub>n-1</sub>}, 它有n个子序列{a<sub>0</sub>, a<sub>1</sub>, ..., a<sub>n-1</sub>}, {a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n-1</sub>}, ..., {a<sub>n-1</sub>}。也就是分别以a<sub>0</sub>, a<sub>1</sub>, ..., a<sub>n</sub>开头，以a<sub>n</sub>结尾的子序列。

> 这n个序列被称为序列A的 **后缀数组**。
分别以0, 1, 2, ..., n-1（也就是序列的第一个元素下标）来表示它们。

### 后缀数组的排序。
> 对于两个后缀数组i, j, 当满足以下条件之一时，称作 **i的字母排序在j之前**：
- 存在m (m < len(i)且m < len(j)), 使得 a<sub>i+m</sub> < a<sub>j+m</sub> 且有 a<sub>i+k</sub> = a<sub>j+k</sub> \(k<m);
- len(i) < len(j);

### 后缀数组的k-前序排序
> 对于A的后缀数组集合，按照其开始的k个元素进行字母序排序，称作数组的k-前序排序

### 位置索引SA 
对于已拍好需的后缀数组，SA<sub>k</sub>表示对后缀数组进行k前缀排序后的结果。
> SA<sub>k</sub>[i] 表示：排序后排在i位置的后缀数组在原数组中的索引（后缀数组的第一个元素）。

### 排序位置值 
> rank<sub>k</sub>[i] 表示：排序后后缀数组i的rank值。

## 排序算法基本思路
- 如果rank<sub>k</sub>[i] < rank<sub>k</sub>[j]，则rank<sub>2k</sub>[i] < rank<sub>2k</sub>[j]
- 如果rank<sub>k</sub>[i] = rank<sub>k</sub>[j]，且rank<sub>k</sub>[i+k] < rank<sub>k</sub>[j+k]，则rank<sub>2k</sub>[i] < rank<sub>2k</sub>[j]
- 如果rank<sub>k</sub>[i] = rank<sub>k</sub>[j]，且rank<sub>k</sub>[i+k] = rank<sub>k</sub>[j+k]，则rank<sub>2k</sub>[i] = rank<sub>2k</sub>[j]

实际上，在对后缀数组进行2k-前序排序时，可以看成先做k-前序排序，再对后偏移k个位置的后缀数组进行k-前缀排序（双关键字排序）。当前，数组已经是按照k-前序排序了，所以我们搞定偏移k个位置的k-前序排序即可。

## 后缀数组排序算法(倍增法)
1. 对数组元素进行排序，得到SA<sub>1</sub>；
2. 排在第一位(SA<sub>1</sub>[0])的，将其rank值置为0。对排序结果进行整理，将rank值离散化。
3. 在已经得到SA<sub>k</sub>时，和rank<sub>k</sub>时，计算SA[sub]2k[/sub]。
4. 由SA[sub]2k[/sub]得到rank<sub>2k</sub>。
5. 重复3，4，直到2k>n。
6. 此时数组的2k前序排序下已经是顺序的了。

## 后缀数组排序算法的程序实现
### 输入
```c
(int *r, int n)
```
### 变量定义和基本函数定义
```c
int y[N] = {0};       //用来保存排序结果
int sa[N] = {0};      //用来保存排序索引
int rank[N] = {0};    //用来保存离散化的各后缀数组排序位置，以0开始，最大值小于N
void cp(int i, int j) { return r[i]<r[j];}             //用来进行排序
void cmp(int i, int j, int p){ return rank[i]==rank[j]&&rank[i+p]==rank[j+p];}  //用来在离散化的过程中判断相邻的两个数rank值是否相同
```
### 程序实现
初始化
```c
for (int i = 0; i < len; i++) y[i] = i;           //初始化y, 顺序的0,1,2,3,4...n-1
for (int i = 0; i < len; i++) sa[i] = i;          //初始化sa，rank，顺序的0,1,2,3,4...n-1
sort(y, y+n, cp);                                 //对y进行排序，排序依据为y位置的值（后缀数组的起始元素）
```
此过程后，y序列即为sa序列，接下来对排序结果进行离散化
```c
rank[y[0]]=0;                                       //排在最前面的后缀数组y[0]，其rank值为0
int j = 0;                                          //j表示当前rank取值
for(int i = 1; i < n ; i++){ 
    if (r[sa[i]=y[i]] == r[y[i-1]]) rank[y[i]] == p;//如果y[i]和y[i-1]处的值（此时是r值）相同，则rank值也相同。
    else rank[y[i]] = ++p;
}
```
此过程后，我们已经完成了前序k=1的所有工作，即对所有的后缀数组完成了***1-前序排序***。
接下来我们要根据***k-前序排序结果***，完成***2k-前序排序结果***。
```c
for(int k = 1; k < n; k *=2){
//根据定义，下标为n-k至n-1的k个数组，其长度<=k，不需要对其进行偏移k个位置的k-前缀排序，直接将其放在第二关键字排序结果的前k位。
    for(int i = 0; i < k; i++){
        y[i] = n - k + i;
    }

    //可以将sa直接作为偏移k个位置的排序结果，将处理次序赋予原索引
    int j = k;
    for(int i = 0; i < n; i++){
        if (sa[i]>k)  y[j++] = sa[i] - k;            //此步骤得到了后缀数组0至n-k在偏移k个位置后的k-前序排序
    }
    //最难写的就是这一段，如何按照偏移k的位置的结果重新排序
    //我们按照 按第二关键字排序后得到的结果(y)来依次处理。
    for(int i = 0; i<n; i++){
        ts[rank[i]]++;                               //将第一关键字的rank值量化。rank[0]多少个，rank[1]多少个... 
    }
    ts[0]=0;
    for(int i = 1; i<n; i++){
        ts[i] += ts[i-1];                            //将第一关键字的rank值阶梯化。ts[i]:小于等于i的有多少个. 
    }                                                //最终排序在此之前的有多少个。
    
    //对排序结果进行离散化，基本思路与k=1时的一致
    //只是判断相等的时候，除了要判断k-前序rank外，还要判断偏移k后的k-前序位置
    rank[y[0]]=0;                                    //排在最前面的后缀数组y[0]，其rank值为0
    int p = 0;                                       //p表示当前rank取值
    for(int i = 1; i < n ; i++){ 
        if (cmp(i, i-1, k)) x[y[i]] == p;            //如果y[i]和y[i-1]处的值（cmp返回结果）相同，则rank值也相同。
        else x[y[i]] = ++p;
    }
```
