##dagger2
#基础
	
概念：
	依赖注入(Dependency Injection简称DI)：
	注解(Annotation):
	依赖注入：就是目标类中所依赖的其他的类的初始化过程，不是通过手动编码的方式创建，而是通过技术手段可以把其他的类的已经初始化好的实例自动注入到目标类中。
	目标类：需要进行依赖初始化的类

Inject：
	注解(Annotation)来标注目标类中所依赖的其他类，同样用注解来标注所依赖的其他类的构造函数，那注解的名字就叫Inject
Component：
	Component也是一个注解类，一个类要想是Component，必须用Component注解来标注该类，并且该类是接口或抽象类
	原理：
		Component需要引用到目标类的实例，Component会查找目标类中用Inject注解标注的属性，查找到相应的属性后会接着查找该属性对应的用Inject标注的构造函数（这时候就发生联系了），剩下的工作就是初始化该属性的实例并把实例进行赋值。因此我们也可以给Component叫另外一个名字注入器（Injector）
	Component的新职责：
		把Module加入Component，modules可以加入多个Module。

目标类想要初始化自己依赖的其他类：
	用Inject注解标注目标类中其他类
	用Inject注解标注其他类的构造函数
	若其他类还依赖于其他的类，则重复进行上面2个步骤
	调用Component（注入器）的injectXXX（Object）方法开始注入（injectXXX方法名字是官方推荐的名字,以inject开始）	

Module
	作用：根本不可能把Inject注解加入这些类中，这时我们的Inject就失效了。Module可以把封装第三方类库的代码放入Module中
	
	Module其实是一个简单工厂模式，Module里面的方法基本都是创建类实例的方法。
使用Module与Component产生联系
	Module中的创建类实例方法用Provides进行标注，Component在搜索到目标类中用Inject注解标注的属性后，Component就会去Module中去查找用Provides标注的对应的创建类实例方法，这样就可以解决第三方类库用dagger2实现依赖注入了。

总结
	Inject，Component，Module，Provides是dagger2中的最基础最核心的知识点。奠定了dagger2的整个依赖注入框架。
	
	1、Inject主要是用来标注目标类的依赖和依赖的构造函数
	2、Component它是一个桥梁，一端是目标类，另一端是目标类所依赖类的实例，它也是注入器（Injector）负责把目标类所依赖类的实例
	注入到目标类中，同时它也管理Module。
	3、Module和Provides是为解决第三方类库而生的，Module是一个简单工厂模式，Module可以包含创建类实例的方法，这些方法用Provides来标注
#深入
Qualifier（限定符）
	实例类从创建：Inject、Module提供。Conponent查找优先级，Module>Inject
	
	依赖注入迷失：基于同一个维度条件下，若一个类的实例有多种方法可以创建出来，那注入器（Component）应该选择哪种方法来创建该类的实例呢？
	Qualifier（限定符）就是解决依赖注入迷失问题的。
注意
	dagger2在发现依赖注入迷失时在编译代码时会报错。

Component的组织方式概念：
	一个App只有一个Component的话，这个Componnet是很难维护、并且变化率很高，很庞大，这是有职责太多导致的。
	划分规则：
		1、要有一个全局的Component(可以叫ApplicationComponent),负责管理整个app的全局类实例
		2、每个页面对应一个Component，比如一个Activity页面定义一个Component，一个Fragment定义一个Component。当然这不是必须的，有些页面之间的依赖的类是一样的，可以公用一个Component。
	类实例共享：
		1、依赖方式
			一个Component是依赖于一个或多个Component，Component中的dependencies属性就是依赖方式的具体实现
		2、包含方式
			一个Component是包含一个或多个Component的，被包含的Component还可以继续包含其他的Component。这种方式特别像Activity与Fragment的关系。SubComponent就是包含方式的具体实现。
		3、继承方式
			官网没有提到该方式，具体没有提到的原因我觉得应该是，该方式不是解决类实例共享的问题，而是从更好的管理、维护Component的角度，把一些Component共有的方法抽象到一个父类中，然后子Component继承。
		
Singleton（单例）
	创建单例：
		1、在Module中定义创建全局类实例方法
		2、ApplicationComponent管理Module
		3、保证ApplicationComponent只有一个实例（在app的Application中实例化ApplicationComponent）
Scope（作用域）
	Scrop真正用处就在于Component的组织；

dagger2能带来哪些实惠？



##dagger2概念
	http://www.jianshu.com/p/cd2c1c9f68d4
	sample:https://github.com/niuxiaowei/Dagger2Sample
##