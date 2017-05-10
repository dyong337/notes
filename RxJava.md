##观察者模式

##Observer
	决定事件触发时，将有怎么样的行为。
	
	实现Observer接口
	Subscriber抽象类，Observer
		onStart()：
			新增方法，在subscribe刚开始的时候，事件还未发送之前被调用，可做一些准备工作。工作线程-->subscribe坐在线程
		unSubscribe():取消订阅，可以用isUnsubscribe()进行判断。
		
##Observable
	Observable 即被观察者，它决定什么时候触发事件以及触发怎样的事件。

##Subscribe(订阅)
##Scheduler(线程控制)
	Schedulers.immediate()
	Schedulers.newThread()
	Schedulers.io()
	Schedulers.computation()
	AndroidSchedulers.mainThread()

	subscribeOn() 和 observeOn() 两个方法来对线程进行控制了
		 * subscribeOn(): 指定 subscribe() 所发生的线程，即 Observable.OnSubscribe 被激活时所处的线程。或者叫做事件产生的线程。
		 * observeOn(): 指定 Subscriber 所运行在的线程。或者叫做事件消费的线程。

Scheduler 的原理 (一)
	变换:所谓变换，就是将事件序列中的对象或整个序列进行加工处理，转换成不同的事件或事件序列.
		map(): 事件对象的直接变换，是一对一的转化
		flatMap():事件对象的直接变换，一对多的转化
		共同点：它也是把传入的参数转化之后返回另一个对象。
		不同点：flatMap()返回的是个Observable对象，并且这个对象并不是直接发送到Subscriber的回到方法中。
		
flatMap()原理：
	使用传入的对象创建一个Observable对象，并将这个对戏那个激活。这个Observable对象开始发送事件。每一个创建出来的Observable发送的事件都汇入同一个Observable，而这个Observable负责将这些事件同意交给Subscriber的回调方法。
		这个步骤，把事件拆成两级，通过一组新建的Observable将初始化的对象铺平之后通过同意路径分发下去。

变换的原理：lift()
		实质上都是针对事件序列的处理和再发送。他们基于同一个基础的变化方法---->lift(Operator).

compose:对Observable整体的变换
	与lift的区别：lift() 是针对事件项和事件序列的
	 compose():是针对 Observable 自身进行变换
		
	observable.compose(liftAll).subscribe(subscriber);
		说明:liftAll为Onservable.Transformer的实现类，里面进行了一些列lift()方法的转换

线程控制：
	subscribeOn():事件生成所在的线程。
	observeOn():事件消费所在的线程。

##闭包概念