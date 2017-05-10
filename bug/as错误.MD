## 导入application错误
	ERRO: Plugin with id 'com.android.application' not found.
		#导入项目的build.gradle文件为null或有错误，检查gradle/builld.gradle文件或直接copy一份能运行的build.gradle文件
## 打包错误 
	#Cannot recover key
		keystore可能被使用了，新建一个keystore即可

## 创建library错误 /创建Module错误
	# Plugin with id 'com.jfrog.bintray' not found. 
		原因：这是原因为该项目的原作者会把项目发布到maven中央仓库中，所以在gradle中添加了相关的maven发布任务。
			 咱们又不能连到maven的仓库，所以我们把你要导入的第三方build.gradle中做些修改。
		解决：找到我们import module的build.gradle只保留
				apply plugin: 'com.android.library'
	
				android {
				    .....
				}
				
				dependices(){
				    ....
				}
			其余的全部删除。