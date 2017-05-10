#2016-12-20
	# apk瘦身
		apk瘦身一般有两条线，
			1.去除无用的代码，例如引用一个比较大的lib，只使用了其中很少的功能。其他无用的代码可以想办法去掉

			2.去除无用的资源文件，可能是第三方lib中的，也有可能是开发中引入了无用的资源

		代码：
			android {
			    buildTypes {
			        release {
			            minifyEnabled true
			            shrinkResources true
			        }
			    }
			}
		会报错：http://blog.csdn.net/nwsuafer/article/details/41943997

#app打包
	链接：
		1、http://blog.csdn.net/yeahhaey/article/details/50636820
		2、http://blog.csdn.net/zivensonice/article/details/51672846
		3、http://blog.csdn.net/u011200604/article/details/53337898
		4、http://blog.csdn.net/sinat_35845281/article/details/53006225
	多渠道：为了针对不同市场做出不同的一些统计，数据分析，收集用户信息。 

	多包名:产品的功能都一样，但是需要不同学校打出不同的包
		1、module下build.gradle：applicationId 设置后默认所有打出来的包都会覆盖AndroidMainfest中的package对应的值，就是平常我们所说的包名。
		2、添加productFlavors配置
			  productFlavors {
			        normal {
			        }
			
			        qinghua {
			            applicationId 'com.package.myapp.qinghua'
			        }
			    }
			normal配置中没有写任何东西，表示该配置下打出的包的所有的变量全部使用默认的值； 
			qinghua配置中把applicationId重新赋值为”com.package.myapp.qinghua”，表示该配置下打出的包包名要改为重新赋值后的值。

#包的命名：
	链接：http://www.tuicool.com/articles/73iu2mQ
	#一.PBL(Package By Layer)
		按层分包组织class，也就是传统做法，分为view, com, core, dao等等包，把职能相同的class放在一起
	#二.PBF(Package By Feature)
		按功能（模块）分包组织class，按app功能分为login、feedback、settings、orders、shipping等等﻿