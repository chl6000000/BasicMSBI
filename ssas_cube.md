# 多维数据集 基本概念介绍 #

## 成员 ##
Member, A single item from a dimension.
成员是维度中的一个项目，表示数据的一次或多次出现。可将维度中的成员看作基础数据库中的一个或多个记录，该记录在此列中的值属于此类别。成员是描述多维数据集中的单元数据时的最低级别的引用。
可以用成员名称或成员键引用某个成员。在上一示例中，用成员在 Time 维度中的名称 4th quarter 来引用该成员。但是，如果维度不具有非唯一的成员名称，则成员名称可以重复，也可以更改渐变维度中的成员名称。
引用成员的另一种方法是引用成员键。维度使用成员键明确标识特定成员。在 MDX 中，“与”符号 (&) 用于区分成员键和成员名称。例如，以下引用使用 4th quarter 成员的成员键 Q4： 
```
[Time].[2nd half].&[Q4]
```


## 元组 ##
Tuple, One member from each dimension. defined by slicer specification and row/column set. 
包含在多维数据集中的数据元素称为“单元”。通过对多维数据集中包含的每个属性层次结构指定一个成员可以唯一地标识一个单元。标识一个单元的属性的组合称为“元组”。
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
　

## 集 ##
Set, Multiple Members on an Axis.
集是零个、一个或多个元组的有序集合。集最常用于定义 MDX 查询中的查询轴和切片器轴，因此可以只有一个元组，在某些情况下，也可以为空。下面的示例显示了具有两个元组的集：
```
{ (Time.[1st half], Route.nonground.air), (Time.[2nd half], Route.nonground.sea) }
```

## 轴 ##
Axis, The Row and Column Headings of a report.

## 切片 ##
Slicer, The Filter Specification for a report.

## 运算式 ##
MDX Expression, 
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
### 成员函数 ###
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
	
#### Ancestor ####
	*Returns the ancestor of a member*
		at a specified level
		at a specified distnace in the hierarchy
	*Equivalent to parent function when distance =1*

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

