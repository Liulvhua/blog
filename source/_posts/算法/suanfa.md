---
title: 数据结构与算法学习(js版)
date: 2020-11-1
updated:
tags: 数据结构
categories: 算法
description: 
top_img: https://image.yangxiansheng.top/img/undraw_teaching_f1cm.png?imglist
cover: https://image.yangxiansheng.top/img/undraw_teaching_f1cm.png?imglist
copyright_author: 努力中的杨先生
copyright_author_href: https://github.com/251205668
---

## 时间复杂度和计算方法

![](https://image.yangxiansheng.top/img/20200810203004.png?imagelist)

计算时间复杂度就是计算代码执行多少次，循环多少次。

例:
```js
o(1)
n+1=2
console.log(n)

o(n)
for(let i = 0;i < n;i++){
    console.log(i)
}

o(n^2)
for(let i =0;i<n;i++){
    for(let j = 0;j < n;j++){
        console.log()
    }
}

o(logN)
let i =1
while(i<n){
    i* = 2
    console.log(n)
}

```

## 空间复杂度

空间复杂度计算就是计算声明的变量，或者内存变量是多少个

例:
```js
O(1)
let i =1
console.log(1)

O(n)
let a = []
for(let i=0;i<n;i++){
    a.push[i]
}

O(n^2)

let a = []
for(let i = 0;i<n;i++){
    a.push([])
    for(let j = 0;j<n;j++){
        a[i][j].push(j)
    }
}
```

## 栈

后进先出的一种内存结构.比如js中的数组

```js
let stack = []
let item1 = stack.push(1)
let item2 = stack.pop()
```

栈这种数据结构出现得场景

- 十进制转二进制 在利用短除法时计算二进制,最后取的是余数从后面排到前面,采用的是栈的思想,后进先出。
- 有效的花括号，例如判断一堆括号是否是有效的闭合的就可以采用栈的形式

解决这个问题的思路就是使用栈,首先遍历字符串，然后如果元素碰到`{([`这种字符就往栈推送元素，否则**判断元素和栈顶元素是否同时满足`{}` `()`,`[]`这种组合**。满足则将元素推出栈。最后判断是否栈为空，为空则返回`true`

```js
function isvalid(s){
    let stack = []
    if(stack.length %2 === 1){
        return false
    }
    for(let i = 0;i < s.length;i++){
        let c = s[i]
        if(c === '{' || c === '(' || c==='['){
            stack.push(c)
        }else{
            let p = stack[stactk.length-1]
            if(p === '(' && c ===')' || p === '{' && c === '}' && p === '[' && p === ']' ){
                stack.pop()
            }else{
                return false
            }
        }
    }
    return statck.length === 0
}
```
- 函数调用,js解析器就使用了栈

```js
// 首先入栈func1 func2 func3 出栈先调用func3 func2 func1
const func1 = ()=>{
    fun2()
}
const func2 = ()=>{
    func3()
}

func1()
```

## 队列

先进先出

```js
const array = []
array.push()
array.shift()
```
使用场景:
- 食堂打饭
- js异步任务队列


- 计算请求次数

计算三千毫秒以内的请求次数

输入：inputs = ["RecentCounter","ping","ping","ping","ping"], inputs = [[],[1],[100],[3001],[3002]]
输出：[null,1,2,3,3]


时间复杂度o(n),空间复杂度O(队列长度)
```js
function counter(){
  // 保证使用同一个队列
  this.q = []
}

Function.prototype.ping = function(t){
  // 请求时间入队
  this.q.push(t)
  // 大于三千毫秒出队
  while(this.q[0] < t-3000){
    this.q.shift()
  }
  return this.q.length
}


var counter = new counter()
counter.ping(t)
```

## 链表

链表元素可以用对象进行模拟,相比与数组最大的特点就是不用移动元素位置

```js
const a = {val:'a'}
const b = {val:'b'}
const c = {val:'c'}

a.next = b
b.next = c

// 遍历链表
let  p =a
while(p){
  console.log(p)
  p = p.next
}

// 增加
const e = {val:'e'}
a.next = e
e.next = b

// 删除
a.next = b

```
**删除某个节点**

![](https://image.yangxiansheng.top/img/20200905113555.png?imagelist)

思路就是将节点换成下个节点的值，然后删除掉下个节点

```js
var deleteNode = function(node) {
    node.val = node.next.val
    node.next = node.next.next
};
```

**反转链表**

首先定义双指针遍历，p1,p2分别指向头部和null，然后在遍历能进行的条件下将p1指向p2

![](https://image.yangxiansheng.top/img/20200905115458.png?imagelist)
```js
var reverseList = function(head) {
    let p1 = head
    let p2 = null
    while(p1){
        // 保存状态
        const temp = p1.next
        // 反转
        p1.next = p2
        // 双指针遍历 一前一后
        p2 = p1
        p1 = temp
    }
    // p1最后指向null
    return p2
};
```

**两数之和**

![](https://image.yangxiansheng.top/img/20200905172314.png?imagelist)

思路: 首先设置两个指针，新建一个链表

如果两个链表元素的值相加大于10，就把个位数追加进新链表，十位数放到下一次再增加
每次添加的值都是: 两个链表的值再加上十位数的值

最后遍历链表。如果链表循环结束之后还有进1情况，就把carry追加到链表上面，最后返回新链表
```js
var addTwoNumbers = function(l1, l2) {
   // 初始化链表
   const l3 = new ListNode(0)
   let p1 = l1 
   let p2 = l2
   let p3 = l3
   // 下一轮要加的数
   let carry = 0
   while(p1 || p2){
       // 取值
      const v1 = p1? p1.val:0
      const v2 = p2? p2.val:0
      const v3 = v1 + v2 + carry
      // 获取下一轮叠加数字 也就是获取十位数
      carry = Math.floor(v3 / 10)
      // 获取本轮个位上数 然后追加到链表上
      p3.next = new ListNode(v3 % 10)
      // 遍历链表
      if(p1) p1 =p1.next
      if(p2) p2 = p2.next
      p3 = p3.next
   }
   // 需要考率到while循环之后进1情况
   if(carry){
    p3.next = new ListNode(carry)
   }
   return l3.next
};

```

**删除链表重复元素**

节点值和下个节点值相同，删除

```js
function deleteAlreadyExisted(head){
    let p = head
    while(p && p.next){
      if(p.val === p.next.val){
        p.next = p.next.next
      }
   }
   return head
}
```

**判断是否是环形链表**

定义两个一块一慢的指针遍历，相逢则成环

```js
function isCircleListNode(head){
  let p1 = head
  let p2 = head
  while(p1 && p2 && p2.next){
    p1 = p1.next
    p2 = p2.next.next
    if(p1 === p2){
      return true
    }
  }
  return false

}
```


## 集合

无需且唯一的数据结构，可以去重

```js
// 去重
const arr1 = [1,2,3,4,4,4]
const arr2 = [...new Set(arr1)]

// 判断一个值是否存在集合中
const set1 = new Set([1,2])
set1.has(1) 

// 求交集
const set2 = new Set([1,2,3,4,5])
const set3 = new Set([...set1].filter(item=>set2.has(item) === item))
```

求两个数组的交集

```js
[...new Set(nums1)].filter(item=>nums2.includes(item))
```

## 字典map

**求一个字符的不重复最长子串长度**

![](https://image.yangxiansheng.top/img/20200906115247.png?imagelist)

```js
var lengthOfLongestSubstring = function(s) {
    let l = 0
    let res = 0
    // 记录子串位置和值
    const map = new Map()
    for(let r = 0;r < s.length;r++){
        // 遇到重复的就把左指针移动到重复节点的下一个 并且需要保证map中保存的数据是在滑动窗口中
        if(map.has(s[r]) && map.get(s[r]) >= l){
            l = map.get(s[r]) +1
        }
       res = Math.max(res,r-l+1)
       map.set(s[r],r)
    }
    return res
};
```
时间: O(n)
空间: O(m) 不重复子串长度

## 树

**深度广度优先遍历**

![](https://image.yangxiansheng.top/img/20200906153925.png?imagelist)

深度优先遍历: 将他看成正常看书顺序，先封面，再从第一章开始看依次下去

口诀: 先访问根节点，再挨个深度有限遍历根节点的children

```js
const tree = {
    val:'a',
    children:[
        {
           val:'b',
           children:[
             {
              val:'d',
              children:[]
            },
            {
              val:'e',
              children:[]
            }
            ]
         },
        {
           val:'c',
           children:[
             {
              val:'f',
              children:[]
            },
            {
              val:'g',
              children:[]
            }
            ]
         },
    ]
}
```

```js
let dfs = (root)=>{
  // 访问根节点
  console.log(root.val)
  // 递归挨个遍历子节点 
  root.children.forEach(dfs)
}
dfs(tree)
```

广度优先遍历: 将他看成先看封面再看`目录`，再去看文章

![](https://image.yangxiansheng.top/img/20200906154657.png?imagelist)

```js
let bfs = (root)=>{
  // 根节点入队
   let q = [root]
   while(q.length > 0){
     // 队头出队 并访问
     const n = q.shift()
     console.log(n.val)
     // 队头children挨个入队
     n.children.forEach(child=>{
       q.push(child)
     })

   }
}
```

**先中后序遍历**
