# common-commands-for-android

android 开发的常用命令


这篇文章主要是一个对于android开发时常用的命令做的一个笔记。



![image_01](./images/image_01.png)

***

[<font face='黑体' color=#ff0000 size=40 >跳转到文章结尾</font>](#参考资料)

***

# 写在前面的话

集合自己常用的android开发命令，以方便自己查阅；

***

# 编译命令

模块编译前，需要执行如下命令配置环境：

```makefile
source build/envsetup.sh && lunch full_ac8257_demo_1g_32-user
或
source build/envsetup.sh && lunch full_ac8257_demo_1g_32-userdebug
```

***

## make
```makefile
make + 模块名
```


```makefile
全编：
make -j64

time make services -j64 
time make framework-minus-apex -j64
time make framework-res -j64

make MtkSystemUI -j32
make FMRadio -j64
make systemimage -j32
```

## ninja

- ninja编译命令

```makefile
ninja + 模块名
```


```makefile
./prebuilts/build-tools/linux-x86/bin/ninja -f out/combined-full_tb8765ap1_bsp_1g.ninja framework-minus-apex  -j32

./prebuilts/build-tools/linux-x86/bin/ninja -f out/combined-full_tb8765ap1_bsp_1g.ninja services  -j32

./prebuilts/build-tools/linux-x86/bin/ninja -f out/combined-full_tb8765ap1_bsp_1g.ninja framework-res  -j32
```

***

- ninja配置方法:

在项目根目录下：
如
~/K80130AA1/android/


1.复制必须的文件

```makefile
mkdir -p ~/bin ~/lib64
cp prebuilts/build-tools/linux-x86/bin/ninja ~/bin/
cp prebuilts/build-tools/linux-x86/lib64/libc++.so ~/lib64/
cp prebuilts/build-tools/linux-x86/lib64/libjemalloc.so ~/lib64/
cp prebuilts/build-tools/linux-x86/lib64/libjemalloc.so ~/lib64/
sudo cp prebuilts/build-tools/linux-x86/lib64/libjemalloc5.so /usr/lib
```


2.bashrc文件最后增加命令findm:

```makefile
alias findm="grep -rnws --include='*.[mb][kp]' 'LOCAL_MODULE\|LOCAL_PACKAGE_NAME\|name:'"
```

***

- ninja的优缺点

优点就是编译速度快，缺点就是如果我们有新文件的增减，编译不会生效，需要使用make 或mmm来编译 。


***

## mmm

```makefile
mmm + 模块路径
```

```makefile
time mmm frameworks/base/ -j32
time mmm frameworks/base/:services -j32
```

***

# adb 命令





***


# 参考资料



***

[<font face='黑体' color=#ff0000 size=40 >跳转到文章开始</font>](#common-commands-for-android)


***

![image_02](./images/image_02.png)
