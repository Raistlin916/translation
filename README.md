## The introduction to Reactive Programming you've been missing

## 关于你错过的响应式编程的介绍

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
