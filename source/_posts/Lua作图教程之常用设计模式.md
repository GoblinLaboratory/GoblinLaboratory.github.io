---
layout: post
title: Lua作图教程之设计模式
date: 2018/5/4 13:48:25
category: "Architecture"
tags: 
- lua
thumbnail: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1529518542867&di=40a6116f36ed75e54dced82bc36d1fd3&imgtype=0&src=http%3A%2F%2Fatt.bbs.duowan.com%2Fforum%2F201510%2F29%2F193005hf1h70zz47wl6pey.jpg
lede: "设计模式（Design pattern）代表了最佳的实践，通常被有经验的面向对象的软件开发人员所采用。设计模式是软件开发人员在软件开发过程中面临的一般问题的解决方案。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。"
featured: true
---

## 设计模式

设计模式（Design pattern）代表了最佳的实践，通常被有经验的面向对象的软件开发人员所采用。设计模式是软件开发人员在软件开发过程中面临的一般问题的解决方案。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。

<!--more-->

到目前为止，我在用lua作图时只用到了单例模式，其他设计在后续开发中在逐步添加。

```lua
    --[[  
    优点  
    一、实例控制  
    单例模式会阻止其他对象实例化其自己的单例对象的副本，从而确保所有对象都访问唯一实例。  
    二、灵活性  
    因为类控制了实例化过程，所以类可以灵活更改实例化过程。  
  
    缺点  
    一、开销  
    虽然数量很少，但如果每次对象请求引用时都要检查是否存在类的实例，将仍然需要一些开销。可以通过使用静态初始化解决此问题。  
    二、可能的开发混淆  
    使用单例对象（尤其在类库中定义的对象）时，开发人员必须记住自己不能使用new关键字实例化对象。因为可能无法访问库源代码，因此应用程序开发人员可能会意外发现自己无法直接实例化此类。  
    三、对象生存期  
    不能解决删除单个对象的问题。在提供内存管理的语言中（例如基于.NET Framework的语言），只有单例类能够导致实例被取消分配，因为它包含对该实例的私有引用。在某些语言中（如 C++），其他类可以删除对象实例，但这样会导致单例类中出现悬浮引用。  
  
]]--  
  
Singleton = {}  
function Singleton:new(o)  
    o = o or {}  
    setmetatable(o,self)  
    self.__index = self  
    return o  
end  
  
function Singleton:Instance()  
    if self.instance == nil then  
        self.instance = self:new()  
    end  
    return self.instance  
end  
  
  
s1 = Singleton:Instance()  
  
s2 = Singleton:Instance()  
  
if s1 == s2 then  
    print("两个对象是相同的实例")  
end  
```