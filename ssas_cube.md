# 多维数据集 基本概念介绍 #
## 维度 ##
数据中公司要分析的元素

## 成员 ##
Member, A single item from a dimension.
成员是维度中的一个项目(一个维度中的一个点)，表示数据的一次或多次出现。可将维度中的成员看作基础数据库中的一个或多个记录，该记录在此列中的值属于此类别。成员是描述多维数据集中的单元数据时的最低级别的引用。
可以用成员名称或成员键引用某个成员。比如，用成员在 Time 维度中的名称 4th quarter 来引用该成员。但是，如果维度不具有非唯一的成员名称，则成员名称可以重复，也可以更改渐变维度中的成员名称。
引用成员的另一种方法是引用成员键。维度使用成员键明确标识特定成员。在 MDX 中，“与”符号 (&) 用于区分成员键和成员名称。例如，以下引用使用 4th quarter 成员的成员键 Q4： 
```
[Time].[2nd half].&[Q4]
```
### 成员函数 ###
成员值：一个成员的一个独特特性
CurrentMember, Parent, PrevMember, ParallelPeriod, Ancestor
#### CurrentMember ####
	*Returns the current member of a dimension*
	*is label on row or column, or filter label*
	*is default function for a dimension*
	*appears in member group of functions list*
	*un-assign means All Member in All Level*
#### Parent ####
	*Returns the parent of the specified member*
	*often follows the currentmember function*
	*appears in the member group of the functions list*
#### PrevMember ####
	Returns previous member on same level
	Can be used with any dimension, but is typically used with time dimension
	Appears in the member group of the function list
#### ParallelPeriod ####
	return a member of a previous member with the same relative position.
	default value,
		if no specify hierarchy or member expression, default value is Time type dimension, Time.CurrentMember.
		If point specify hierarchy expression, but not point specify member expression,
		default value is Level_Expression.Hierarchy.CurrentMember.
		Default index value is 1. 
		Default hierarchy is the hierarchy of specify member ' parent. 
#### Ancestor ####
	*Returns the ancestor of a member*
		at a specified level
		at a specified distnace in the hierarchy
	*Equivalent to parent function when distance =1*

## 属性 ##
一个维度的类似成员集合

## 维度层次 ##
维度成员的有序结构

## 维度长度 ##
维度包含的成员个数

## 元组 ##
Tuple, One member from each dimension. defined by slicer specification and row/column set. 
包含在多维数据集中的数据元素称为“单元”，是多维空间中的一个坐标。通过对多维数据集中包含的每个属性层次结构指定一个成员可以唯一地标识一个单元。标识一个单元的属性的组合称为“元组”。
元组标识多维数据集中的单元。一个元组由多维数据集中每个层次结构中的一个成员组成（显式或隐式引用）。如果特定层次结构中的成员没有在元组中显式引用，则该层次结构中的默认成员将隐式包含在元组中。 
在 MDX 中，元组根据其复杂性依照语法进行构造。如果元组只由一个层次结构中的一个成员组成（通常称为“简单元组”），则下列语法是可以接受的：
```
Time.[2nd half] 
```
　　例如，下面的元组标识了上图中值为 240 的一个单元（因为这里有四个维度，所以四维定义一个元组）：
```
( Source.[Eastern Hemisphere].Africa, 
	　　Time.[2nd half].[4th quarter],
	　　 Route.Air,
	　　 Measures.Packages)
```

正如可以指定从关系数据库的表中检索多组列或行一样，您可以指定从多维数据集中检索一组元组。MDX 中用来指一个有序的元组集合的标识符称为“集”。下面的示例标识了上图所示的多维数据集中的一个元组集：

```
{ (Time.[1st half].[1st quarter]),
　　 Time.[2nd half].[3rd quarter]) }
```
　
## 度量值 ## 
单元格的值,要分析的值，用多维数据集中与维度关系组合成度量值组。这些值是基于要分析的事实数据。
度量值定义哪些数据可供分析，用什么形式，按什么规则变换数据。

## 度量值组 ##
度量值组定义数据如何绑定到多维数据集的多维空间。
### AggregateFunction ### 
确定如何从事实空间数据值计算多维空间的数据。
累加聚合：sum, count, Min & Max
非累加聚合：DistincCount, None
半累加聚合：FirstChild, LastChild, FirstNonEmpty, LastNonEmpty, AverageOfChildren & ByAccount
### Granularity(粒度) ###
粒度是度量值组的属性，定义其长度，复杂度和与多维数据集的绑定方法。

