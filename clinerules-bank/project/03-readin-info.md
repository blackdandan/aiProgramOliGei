1. 创建Activity需要继承BaseVMActivity，创建Fragment需要继承BaseVMFragment。Activity，Fragment需要查看BaseVMActivity，BaseVMFragment
2. 创建ViewModel需要继承BaseViewModel，询问继承了BaseViewModel的类的问题，需要查看BaseViewModel；
3. 编写逻辑需要符合MVVM架构
4. 数据相关的需要考虑环境@/core/src/main/java/com/readin/app/core/env/BaseEnvManager.kt 及其字类的设计
5. 网络相关的需要使用Retrofit，查看@/core/src/main/java/com/readin/app/core/network/retrofit/BaseRetrofitClient.kt ,
6. 打印日志使用@/core/src/main/java/com/readin/app/core/utils/log/ReaderLogger.java 
7. 公共Util使用或者新增，查看@/core/src/main/java/com/readin/app/core/utils 目录，其他查看各自模块的utils目录
8. 展示弹窗，使用@com.readin.app.core.uicommon.dialog.vipdialog.source下的
9. 代码设计应该考虑是否符合数据驱动，DataSource-Repository-ViewModel-StateFlow-View
10. 编写UI的时候注意按需使用@/core/src/main/res/values/dimens.xml ， @/core/src/main/res/values/colors.xml @/core/src/main/res/values/styles.xml 中的值