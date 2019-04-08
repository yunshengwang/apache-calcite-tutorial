<!--推荐阅读[《数据库查询优化器的艺术》](https://item.jd.com/11389754.html)-->


# 关系代数解释
[关系代数](https://zh.wikipedia.org/wiki/%E5%85%B3%E7%B3%BB%E4%BB%A3%E6%95%B0_%28%E6%95%B0%E6%8D%AE%E5%BA%93%29)
[relational algebra](https://en.wikipedia.org/wiki/Relational_algebra)

# 常用关系代数

## 选择(selection) δ
例子:
| A | B  | C | D|
|--|--|--|--|
| a | a | 1 | 3 |
| a | b | 1 | 5 |
| b | b | 2 | 7 |
| b | b | 3 | 7 |
表达式: δ (A=B) ^ (D>5) (A等于B并且D>5)
| A | B  | C | D|
|--|--|--|--|
| b | b | 2 | 7 |
| b | b | 3 | 7 |

一般选择的谓词包括:
 ∧ (and), ∨ (or), ¬ (not)
 
 =, ≠, >, ≥. <. ≤
## 投影(projection) π
例子:
| A | B  | C | D|
|--|--|--|--|
| a | a | 1 | 3 |
| a | b | 1 | 5 |
| b | b | 2 | 7 |
| b | b | 3 | 7 |

表达式:πA,C

| A | C |
|--|--|
| a | 1 |
| b | 2 |
| b | 3 |

投影运算需要在结果集中删除重复行
因重复行被删除
| A | C |
|--|--|
| a | 1 |

## 笛卡尔积(cartesian product ) ×
例子
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| b | 3 |
s表
| C| D |
|--|--|
| a | 1 |
| b | 3 |

r × s 求笛卡尔积
| A | B  | C | D|
|--|--|--|--|
| a | 1 | a | 1 |
| a | 1 | b | 3 |
| b | 2 | a | 1 |
| b | 2 | b | 3 |
| b | 3 | a | 1 |
| b | 3 | b | 3 |

注意:
* 求笛卡尔积时有重复字段必须重命名


## 并集(set union) U
例子
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| b | 3 |
s表
| A| B |
|--|--|
| a | 1 |
| b | 7 |

r U s 求并集
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| b | 3 |
| b | 7 |

交集的两个条件
* r和s必须有相同的列数
* r和s对应的列类型相同
## 差集(set difference) -
例子
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| b | 3 |
s表
| A| B |
|--|--|
| a | 1 |
| b | 7 |
r - s 求差集
| A | B |
|--|--|
| b | 2 |
| b | 3 |

差集的两个条件
* r和s必须有相同的列数
* r和s对应的列类型相同
## 重命名(rename) ρ
例子
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| b | 3 |

 ρ r(A',B)  r表A列重命名为A'
 | A' | B |
|--|--|
| a | 1 |
| b | 2 |
| b | 3 |
#  其他操作
## 链接运算
### 自然连接 (Natural join) ⋈
例子1
R = (A, B, C, D)
S = (E, B, D)
Result schema = (A, B, C, D, E)

例子2
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| b | 3 |

s表
| A | C |
|--|--|
| a | j |
| b | q |

r ⋈ s

| A | B | C |
|--|--|--|
| a | 1 | j |
| b | 2 | q |
| b | 3 | q |

### 相等连接 (equijoin)
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| b | 3 |

s表
| A | C |
|--|--|
| a | j |
| b | q |

r ⋈ s

| A | B | C |
|--|--|--|
| a | 1 | j |
| b | 2 | q |
| b | 3 | q |
### θ-join (theta join)
θ是在集合{<, ≤, =, >, ≥}中的二元关系

r表
| A | B |
|--|--|
| a | 2 |
| b |  3|
| b | 4 |

s表
| C | D |
|--|--|
| j|  2|
| q | 3 |
| k | 4 |
r ⋈ s B>D
| A | B | C | D |
|--|--|--|--|
| b |  3| j | 2 |
| b | 4 | j | 2 |
| b | 4 | q | 3 |
### 半链接(Semijoin) ⋉/⋊
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| c | 3 |

s表
| A | C |
|--|--|
| a | j |
| b | q |
| e | q |

r ⋉ s
| A | B | C |
|--|--|--|
| a | 1 | j |
| b | 2 | q |



### 反连接 Antijoin ▷
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| c | 3 |

s表
| A | C |
|--|--|
| a | j |
| b | q |
| e | q |

r ▷ s 
| A | B |
|--|--|
| c | 3 |

### 外链接 Outer joins
数据库语言SQL所定义的NULL，所以衍生以下链接操作

#### 左外链接 Left outer join (⟕)
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| c | 3 |

s表
| A | C |
|--|--|
| a | j |
| b | q |
| e | q |

r ⟕ s
| A | B | C |
|--|--|--|
| a | 1 | j |
| b | 2 | q |
| c | 3 | NULL |

#### 右外链接 Right outer join (⟖)
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| c | 3 |

s表
| A | C |
|--|--|
| a | j |
| b | q |
| e | q |

r ⟖ s
| A | B | C |
|--|--|--|
| a | 1 | j |
| b | 2 | q |
| e | q | NULL |
#### 全链接 Full outer join (⟗)
r表
| A | B |
|--|--|
| a | 1 |
| b | 2 |
| c | 3 |

s表
| A | C |
|--|--|
| a | j |
| b | q |
| e | q |

r ⟖ s
| A | B | C |
|--|--|--|
| a | 1 | j |
| b | 2 | q |
| c | 3 | NULL |
| e | q | NULL |

## 除 Division (÷)

r表
| A | B |
|--|--|
|s1 |p1
|s1 |p2
|s1 |p3
|s1 |p4
|s2 |p1
|s2 |p2
|s3 |p2
|s4 |p2
|s4 |p4

s1表
| B |
|--|
| p2|

s2表
| B |
|--|
| p2
|p4

r ÷ s1
|A
|--|
|s1
|s2
|s3
|s4
r ÷ s2
|A
|--|
|s1
|s4
## 聚集运算
* 求和 Sum
* 计数 Count
* 平均 Average
* 最大 Maximum
* 最小 Minimum
