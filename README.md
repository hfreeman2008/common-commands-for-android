# common-commands-for-android

android 开发的常用命令


这篇文章主要是一个对于android开发时常用的命令做的一个笔记。



![image_01](./images/image_01.png)

***

[<font face='黑体' color=#ff0000 size=40 >跳转到文章结尾</font>](#参考资料)

***

# 写在前面的话

集合自己常用的android开发命令，以方便自己查阅；



# 编译命令

模块编译：

- make
```makefile
make + 模块名
```


```makefile
time make services -j64 
time make framework-minus-apex -j64
time make framework-res -j64
```

- ninja

```makefile
ninja + 模块名
```


```makefile
./prebuilts/build-tools/linux-x86/bin/ninja -f out/combined-full_tb8765ap1_bsp_1g.ninja framework-minus-apex  -j32

./prebuilts/build-tools/linux-x86/bin/ninja -f out/combined-full_tb8765ap1_bsp_1g.ninja services  -j32

./prebuilts/build-tools/linux-x86/bin/ninja -f out/combined-full_tb8765ap1_bsp_1g.ninja framework-res  -j32
```


- mmm

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
