
## 计算智能讲义——**Maxsat** 问题

计算机科学与技术学院 **M201973122** 李研

## 1 Definition

| 首先给出以下符号定义: Symbol Definition   | Description   |            |     |           |
|--------------------------------------------|---------------|------------|-----|-----------|
| i x                                        | Variable      | 变元,     | 1,  | 1,2,...,  |
| i                                          | i             |            |     |           |
| x                                          | x             | i          | n   |           |
|                                           |              |           |     |           |
|                                            | Clause        | 子句,     | ,   | 1, 2,..., |
|                                           |             |           |     |           |
| Ci                                         | C             | x          | x i | m         |
|                                           |              |           |     |           |
|                                           |             |           |     |           |
|                                           |             |           |     |           |
|                                           |              |            |     |           |
| i                                          | i             | i          |     |           |
| i S                                        | i S           |            |     |           |
|                                           |              |            |     |           |
|                                           |              |            |     |           |
| i                                          | i             |            |     |           |
| CNF                                        | Formula       | 合取范式, | ,   | 1, 2,..., |
| i                                          |               |            |     |           |
| CNF                                        | C i           | n          |     |           |
|                                           |              |            |     |           |
|                                           |               |            |     |           |
| i S                                       |               |            |     |           |

Maximum Satisfiability:找到一组 i x 的取值,使得满足的子句数目 Ci 最大。

## 2 Algorithms

| 给出以下符号定义: Symbol Definition   | Description                                          |                               |
|----------------------------------------|------------------------------------------------------|-------------------------------|
| E                                      | Z                                                   |                               |
| i                                     | Expectation                                          | 期望,对应字句Ci 被满足的期望 |
| E                                      | Z x                                                 |                               |
| |                                      | i                                                   |                               |
| Conditional Expectation                | 条件期望,在 i x 确定取值的前提下, CNF 被满足的期望 |                               |
| E                                      | Z                                                   |                               |
|                                       | Total Expectation                                    | 总期望,CNF 被满足的期望      |

   

1 1

( ) 1 2 i

m mC

i

i i

E Z E Z 

 

     。

## 2.1 Randomized Algorithm

 **算法描述:**
将 i x 分别以 1 2 的概率设置为 0 或 1,则Ci 被满足的期望为   1 2 Ci E Zi
   ,
CNF 被满足的期望为 
 **近似比分析:**
设min C K i  ,则有
**1 2 E[ ]** 
K **m Z OPT m**      

 **算法分析:**
 **简单粗暴,易于理解;** **结果不可控,近似比只是给出理论上期望的上界,而未必每次都能得到**
相应质量的解。

## 2.2 Derandomized Algorithm

 **算法描述:**
在算法①的基础上,每个变元 i x 都有 1 2 的概率取 0 或 1,即有
      1 1 **| 1 | 0**
2 2 E Z E Z x E Z x     i i 。对于每个变元 i x ,比较 
  1| 1 2 E Z xi 
  1| 0 2 E Z xi  的大小,选择二者中期望值较大者对应的 i x **取值。在此基**

和 础上,进行下一步迭代。

 **近似比分析:**
因为每一步迭代都选择了期望值较大的,所以总的条件期望 E Z x  | 要大于 随机算法的期望值 E Z  :
E[ | ] E[ ] 1 2  
K **Z x Z m**     

 **算法分析:**
 **结果可控,在变元顺序确定的情况下能保证结果一致性;** **结果与变元顺序有关,没有回溯,变元的值一旦确定便不能再更改;**
 **算法复杂度较高,条件期望的计算比较耗时;**
 可能的优化方向:使用branch and bound   **的树搜索框架,通过维护一**
个全局的条件期望实现剪枝和加速搜索。

## 2.3 Lp Rounding Algorithm

