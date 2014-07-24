## The introduction to Reactive Programming you've been missing

## 关于你错过了的响应式编程的简介

(by [@andrestaltz](https://twitter.com/andrestaltz))

(作者[@andrestaltz](https://twitter.com/andrestaltz))


====

So you're curious in learning this new thing called (Functional) Reactive Programming (FRP).

所以你对学习新兴的响应式（函数式）编程很感兴趣。

Learning it is hard, even harder by the lack of good material. When I started, I tried looking for tutorials. I found only a handful of practical guides, but they just scratched the surface and never tackled the challenge of building the whole architecture around it. Library documentations often don't help when you're trying to understand some function. I mean, honestly, look at this:

因为缺乏好的学习材料，所以学习它很困难。当开始学习时，我试图寻找教程。我发现只有一些实践指导，浮于表面并且根本没有整体架构上的挑战。文档经常对你理解一些函数没有帮助，我得诚实的指出，像下面这个：


> **Rx.Observable.prototype.flatMapLatest(selector, [thisArg])**

> Projects each element of an observable sequence into a new sequence of observable sequences by incorporating the element's index and then transforms an observable sequence of observable sequences into an observable sequence producing values only from the most recent observable sequence.

Holy cow.

见鬼。

I've read two books, one just painted the big picture, while the other dived into how to use the FRP library. I ended up learning Reactive Programming the hard way: figuring it out while building with it. At my work in [Futurice](https://www.futurice.com) I got to use it in a real project, and had the [support of some colleagues](http://blog.futurice.com/top-7-tips-for-rxjava-on-android) when I ran into troubles.

我已经读过两本书，一本只是描绘了大致的画面，而另一本则一头扎进了如何使用FRP库。我最终通过困难的方式学习响应式编程：弄清楚如何构建它。在我的关于[Futurice](https://www.futurice.com)的工作中我曾经把它应用于真实的项目，并且在遇到麻烦时得到了[同事的帮助](http://blog.futurice.com/top-7-tips-for-rxjava-on-android)。

The hardest part of the learning journey is thinking in FRP. It's a lot about letting go of old imperative and stateful habits of typical programming, and forcing your brain to work in a different paradigm. I haven't found any guide on the internet in this aspect, and I think the world deserves a practical tutorial on how to think in FRP, so that you can get started. Library documentation can light your way after that. I hope this helps you.

学习之旅中遇到最困难的部分是用FRP的方式思考。这需要摒弃古老而经典的命令状态式编程，并且强迫你的大脑按不同的范式工作。我没有在互联网上发现这个方面的任何指南，同时我认为世上需要一个实用教程关于如何FRP的思考方式，这样你才能得以开始。文档库可以照亮你之后的道路。我希望这些能帮到你。

## "What is Functional Reactive Programming (FRP)?"

## “什么是函数响应式编程(FRP)?”

There are plenty of bad explanations and definitions out there on the internet.

互联网上有大量糟糕的解释和定义。

[Wikipedia](https://en.wikipedia.org/wiki/Functional_reactive_programming) is too generic and theoretical as usual. [Stackoverflow](http://stackoverflow.com/questions/1028250/what-is-functional-reactive-programming)'s canonical answer is obviously not suitable for newcomers. [Reactive Manifesto](http://www.reactivemanifesto.org/) sounds like the kind of thing you show to your project manager or the businessmen at your company. Microsoft's [Rx terminology](https://rx.codeplex.com/) "Rx = Observables + LINQ + Schedulers" is so heavy and Microsoftish that most of us are left confused. Terms like "reactive" and "propagation of change" don't convey anything specifically different to what your typical MV* and favorite language already does. Of course my framework views react to the models. Of course change is propagated. If it wouldn't, nothing would be rendered.

[维基](https://en.wikipedia.org/wiki/Functional_reactive_programming) 像通常一样泛泛和理论化。[Stackoverflow](http://stackoverflow.com/questions/1028250/what-is-functional-reactive-programming) 里的标准答案明显不适合新人。[Reactive Manifesto](http://www.reactivemanifesto.org/) 听起来像在公司展示给你的项目经理或者商务人员看。微软的[Rx terminology](https://rx.codeplex.com/) "Rx = Observables + LINQ + Schedulers" 实在是太重型和微软化让我们大多数人感到困惑。像"响应式"和"propagation of change"这样的术语没有传达任何跟你使用典型的MV*和最爱的语言已经做到的之间特殊的差别。当然了，我的框架视图响应模型。当然了，变化在传播。如果没有，没有东西会被渲染。

So let's cut the bullshit.

让我们少说废话。

#### FRP is programming with asynchronous data streams.

#### FRP 是异步数据流的编程

In a way, this isn't anything new. Event buses or your typical click events are really an asynchronous event stream, on which you can observe and do some side effects. FRP is that idea on steroids. You are able to create data streams of anything, not just from click and hover events. Streams are cheap and ubiquitous, anything can be a stream: variables, user inputs, properties, caches, data structures, etc. For example, imagine your Twitter feed would be a data stream in the same fashion that click events are. You can listen to that stream and react accordingly.

在某种程度上，这不是什么新东西。事件总线或者典型的点击事件真的是一个可以观察并做一些副作用的异步事件流。 FRP is that idea on steroids。你能够去创造任何东西的数据流，不仅来自点击和悬浮事件。流是廉价和无处不在的，任何东西可以作为一个流：变量，用户输入，属性，缓存，数据结构等等。举例而言，想象你的Twitter关注列表是一个像点击事件一样的数据流。你可以监听那个流并做相应的响应。

**On top of that, you are given an amazing toolbox of functions to combine, create and filter any of those streams.** That's where the "functional" magic kicks in. A stream can be used as an input to another one. Even multiple streams can be used as inputs to another stream. You can _merge_ two streams. You can _filter_ a stream to get another one that has only those events you are interested in. You can _map_ data values from one stream to another new one.

**最总要的是，你得到了一个令人惊异的工具箱，可以组合函数，创建并过滤任意流。**那就是“函数式”的magic kicks in。一个流能够被用作另一个流的输入。甚至多个流也能作为另一个流的输入。你可以_合并_两个流，可以_过滤_一个流从而得到另外一个只拥有你感兴趣的事件的流，可以把数据值从一个流_映射_到另外一个新的流。

If streams are so central to FRP, let's take a careful look at them, starting with our familiar "clicks on a button" event stream.

如果流对FRP而言是如此核心，那么让我们仔细的研究一下，通过我们熟悉的“点击按钮”事件流。

![Click event stream](https://gist.githubusercontent.com/staltz/868e7e9bc2a7b8c1f754/raw/49da694b2489f9e7b7276df31a1dcb206179a496/zclickstream.png)

A stream is a sequence of **ongoing events ordered in time**. It can emit three different things: a value (of some type), an error, or a "completed" signal. Consider that the "completed" takes place, for instance, when the current window or view containing that button is closed.

一个流是一个按时间排序的正在进行的事件序列。它能够发出三种不同的东西：值（按某种格式），错误和“完成”事件。考虑一下发生“完成”，例如，在当前窗口或包含该按钮的视图被关闭时。

We capture these emitted events only **asynchronously**, by defining a function that will execute when a value is emitted, another function when an error is emitted, and another function when 'completed' is emitted. Sometimes these last two can be omitted and you can just focus on defining the function for values. The "listening" to the stream is called **subscribing**. The functions we are defining are observers. The stream is the subject (or "observable") being observed. This is precisely the [Observer Design Pattern](https://en.wikipedia.org/wiki/Observer_pattern).

我们只能异步捕获那些被发出的事件，通过定义一个当发出值时会执行的函数，一个当发出错误时执行的函数，和一个发出“完成”时执行的函数。有时最后两个可以被忽略，你可以只关注定义给值的函数。“监听”流被称作“订阅”。定义的那些函数是观察者。流是被观察的主体。这在[观察者设计模式](https://en.wikipedia.org/wiki/Observer_pattern)中有所讨论。

An alternative way of drawing that diagram is with ASCII, which we will use in some parts of this tutorial:
```
--a---b-c---d---X---|->

a, b, c, d are emitted values
X is an error
| is the 'completed' signal
---> is the timeline
```

绘制该图表的一个可行方案是使用ASCII码，我们会在本教程的某些部分使用：
```
--a---b-c---d---X---|->

a, b, c, d are emitted values
X is an error
| is the 'completed' signal
---> is the timeline
```

Since this feels so familiar already, and I don't want you to get bored, let's do something new: we are going to create new click event streams transformed out of the original click event stream.

由于这感觉太熟悉了，我不想让你感到厌倦，所以我们来做些新东西：我们要创造从原有的事件流中转变出来的新的点击事件流。

First, let's make a counter stream that indicates how many times a button was clicked. In common FRP libraries, each stream has many functions attached to it, such as `map`, `filter`, `scan`, etc. When you call one of these functions, such as `clickStream.map(f)`, it returns a **new stream** based on the click stream. It does not modify the original click stream in any way. This is a property called **immutability**, and it goes together with FRP streams just like pancakes are good with syrup. That allows us to chain functions like `clickStream.map(f).scan(g)`:

首先，我们来做一个表明按钮有多少次点击的计数流。在常见的FRP库中，每个流都有很多方法来连接到它，像`map`,`filter`,`scan`等等。当你调用这其中的一个方法，类似`clickStream.map(f)`，它会返回一个**新流**基于点击流。它不会以任何方式修改原有点击流。这个属性叫做不变性，它跟FRP流的关系就像煎饼配糖浆一样合适。这允许我们链式调用函数，像`clickStream.map(f).scan(g)`:

```
  clickStream: ---c----c--c----c------c-->
               vvvvv map(c becomes 1) vvvv
               ---1----1--1----1------1-->
               vvvvvvvvv scan(+) vvvvvvvvv
counterStream: ---1----2--3----4------5-->
```
