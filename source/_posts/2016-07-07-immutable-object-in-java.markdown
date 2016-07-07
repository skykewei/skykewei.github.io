---
layout: post
title: "Java并发中的不可变对象"
date: 2016-07-07 13:27:32 +0800
comments: true
categories: 并发
---


在多线程编程中，不可变对象非常有用，因为不可变对象在多线程环境共享下没有状态不一致问题，线程访问不可变对象没有加锁解锁的开销。 不可变对象的状态自从创建后就不会被改变，因为不可变对象天生具备线程安全的特性。

在JDK中，String和Integer Long等基本类型的包装类是典型的不可变对象。

## 编写不可变对象准则 ##

- 不可变对象的状态自创建后不可更改，或者使用另一个不可变对象整体替换
- 不可变对象的所有成员变量应该是`final`
- 不可变对象不可继承，`final Class` 实现
- 不可变对象的只设置getter方法，添加setter方法。getter返回成员变量对象引用时，返回该对象一个副本

<!--more-->

当然对于上边第二条也有例外，例如`String`中的成员变量
    
    /** Cache the hash code for the string */
    private int hash; // Default to 0

hash就不是`final` 关键字。但是String的`public`方法能够访问hash的只有`hashCode()`方法。因为`value`是不变的，每次计算hash也是一样的。


		/** The value is used for character storage. */
        private final char value[];

        public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
        }

## 简单不可变对象例子 ##

		public final class Person{

            private final String name;
            private final String sex;
			private final Date birthday;
            public Person(String name,String sex){
                this.name = name;
                this.sex = sex;
            }

            public String getName(){
               return name;
            }
            
            public String getSex(){
               return sex;
            }

            public Date getBirthDay(){
               return (Date)birthday.clone();
            }
        }