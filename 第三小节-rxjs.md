在 RxJS 中有四种 Subject 分别是：Subject，BehaviorSubject，AsyncSubject，ReplaySubject；这四种 Subject 都是特殊的 Observable。

在介绍它们之前，我们先来看下这四种 Subject 与普通 Observable 的区别：

[RxJS：四种 Subject 的用法和区别](https://limeii.github.io/2019/07/rxjs-subject/)

# Cold vs Hot
在文章【RxJS：Cold vs Hot Observables】里详细介绍了 Cold Observables 与 Hot Observables 的区别，除了 Observable 之外，这四种 Subject 都是 Hot Observables。

# 数据生产者 vs 数据消费者

- 数据生产者是指 Observable(可观察对象)，产生数据的一方。

- 数据消费者是指 Observers(观察者)，接收数据的一方。

普通的 Observable 只是数据生产者，发送数据。而 Subject，BehaviorSubject，AsyncSubject 和 ReplaySubject 即是生产者又是消费者。比如在 Angular项目中，我们经常用 Subject 在多个 component 中共享数据：

# 单播 vs 多播
在上面的代码例子中，不仅是在 component c 可以订阅拿到数据，在其他 component 中也可以订阅拿到数据。比如在 RxjsSubjectAComponent 中定义如下代码：

从 RxjsSubjectBComponent 发出值，在 RxjsSubjectCComponent 和 RxjsSubjectAComponent 能拿到同样的值，这个就是多播。

普通的 Observables 是 Cold Observables 并且是单播的，其他四种 Subject 是 Hot Observables 并且是多播的。

单播的意思是，每个普通的 Observables 实例都只能被一个观察者订阅，当它被其他观察者订阅的时候会产生一个新的实例。也就是普通 Observables 被不同的观察者订阅的时候，会有多个实例，不管观察者是从何时开始订阅，每个实例都是从头开始把值发给对应的观察者。

# BehaviorSubject
Subject 其中的一个变体就是 BehaviorSubject，它有一个“当前值”的概念。它保存了发送给消费者的最新值，当有新的观察者订阅时，会立即从 BehaviorSubject 那接收到“当前值”，在定义一个 BehaviorSubject 时需要有初始值。

在 messageService 定义一个 BehaviorSubject，初始值是1，updateBehaviorSubject 方法把值更新为2，代码如下：

