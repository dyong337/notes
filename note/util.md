##Android studioy依赖
1. 黄油刀
	
		1. 官网
			http://jakewharton.github.io/butterknife/
		2. 依赖
			compile 'com.jakewharton:butterknife:8.5.1'
			annotationProcessor 'com.jakewharton:butterknife-compiler:8.5.1'
		3. 混淆
			-keep class butterknife.** { *; }
			-dontwarn butterknife.internal.**
			-keep class **$$ViewBinder { *; }
			
			-keepclasseswithmembernames class * {
			    @butterknife.* <fields>;
			}
			
			-keepclasseswithmembernames class * {
			    @butterknife.* <methods>;
			}
2. Gilde
 
		1. 官网
			https://github.com/bumptech/glide
		2. 依赖
			compile 'com.github.bumptech.glide:glide:3.7.0'
  			compile 'com.android.support:support-v4:19.1.0'
		3. 混淆
			-keep public class * implements com.bumptech.glide.module.GlideModule
			-keep public enum com.bumptech.glide.load.resource.bitmap.ImageHeaderParser$** {
			  **[] $VALUES;
			  public *;
			}
			
			# for DexGuard only
			-keepresourcexmlelements manifest/application/meta-data@value=GlideModule
3. BaseRecyclerViewAdapterHelper

		1. 官网 
			https://github.com/CymChad/BaseRecyclerViewAdapterHelper
		2.  依赖
			1.  
			maven { url "https://jitpack.io" }
			compile 'com.github.CymChad:BaseRecyclerViewAdapterHelper:2.9.13'
		3.  混淆
			-keep class com.chad.library.adapter.** {
			   *;
			} 
4. 
 


##android xml转移字符
1. http://lanyan-lan.iteye.com/blog/1561500

##图片处理
1. 创建一个圆形图片

		int resourceId = card.getLocalImageResourceId(getContext());
		Bitmap bitmap = BitmapFactory
                .decodeResource(getContext().getResources(), resourceId);
		RoundedBitmapDrawable drawable = RoundedBitmapDrawableFactory.create(getContext().getResources(), bitmap);
		drawable.setAntiAlias(true);
		drawable.setCornerRadius(
                Math.max(bitmap.getWidth(), bitmap.getHeight()) / 2.0f);
		imageView.setImageDrawable(drawable);
		bitmap.recycle();
##RatingBar颜色、大小
	属性
	Android:isIndicator		RatingBar是否是一个指示器（用户无法进行更改）
	Android:numStars		显示的星型数量，必须是一个整形值，像“100”。
	android:rating			默认的评分，必须是浮点类型，像“1.2”。
	android:stepSize		评分的步长，必须是浮点类型，像“1.2”。
	修改大小
	style="@style/Widget.AppCompat.RatingBar.Small"
	修改颜色:
    1. android:theme="@style/RattingBar"
	2. styles.xml
	   	<style name="RattingBar" parent="Theme.AppCompat">
	        <item name="colorControlNormal">@android:color/holo_green_light</item>
	        <item name="colorControlActivated">@android:color/holo_red_light</item>
	    </style>
##各种默认样式
1. ImageCardView

		<style name="completedCardTheme" parent="Theme.Leanback">
	        <!--整体布局-->
	        <item name="imageCardViewStyle">@style/Widget.Leanback.ImageCardViewStyle</item>
	        <!--图片的样式-->
	        <item name="imageCardViewImageStyle">@style/Widget.Leanback.ImageCardView.ImageStyle</item>
	        <!--标题的样式-->
	        <item name="imageCardViewTitleStyle">@style/Widget.Leanback.ImageCardView.TitleStyle</item>
	        <!--文字内容的样式-->
	        <item name="imageCardViewContentStyle">@style/Widget.Leanback.ImageCardView.ContentStyle
	        </item>
	        <!--小图标的样式-->
	        <item name="imageCardViewBadgeStyle">@style/Widget.Leanback.ImageCardView.BadgeStyle</item>
	        <!--Info部分的样式-->
	        <item name="imageCardViewInfoAreaStyle">@style/Widget.Leanback.ImageCardView.InfoAreaStyle
	        </item>
	    </style>
2. HeaderRow


##RecyclerView
1. 滑到边界去除阴影
		
		 android:overScrollMode="never"

##添加水波纹效果
	
		<ripple xmlns:android="http://schemas.android.com/apk/res/android"
		    android:color="?attr/colorControlHighlight">
		    <item android:id="@id/mask">
		        <color android:color="@color/white" />
		    </item>
		</ripple>
	