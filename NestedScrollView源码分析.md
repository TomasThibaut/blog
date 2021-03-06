support-v4中最新添加了一个类NestedScrollView，根据官方文档描述
>it supports acting as both a nested scrolling parent and child on both new and old versions of Android
NestedScrollView既可以作为嵌套滚动的父容器，也可以作为嵌套滚动的子view。

```java
	public class NestedScrollView extends FrameLayout implements NestedScrollingParent,
        NestedScrollingChild
```
看源码可以知道NestScrollView继承自FrameLayout，同时还实现了两个接口NestedScrollingParent和NestedScrollingChild。实现的这两个接口正是NestedScrollView既可以作为嵌套滚动的父容器也可以作为子view的关键。下面我就来分析一下这两个接口。

## 介绍
我们先来简单看看NestedScrollingParent和NestedScrollingChild

### NestedScrollingParent
NestedScrollingParent接口必须要被ViewGroup的子类来实现，实现这个接口的目的是用来接收子view委托过来的滚动事件。也就是说我们在实现嵌套滚动的时候，子view会将滚动事件通过该接口通知给父容器，父容器在实现方法中决定如何滚动，子view再根据父容器的滚动结果，决定自己如何滚动。

实现NestedScrollingParent接口的类中必须创建一个NestedScrollingParentHelper实例，并将view或者viewgroup中所有和NestedScrollingParentHelper同签名的方法都委托给这个实例的方法。也就是说真正的滚动都是在NestedScrollingParentHelper中实现的，NestedScrollingParentHelper对view的滚动又都是委托给ViewCompat, ViewGroupCompat 或者 ViewParentCompat的，这样才能实现兼容到5.0之前的版本。

###NestedScrollingChild
NestedScrollingChild接口必须要被View的子类来实现，实现这个接口的目的是用来将自己的滚动事件派发给需要配合的父层ViewGroup。也就是说我们在实现嵌套滚动的时候，子view会通过该接口定义的方法将滚动事件通过该接口通知给父容器，父容器在实现方法中决定如何滚动，子view再根据父容器的滚动结果，决定自己如何滚动。

实现NestedScrollingChild接口的类中必须创建一个NestedScrollingChildHelper实例，并将view中所有和NestedScrollingChildHelper同签名的方法都委托给这个实例的方法。也就是说真正的滚动都是在NestedScrollingChildHelper中实现的，NestedScrollingChildHelper对view的滚动又都是委托给ViewCompat, ViewGroupCompat 或者 ViewParentCompat的，这样才能实现兼容到5.0之前的版本。