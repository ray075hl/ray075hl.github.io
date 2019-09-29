---
layout: post
date: 2019-09-29 18:00:00
img: sparse_matrix.jpg
tags: [machine learning]
title: py2neo中基本数据结构
---



### 基本数据类 Node， Relationship

```python
class py2neo.data import Node, Relationship
a = Node("Person", name="Alice") # Person是label, name是属性
b = Node("Person", name="Bob"
ab = Relationship(a, "KNOWS", b)
```



#### Node类，节点类

基本操作

| 操作符号                   | 含义                                                 |
| -------------------------- | ---------------------------------------------------- |
| ==                         | node的ID相等，就说两个node相等，不需要属性值都相等   |
| ！=                        | 不相等                                               |
| hash(node)                 | 返回node的ID                                         |
| node[key]                  | 返回key这个属性的值                                  |
| node[key]=value            | 赋值                                                 |
| del node[key]              | 删除节点属性                                         |
| len(node)                  | 返回node的属性个数                                   |
| dict(node)                 | 把node转成dict类型                                   |
| walk(node)                 | Yield node as the only item in a ***walk()***        |
| node.labels                | Return the full set of labels associated with *node* |
| node.has_label(label)      | 略                                                   |
| node.add_label(label)      |                                                      |
| node.remove_label(label)   |                                                      |
| node.clear_labels()        |                                                      |
| node.update_labels(labels) |                                                      |

#### Relationship类



```python
class py2neo.data.Relationship(start_node, type, end_node, **properties)
class py2neo.data.Relationship(start_node, end_node, **properties)
class py2neo.data.Relationship(node, type, **properties)
class py2neo.data.Relationship(node, **properties)

a = Node("Person", name="Alice") # Person是label, name是属性
c = Node("Person", name="Carol")
class WorksWith(Relationship): pass
ac = WorksWith(a, c)
type(ac)  # 'WORKS_WITH'
```



| 操作                      | 含义                                                         |
| ------------------------- | ------------------------------------------------------------ |
| ==                        | 判断关系是否相等                                             |
| ！=                       |                                                              |
| hash(relationship)        | Return a hash of *relationship* based on its start node, end node and type. |
| relationship[key]         |                                                              |
| relationship[key] = value | Remove the property with key *key* from *relationship*, raising a `KeyError` if such a property does not exist. |
| del relationship[key]     |                                                              |
| len(relationship)         | Return the number of properties in *relationship*.           |
| dict(relationship)        |                                                              |
| walk(relationship)        | Perform a [`walk()`](https://py2neo.org/v4/data.html#py2neo.data.walk) of this relationship, yielding its start node, the relationship itself and its end node in turn. |
| type(relationship)        |                                                              |



#### Subgraph类

Subgraph是一个静态集合（关于nodes和relationships的），所以subgraph可以支持一系列集合的

操作，



#### Path类 和 Walkable类

A *Walkable* is a `Subgraph` with ***added traversal information***. 

A *Path* is a type of `Walkable` returned by some Cypher queries.



#### Record类

A `Record` object holds an ordered, keyed collection of values.



#### Table类

A `Table` holds a list of `Record` objects, typically received as the result of a Cypher query. I