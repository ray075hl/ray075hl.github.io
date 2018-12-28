---
layout: post
tags: [programming]
title: LUA programming
date: 2018-12-28 12:00:00
img: modern_cplusplus.jpg
---

#### 1 LUA programming

最近需要用Lua来解决一些实际问题, 简单的学习一下.

lua中的**关键词**:

| *and*       | *break*   | *do*           | *else*       | *elseif*        | *end*       |
| ----------- | :-------- | -------------- | ------------ | --------------- | ----------- |
| ***false*** | ***for*** | ***function*** | ***if***     | ***in local***  | ***nil***   |
| ***not***   | ***or***  | ***repeat***   | ***return*** | ***then true*** | ***until*** |
| ***while*** |           |                |              |                 |             |

注释:

```lua
--[[This is a lua comment--]]
--This is a lua comment.
print("Hello World!")
fruit = apple + oranges --get the total fruit 
```



##### 1.1 变量(Variables)

* Global variables: 
* Local variables: 
* Table fields: 

```lua
local d, f = 5, 10    --declaration of d and f as local variables.
d, f = 5, 10;		  --declaration of d and f as global variables.
d, f = 10			  --[[decalaration of d and f as global variables.
						  Here value of f is nil 初始化没给值就是默认为"nil"--]] 
```

左值和右值

Lvalue: L指的是Location, 表示可寻址.

Rvalue: R指的是Read, 表示可读.

**Lua是一种动态类型语言, 变量(variables)没有类型, 但是"值(value)"有类型, 值存于变量中.**

| Type     | Description                                                  |
| -------- | ------------------------------------------------------------ |
| nil      | Used to differentiate the value from having some data or no(nil) data.相当于python里的None |
| boolean  | Include true and false as values.                            |
| number   | Represents real(double precision floating point) numbers.    |
| string   | Represents array of characters.                              |
| function | Represents a method that is written in C or Lua.             |
| userdata | Represents arbitrary C data                                  |
| thread   | Represents independent threads of execution and it is used to implement coroutines. |
| table    | Represent ordinary arrays, symbol tables, sets, records, graphs, trees, etc., and implements associative arrays. It can hold any value (except nil). |

```lua
print(type("What is my type"))
print(type(5.8))		--> number
print(type(print))
print(type(true))
print(type(nil))
print(type(type(nil)))  --> string
```

| 关系运算符 | ==     | ~=     | >    | <    | >=   | <=   |
| ---------- | ------ | ------ | ---- | ---- | ---- | ---- |
|            | 含义略 | 不等于 | 略   | 略   | 略   | 略   |

| 逻辑运算符 | and  | or   | not  |
| ---------- | ---- | ---- | ---- |
|            | 略   | 略   | 略   |

| 其他操作符 | ..                        | #                                                            |
| ---------- | ------------------------- | ------------------------------------------------------------ |
|            | concatenates two strings. | An unary operator that return the length of the a string or a table. 返回一个字符串或table的长度. |

| 类别                 | 操作符          | 关联顺序 |
| -------------------- | --------------- | -------- |
| 一元(Unary)          | not # -         | 右至左   |
| concatenation        | ..              | 右至左   |
| 乘除(Multiplicative) | * / %           | 左至右   |
| 加减(Additive)       | +-              | 左至右   |
| 关系(Relational)     | < > <= >= == ~= | 左至右   |
| 逻辑与               | and             | 左至右   |
| 逻辑或               | or              | 左至右   |

##### 1.2 循环 和 条件

while, for, repeat...until.   

break.

if else.

```lua
while( true )
do 
    print("This loop will run forever.")
end
--[[只有false 和 nil 会被认为是不满足条件, 即便是0也会被认为是满足条件的true--]]
```

##### 1.3 函数

```lua
--[[Defining a Function--]]
optinal_function_scope funtion function_name(argument1, argument2, ... argumentn)
function_body
return result_params_comma_separated
end
```

例子:

```lua
--[[ function returning the max between two numbers --]]
function max(num1, num2)
    
    if (num1 > num2) then
        result = num1;
    else
        result = num2;
    return result;
end
-- calling a function
print("The maximum of the two numbers is ", max(10, 4))
print("The maximum of the two numbers is ", max(5, 6))
```

把函数当变量传递

```lua
myprint = function(param)
    print("This is my print function -  ##", param, "##")
end

function add(num1, num2, functionPrint)
    result = num1 + num2
    functionPrint(result)
end

myprint(10)
add(2, 5, myprint)
```



接受变长数目输入的函数

```lua
function average(...)
    result = 0
    local arg = {...}
    for i, v in ipairs(arg) do  -- ipairs 相当与 enumerate
        result = result + v
    end
    return result/#arg
end

print("The average is", average(10, 5, 3, 4, 5, 6))
```