| 给出以下两个决策变量的定义: Symbol Definition   | Description        |                                    |
|--------------------------------------------------|--------------------|------------------------------------|
| i y                                              | Decision Variables | 决策变量,变元 i x 的 0/1 决策变量 |
| i q                                              | Decision Variables | 决策变量,子句Ci 的 0/1 决策变量   |

 **算法描述:**
建立 MaxSAT **混合整数规划模型如下:**

$$\begin{array}{l l}{{\mathrm{maximize:}}}&{{\sum_{i=1}^{m}q_{i}}}\\ {{\mathrm{s.t.}}}&{{q_{i}\leq\sum_{j\in S_{i}^{+}}y_{j}+\sum_{j\in S_{i}^{-}}\left(1-y_{j}\right)\quad\forall i}}\\ {{}}&{{q_{i},y_{j}\in\{0,1\}}}\end{array}$$

 **Relaxing**:
松弛最后一条布尔约束为0 , 1 i j   q y **,即得到线性规划模型。**
 **Rounding**:
求解模型,并按照下述策略设置变元取值:
j n for 1 to do 

$$\operatorname{set}x_{j}={\begin{cases}1\,\colon{\mathrm{with~probability~}}y_{j}^{*}\\ 0\,\colon{\mathrm{with~probability~}}1-y_{j}^{*}\end{cases}}$$
 Independently set **0 : with probability 1**
 end for 
 **近似比分析:**
以C x x 1 1   k 为例,利用算数-几何平均不等式,得到C1**的部分期望如下:**

$\Pr[C_{1}]=1-\prod(1-y_{j}^{*})$  $\geq1-\left(\frac{1}{k}\sum_{j=1}^{k}(1-y_{j}^{*})\right)^{k}$  $\geq1-\left(1-\frac{q_{1}^{*}}{k}\right)^{k}$  $\geq1-\left(1-\frac{1}{k}\right)^{k}$  $\geq q_{1}\left(1-\left(1-\frac{1}{k}\right)^{k}\right)$  $\geq q_{1}(1-1/e)$
 
1 1 1 1 E[ ] E 1 1 1 1 Ci K m m i i i i Z Z m   C K                                    .  则 
 **算法分析:**
 **结果相对可控,较算法①求解质量有较大提升;**
 **求解效率与线性规划求解器的性能有较大关系,随着变元数目增多,求**
解时间也逐渐延长。

## 2.4 Lp Derandomized Algorithm

 **算法描述:**
在算法③的基础上去随机化,每个变元 i x 都有 *
i y 的概率取 1, * 1 i  y **的概率取**
0,即      
* * | 1 (1 ) | 0 E Z y E Z x y E Z x        i i i i 。对于每个变元 i x ,比较
 
*| 1 i i **y E Z x**   和  
*
(1 ) | 0 i i    y E Z x **的大小,选择二者中较大的期望值,取**
其对应的 i x **取值。在此基础上,进行下一步迭代。**
 **近似比分析:**
同理,该算法的条件期望 E Z x  | 要大于算法③的期望值 E Z  :
   

$$\mathrm{E}[Z]=\sum_{i=1}^{m}\mathrm{E}\left[Z_{i}\right]\geq\sum_{i=1}^{m}\frac{1}{2}\left(\left(1-2^{-\left|C_{i}\right|}\right)+\left(1-\left(1-\frac{1}{\left|C_{i}\right|}\right)^{\left|C_{i}\right|}\right)\right)\geq(3\,/\,4)m\,,$$
$$\operatorname{E}[Z\,|\,x]\geq\operatorname{E}[Z]\geq m\left(1-\left(1-{\frac{1}{K}}\right)^{K}\right)$$

 **算法分析:**
 **结果可控,且与变元顺序无关;**
 **继承了算法②③的优缺点,既是四类算法中求解质量最好的,也是耗时**
最多的;
 **可能的优化方向:将算法②和算法④结合可以给出一个** 3 4 近似比的算法
(每次从两个算法中随机挑选一个执行):

## 3 Implementation

Github: https://github.com/lyandut/MyMaXSat
