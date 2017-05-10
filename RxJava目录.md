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

##闭包概念