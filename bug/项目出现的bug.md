##免啦商家、免啦消费者
	1、真机调用相册，选择图片后，点X后程序奔溃
	
	2、FileUriExposedException：
	
		当你跨package域传递file://的URI时，接收者得到的将是一个无权访问的路径，因此，这将会触发FileUriExposedException。
		对于这类作，官方推荐的方式是使用FileProvider，当然你也可以使用ContentProvider。
	
			Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
			Uri uri = Uri.fromFile(sdcardTempFile);
			intent.putExtra(MediaStore.EXTRA_OUTPUT, uri);
	
		当你将targetSdk设置为Android N时，很不幸，在执行到这段代码时app就crash了，crash便是FileUriExposedException。
		把代码修改下，使用ContentProvider方式传递uri，这样在Android N上便可以正常运行了。
	
			Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
			ContentValues contentValues = new ContentValues(1);
			contentValues.put(MediaStore.Images.Media.DATA, sdcardTempFile.getAbsolutePath());
			Uri uri = context.getContentResolver().insert(MediaStore.Images.Media.EXTERNAL_CONTENT_URI,contentValues);
	3、SecurityException：
	
			如果你使用MODE_WORLD_READABLE或者 MODE_WORLD_WRITEABLE操作文件，将会触发SecurityException

##错误提示：Error: Expected resource of type styleable [ResourceType] 
	这个错误在编译运行时候并不会出现，但是当需要编译打包的时候，就会爆出这个异常。
		这个错误出现的位置位于自定义View中,代码如下:
			TypedArray ta = mContext.obtainStyledAttributes(attrs);
			boolean hasBottomLine = ta.getBoolean(0, false);
			boolean hasTopLine = ta.getBoolean(1, false);
		点击异常信息会定位到第三行，只有当 TypedArray 获取第二个属性以后数据时，才会出现此异常，ta.getBoolean(0, false) 这句则不会报错，其实这应该是一个警告，所以才会在调试的时候正常编译，但却在编译签名包的时候失败。
		解决办法
			解决办法就是在使用 TypedArray 的方法处，加上 @SuppressWarnings("ResourceType") ，这样即可过滤该警告，可以正常通过签名编译。例如：
				@SuppressWarnings("ResourceType")
				public void initView() {
				    TypedArray ta = mContext.obtainStyledAttributes(attrs);
				    boolean hasBottomLine = ta.getBoolean(0, false);
				    boolean hasTopLine = ta.getBoolean(1, false);
				    ta.recycle();
				}