## 度量值组维度 ##
是属于度量值组（事实）的一列多维数据集维度。用多维数据集维度的ID引用。如果多维数据集中维度列表与度量值组中的维度列表相同，则度量值组的维度性与多维数据集相同。这时多维数据集中所有的维度定义数据如何载入度量值组。无关的维度不定义数据如何载入度量值组。

度量值组维度分为两种：
1. 直接维度，直接与事实相关系
2. 间接维度，通过其他维度或度量值组关联到事实
	a, 引用维度， 相对直接维度多了两个属性：intermediateCubeDimension, intermediateGranularityAttribute
	b, 多对多维度
	c, 数据挖掘维度
	

## 集 ##
Set, Multiple Members on an Axis.
集合是由多个元组组成，多个元组以逗号分隔，前后以{}包含起来。
若集合中只有一个元组则可以省略{}。
一个集合内所有的元组必须具有相同维度性。
集是零个、一个或多个元组的有序集合。集最常用于定义 MDX 查询中的查询轴和切片器轴，因此可以只有一个元组，在某些情况下，也可以为空。下面的示例显示了具有两个元组的集：
```
{ (Time.[1st half], Route.nonground.air), (Time.[2nd half], Route.nonground.sea) }
```

## 子多维数据集 ## 
多维数据集完全空间的一部分


## 轴 ##
Axis, The Row and Column Headings of a report.
Statement can show 128 axis, but only display two dimension in page. 
Columns =Axis(0); Rows =Axis(1)
Axis must be use Set.
Use Set two display multi members in Axis.
It can be one or multi tuple
one dimension(hierarchy) only visible in one Axis. 


## 切片 ##
Slicer, The Filter Specification for a report. 多维空间中可以用元组定义的一段。

SELECT {Set} ON Axes FROM Cube WHERE Tuple 

## 运算式 ##
MDX Expression, 
### 累加函数 ###
可以从事实空间中单元格的值计算逻辑空间中单元格的值

### 字符串函数 ###
Name, Properties
#### Name ####
	*Returns member name as a string*
	*Has Three versions*
		Name: Dimension or hierarchy
		Name: Level
		Name: Member
	*Appears in string group of functions list*
#### Properties ####
	*Returns a member property*
	*Requires property name in Quotation marks*
	*Returns an error if property name does not exist*
	*Returns property value as a string even if it is a number*
	*default member perperty include Name, ID, key, caption etc.*


### 逻辑函数 ###
IsEmpty
#### IsEmpty ####
	Returns true if specified value is empty
	Use to avoid division by zero and other errors
	requires two Levels of parentheses if expression is tuple with multiple dimensions
	appears in the logical group of the functions list
### 阶层函数 ###
Level, Hierarchy, Dimension
#### Level ####
	*Returns Level of a member*
	*often follows the currentMember function*
	*can be followed by the name function*
	*appears in the level group of the functions list*
### EXCEL & VBA函数 ###
VAL, IIF
#### VAL ####
	*Converts a string to a number*
	*is useful for converting a member property to a number*
	*is a VBA Function, not an MDX function*
	*VBA and EXCEL functions are automatically available*
#### IIF ####	
	*Returns one of two values based on conditional expression*
	*Returns first value if true, second value if false*
	*IIF stands for immediate IF, similar to VBA IIF Function or EXCEL IF Function*
	*Both expressions must be same data type*

## 多维数据库存储模式 ##
SSAS 将多维数据集的数据存放在分区中，处理多维数据集时，将数据载入其所有分区中，分区有三种存储模式：
### 关系型系统（ROLAP） ### 
不直接在多维数据库中存储数据，在处理期间只是检查分区元数据的一致性，数据在需要时从关系数据库中载入。
### 多维系统（MOLAP） ### 
将数据载入多维数据库中缓存，并建立索引和聚合，有查询请求在缓存中查询数据。
### 混合系统（HOLAP） ### 
在多维数据库中缓存汇总数据，建立聚合。需要更多细节信息，就从关系数据库载入数据。
