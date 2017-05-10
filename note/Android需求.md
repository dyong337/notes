##网络框架
1. Retrofit2+RxJava2.0
2. Debug模式下打印Log:发送请求log，响应请求log:HttpLoggingInterceptor                                     ok
3. 界面：
	1. 显示发送请求状态(显示Dialog)																		
	2. 第一次请求状态页面(Error Fragment页面,No Data Fragment,Lodding Fragment)
	3. 第一次成功后，加载更多失败后显示Item(提示：加载失败，请重新尝试)
	4. 请求过程中身份验证失效(Token失效)，显示Dialog重新登录或重新登陆Fragment
4. 请求成功：
	1. 在主线程中调用的处理接口或处理类(以匿名内部类的形式)
	2. 过滤请求结果(截取data数据;返回message数据)
	3. 主动取消Dialog显示
	4. 被动取消Dialog显示
5. 请求失败
	1. 同意状态码处理
	2. 取消Dialog
	3. 第一次请求显示状态页面(Error Fragmetn,No Data，Try Again)
6. 网络请求过程中错误处理
	1. 统一错误处理类，或接口
	2. Debug模式下打印出错误信息/弹出出错的Dialog
	3. Release保存错误信息在本地并显示给用户下一步处理的界面(Dialog)
7. 缓存
	1. 本地缓存(生命周期)
	2. 内存缓存(怎么实现)
	3. 第一加载并且没有网络的情况下显示本地缓存数据，并显示提示信息

##retrofit
Retrofit将您的HTTP API转换为Java接口。
使用注释来描述HTTP请求：
	1. URL参数替换和查询参数支持
	2. 对象转换为请求正文（例如，JSON，协议缓冲区）
	3. Multipart请求体和文件上传

1. 请求方法
		1. 申明请求方法和相对地址或绝对地址
		@GET("users/list")
		2. 指定请求参数
		@GET("users/list?sort=desc")
2. url操作
	1. @Path（）：
		动态修改url,使用｛name｝包裹要修改的地址，使用@Path(name)注解要修改的地址，name不需与{}中的值一样
	
			@Get("group/{id}/users")
			Call<List<User>> groupList(@Path("id") int groupId);
	2. @Query 请求参数
			
			@GET("group/{id}/users")Call<List<User>> groupList(@Path("id") int groupId, @Query("sort") String sort);
	3. @QueryMap 复杂的请求参数

			@GET("group/{id}/users")Call<List<User>> groupList(@Path("id") int groupId, @QueryMap Map<String, String> options);
3. 请求体
	1. @Body() ：对象请求体，对象将被Retrofit的实例转换；如果没有转换器，只能使用@RequestBody
	2. @RequestBody:

4. 表单编码和Multilpart
	1. @FormUrlEncode:表单编码数据(Form-encoded data),每个键值对(key-value)使用@Field(key)注解标明

			@FormUrlEncoded
			@POST("user/edit")
			Call<User> updateUser(@Field("first_name") String first, @Field("last_name") String last);
	2. @Multipart：multipart/form-data(多部分/表单数据),使用@Part()申明多个请求内容

			@Multipart
			@PUT("user/photo")
			Call<User> updateUser(@Part("photo") RequestBody photo, @Part("description") RequestBody description)
5. 请求头操作
	1. @Headers():这种方式并不会覆写请求头，拥有相通名字的请求头将在一个请求里面
		
				1.
				@Headers("Cache-Control: max-age=640000")
				@GET("widget/list")
				Call<List<Widget>> widgetList();
				2.
				@Headers({"Accept: application/vnd.github.v3.full+json","User-Agent: Retrofit-Sample-App"})
				@GET("users/{username}")
				Call<User> getUser(@Path("username") String username);
	2. @Head():动态修改请求投
	
				@GET("user")
				Call<User> getUser(@Header("Authorization") String authorization)
	3.可以使用OkHttp inteceptor为每个请求添加请求头
6. 同步 vs 异步 说明
	1. Call的实例既可以执行同步和异步请求。每个实例只能用一次，但可以使用clone()创建一个新的实例来使用。
	2. 在Android，callbacks将在主线程执行；在JVM，callbacks将在调用HTTP请求的线程上执行。

7. Retrofit配置
	1. Converters(转换)：
		1. HTTP Bodies默认反序列成OkHttp的ResponseBody，并且@Body只能接受RequestBody的类型
			1. 6中支持的转换类型
				1. Gson: com.squareup.retrofit2:converter-gson
				2. Jackson: com.squareup.retrofit2:converter-jackson
				3. Moshi: com.squareup.retrofit2:converter-moshi
				4. Protobuf: com.squareup.retrofit2:converter-protobuf
				5. Wire: com.squareup.retrofit2:converter-wire
				6. Scalars (primitives, boxed, and String): com.squareup.retrofit2:converter-scalars
		2. 自定义转换类型：集成Converter.Factory类



##Retrofit测试
1. 表头
	1. 使用@Headers()添加auth-key：可以
	2. 使用@Header()添加auth-key :可以，但是出现在请求体中
	3. 使用拦截器：ok
	
2. (多)表单上传(暂时不管)
	1. 上传多文件(暂时不管)
	2. 断点续传文件(暂时不管)	
3. 下载(暂时不管)
	 1. 断点下载
	 2. 多线程下载