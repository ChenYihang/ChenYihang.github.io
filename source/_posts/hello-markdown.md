---
title: hello markdown
date: 2019-03-25 18:35:40
tags: Markdown
categories: Markdown
update: 2019-04-13
---

#### What is Markdown？

[**Markdown**](<https://daringfireball.net/projects/markdown/syntax>)是一种纯文本格式的标记语言，其语法目标是成为一种适用于网络的书写语言。Markdown兼容HTML，但它不是一种发布的格式，而是一种书写的格式，Markdown 的格式语法只涵盖纯文本可以涵盖的范围。

#### **Markdown语法**

- 标题，使用1-6个`#`字符作为行起始，对应6个等级，如

  ```
  # H1
  ## H2
  ### H3
  ```



- 引用，Markdown使用email-style的`>`字符标明引用块如

  > 这是一个引用块
  >
  > > 层级引用



- 列表，使用`*`字符标明列表或者输入1. 2.如

  - list1
  - list2
  - list3
    1. list1
    2. list2

- 任务列表，使用`[]`或`[x]`  (完成或未完成)，如

  - [ ] task1
  - [x] task1   

  

- 代码块，使用\`\`\`并回车

  ```c++
  template <typename objType>
  const objType& GetMax(const objType& value1, const objType& value2)
  {
  if (value1 > value2)
  return value1;
  else
  return value2;
  }
  ```

  

- 链接

  1. 内部链接，格式

     ```
     [name](your link)//[google](https://www/google.com)
     ```

  2. 参考形式链接，格式

     ```
     [name][id]    //[google][1]
     
     [id]: your link   // [1]:https://www/google.com
     ```

  

- 斜体 使用`*`或者`_` ,加粗则双写上述字符

  *example* **example** 

  

- 删除线使用``~~`` 

  ~~example~~

- 下划线，由raw html支持 ，``<u>Underline</u>``为

  <u>Underline</u> 

- Emoji :smile:

 ```
  :smile:  //在Typora编辑器上能显示，blog页面好像不支持
 ```

