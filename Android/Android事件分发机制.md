##View事件
  View内事件分发机制：

	/***
	 * View内事件分发机制：
	 *      1、调用dispatchTouchEvent方法:将事件分发个onTouch方法或onTouchEvent()方法
	 *      2、onTouch()方法:当View注册了OnTouchListener事件、View enable=true、OnTouchListener.onTouch()方法返回true,事件分发到该方法中并被消耗。
	 *      3、onTouchEvent()方法:事件在这里被消耗,进一步分发给OnClick、OnLongClick等事件。
	 *      不同都动作ACTION_DOWN、ACTION_UP、ACTION_MOVE都会触发dispatchTouchEvent方法。
	 *      ACTION_DOWN动作触发,会检测OnTouchListener.onTouch()返回值:
	 *          返回true,只接受ACTION_DOWN动作,余下事件不会再触发该方法,直到下一次ACTION_DOWN触发。
	 *          返回false,每个事件都会进入该方法。
	 * 参考：http://blog.csdn.net/guolin_blog/article/details/9097463
	 */
	     
  参考：http:blog.csdn.netguolin_blogarticledetails9097463

##ViewGroup事件
	ViewGroup事件分发机制

	/**
	 * ViewGroup的事件分发机制:
	 * 调用流程:1.ViewGroup.dispatchTouchEvent()分发事件
	 * 2.dispatchTouchEvent方法内判断调用onInterceptTouchEvent()方法返回值:
	 * 3.返回true:拦截事件,调用ViewGroup的onTouch()方法处理进行判断:
	 * ------->4.ViewGroup.onTouch()返回true:再次进入ViewGroup.dispatchTouchEvent(),将事件分发给剩余事件分发个ViewGroup.onTouch()方法处理
	 * ------->4.ViewGroup.onTouch()返回false:再次进入ViewGroup.dispatchTouchEvent(),将时间分发个ViewGroup.onTouchEvent()方法处理
	 * 3.返回false:找到事件的触发的所在的child view,再调用child view的dispatchTouchEvent()方法,在里面调用View.onTouch()方法:
	 * ------>返回true:事件被View.onTouch()方法消耗
	 * ------>返回false:事件被onTouchEvent()方法消耗;
	 */
 	
	参考：http://blog.csdn.net/guolin_blog/article/details/9153747/

##当父控件的点击事件没有响应，子控件的点击事件有响应时
原因分析:listview--itemview-button

	//listview.setOnClickListener()---报运行时错误ava.lang.RuntimeException: Don't call setOnClickListener for an AdapterView. You probably want setOnItemClickListener instead

	listView.setOnTouchListener()
	button.setOnTouchListener()--返回ture,onClick()没有响应，事件被onTouch()消耗；返回false,onclick()有响应，onTouch依然调用
	button.setOnClickListener()--参考button.setOnTouchListener();
	itemView.setOnTouchListener()--返回true,onClick()没有响应，事件被onTouch()消耗；返回false，onClick()有响应，onTouch()依然被调用
	itemview.setOnClickListener()--->参考itemView.setOnTouchListener();
	button.setEndable(false):onItemClcik()有响应，button.onTouch()没反应，这个跟enable有关。itemview.onTouch()返回true消耗事件，ItemClick()没有反应。
	
	分析：	
		1、当listview的item里面有一个child view 是enable的，事件被它消耗即使没有设置点击监听事件。可以设置该view enable=false,onItemClick会有响应(单不推荐，一般能点击，事件是会用到的);
		2、可以单独设置convertView点击事件，这样button可以点击、item也可以点击
		3、item的最外层布局添加 android:descendantFocusability="true"就好;
	listview的事件分发机制：事件进入最内层view，判断事件所在view的onTouch()是否返回true：是，该事件被这个view消耗；不是，判断view enable 是否等于true ：是，事件被消耗；不是返回上一次viewgroup，还是判断改viewgroup的onTouch()事件：是还是被消耗；不是，进入OnTouch()方法判断；
		

