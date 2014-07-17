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

**最总要的是，你得到了一个令人惊异的工具箱，可以组合函数，创建并过滤任意流。**那就是“函数式”的magic kicks in。一个流能够被用作另一个流的输入。甚至多个流也能作为另一个流的输入。你能够_过滤_一个流去得到另外一个只拥有你感兴趣的事件的流。你可以映射数据值从一个流到另外一个新流。


