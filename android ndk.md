#Android NDK
Androidndk开发打包时我们应该如何注意平台的兼容（x86,arm,arm-v7a）

一般有两方面的原因，具体原因如下：
##1.apk中有对应平台的文件夹，但是文件夹里却没有对应的so。
举个例子，apk中lib下面一旦出现x86文件夹，程序运行的时候就会去加载x86对应的库，但是如果此时x86文件夹没有将so放进来，则会遇到报错。
##2.第三方对平台的兼容策略与自己不一致。
可能第三方选择了只支持armeabi（假设某支付sdk），但是我们的游戏在Application.mk中配置了APP_ABI := all，如此，我们的游戏打包出 了所有平台的so，但是第三方却只有armeabi文件夹对应的so，造成程序运行异常，这种情况在开发期间最常见，一些小公司由于测试人员不足或者测试设备不足，上线后才发现这个问题也不奇怪。
二、对于平台的支持，我们应该如何选择。
armeabi-v7a确实是可以兼容armeabi的，而v7a的CPU支持硬件浮点运算，目前绝大对数设备已经是v7a了，所以为了性能上的更优，就不要为了兼容放到armeabi。 x86是可以兼容armeabi平台运行的，无论是armeabi-v7a还是armeabi，同时带来的也是性能上的损耗，另外需要指出的是，打包出的x86的so，总会比armeabi平台的体积更小，对于性能有洁癖的童鞋们，还是建议在打包so的时候支持x86。具体会有怎样的性能损耗，作者还不能说的非常清楚，可以访问下intel官方在csdn的博客。 总结一下在项目中的表现就是: 
如果项目只包含了 armeabi，那么在所有Android设备都可以运行； 如果项目只包含了 armeabi-v7a，除armeabi架构的设备外都可以运行； 如果项目只包含了 x86，那么armeabi架构和armeabi-v7a的Android设备是无法运行的； 如果同时包含了 armeabi， armeabi-v7a和x86，所有设备都可以运行，程序在运行的时候去加载不同平台对应的so，这是较为完美的一种解决方案，同时也会导致包变大。
