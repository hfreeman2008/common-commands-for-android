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

## make 命令

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
make clean
make otapackage

强制生成system.img文件
make snod


make kernel
make bootimage
make dtboimage


make update-api

编译kernel，刷机boot分区

make  kernel && make  bootimage
线刷 out\target\product\********\boot.img
```

## ninja 命令

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

# 日志相关的命令

```makefile
adb logcat -c && adb logcat -v time  *:E
adb wait-for-device && adb logcat -c && adb logcat -v time > log.txt
adb wait-for-device && adb logcat -c && adb shell "logcat | grep debug_hxm"
adb wait-for-device && adb logcat -c && adb shell "logcat | grep hxm_audio"
adb logcat -c && adb logcat -b events -b main -b system > log_001.txt
adb wait-for-device && adb logcat -c && adb logcat -b all > log_001.txt

adb logcat -b events -b main -b system > 2.5_check_slow_01.log
adb logcat -c && adb shell "logcat | grep dispatchTouchEvent"
adb logcat -c && adb shell "logcat | grep debug_hxm"
adb wait-for-device  && adb logcat -s AppTwoOneWidgetView
adb shell "logcat -c && logcat -b all | grep -E 'RecentsActivity|StatusBar|TaskStackView'"
adb shell "logcat | grep -i 'RecentsActivity dd'"

只显示Error级别的log：
adb logcat *:E

直接看Exception的命令：
adb logcat -s */E

crash日志的tag：
adb logcat -s AndroidRuntime
adb logcat -s DEBUG

adb logcat -s Watchdog

抓取kernel log：
adb shell cat /proc/kmsg > kernel.log
adb shell dmesg > kernel_001.log

查看touch日志：
adb shell getevent -l

查看应用启动的速度：
adb logcat -c && adb logcat -s ActivityTaskManager
```



***


# am 命令


- 启动应用

```makefile
打开开机向导界面：
adb shell pm enable com.android.provision/com.android.provision.WelcomActivity

adb shell am start -n com.android.provision/com.android.provision.WelcomActivity
adb shell am start -n com.android.stk/.StkMenuActivity
adb shell am start -n com.android.inputmethod.latin/com.android.inputmethod.latin.settings.SettingsActivity
adb shell am start -n com.android.settings/.inputmethod.InputMethodAndSubtypeEnablerActivity
adb shell am start -n com.android.settings/.Settings$UsbDetailsActivity
adb shell am start -n com.android.phone/com.android.phone.EmergencyDialer
adb shell am start -n com.water.flashlight/.MainActivity
adb shell am start -n com.android.permissioncontroller/com.android.packageinstaller.permission.ui.GrantPermissionsActivity
adb shell am start -n  com.android.systemui/.usb.UsbPermissionActivity
adb shell am start -n  com.google.android.marvin.talkback/com.google.android.accessibility.switchaccess.SwitchAccessPreferenceActivity
adb shell am start -n com.hisense.firstdemo/com.hisense.firstdemo.MainActivity
adb shell am start -n com.ju.baselib/com.ju.middleware.MainActivity
adb shell am start -n com.pve.installapk/com.pve.installapk.MainActivity
adb shell am start -n  com.android.settings/com.android.settings.Settings
adb shell am start -n  com.android.settings/com.autochips.atcsettings.mainui.MainActivity
adb shell am start -n  com.android.ServiceMenu/com.android.ServiceMenu.ServiceMenu
adb shell am start -a "test.action.USE_CAMERA_OR_MIC" -n  android.appthataccessescameraandmic/.AccessCameraOrMicActivity
adb shell am start  -n  com.retroidpocket.gameassistant/.activity.MainActivity
adb shell am start  -n  com.odin2.gameassistant/.activity.MainActivity
adb shell am start  -n com.odin2.gameassistant/com.odin2.gameassistant.activity.GamepadTestActivity
adb shell am start -n com.debug.loggerui/.MainActivity

升级界面：
adb shell am start  -n com.hytera.nrm/com.hytera.nrm.UpgradeActivity
adb shell am start  -n com.example.android.systemupdatersample/com.example.android.systemupdatersample.ui.BgActivity
adb shell am start  -n com.example.android.systemupdatersample/com.example.android.systemupdatersample.ui.BgActivity  --es "float_key" "start"  --ez "float_key_finish" "true"
adb shell am start  -n com.mcc.ler/com.mcc.ler.mvp.view.welcome.WelcomeActivity
adb shell am start  -n com.mcc.ler/com.mcc.ler.mvp.view.login.LoginActivity


工程模式：
adb shell am start -n com.mediatek.engineermode/.EngineerMode

查看对应应用的详细信息界面：
adb shell am start -a "android.settings.APPLICATION_DETAILS_SETTINGS" -d "package:sogou.mobile.explorer.flexpai"

```

***


- 发送广播
```makefile
adb shell am broadcast com.antutu.benchmark.full.3D_RUN
adb shell am broadcast -a  android.intent.action.BOOT_COMPLETED
adb shell am broadcast -a  xy.android.nextmedia
adb shell am broadcast -a  canbus.intent.action.key    --ei "canbus_key" "0x0A"
adb shell am broadcast -a  canbus.intent.action.key    --ei canbus_key 0x0B

adb shell am broadcast -a  ACTION_XY_AUTO_ADD_UP_BRIGHTNESS --ei "key_brightness" "10"
adb shell am broadcast -a  ACTION_XY_AUTO_ADD_UP_BRIGHTNESS

adb shell am broadcast -a com.xy.audiofocus.require

相当于：
Intent intent = new Intent();
intent.setAction("android.test");
intent.addFlags(Intent.FLAG_RECEIVER_INCLUDE_BACKGROUND);
intent.putExtra("key01","value01");
sendBroadcast(intent);

参数说明
-a action       指定intent操作，如android.intent.action.VIEW。
-f flags        向setFlags()支持的intent添加标记。如FLAG_RECEIVER_INCLUDE_BACKGROUND：0x01000000
–es extra_key extra_string_value        以键值对的形式添加字符串数据。
–ez extra_key extra_boolean_value       以键值对的形式添加布尔值数据。
–ei extra_key extra_int_value           以键值对的形式添加整数型数据。
–el extra_key extra_long_value          以键值对的形式添加长整型数据。
–ef extra_key extra_float_value         以键值对的形式添加浮点型数据。


adb shell am broadcast -a xy.save_zlink.activate_code_to_mcu   --es "activate_code" "111000222"
adb shell am broadcast -a android.action.MCU_KEY_OUT_TO_SDCARD -n com.xyauto.Settings/.InOutKeyEventReceiver
adb shell am broadcast -a android.action.MCU_KEY_IN_FROM_SDCARD -n com.xyauto.Settings/.InOutKeyEventReceiver
adb shell am broadcast -a android.intent.action.SET_BOOT_LOGO --es "filePath" "/sdcard/logo.bmp"  -n com.android.Settings/.NvRamOperationReceiver
adb shell am broadcast -a com.android.action.powerkey.reboot   -n com.android.Settings/.NvRamOperationReceiver
adb shell am broadcast -a com.smarteye.mpu  --ei "scanCode" "114"  --ez "longPress" "true"  -p com.smarteye.mpu
adb shell am broadcast -a com.smarteye.mpu  --ei "scanCode" "187"  --ez "longPress" "true"  -p com.smarteye.mpu
adb shell am broadcast -a com.smarteye.mpu  --ei "scanCode" "185"  --ez "longPress" "true"  -p com.smarteye.mpu

恢复出厂设置：
adb shell am broadcast -a android.intent.action.MASTER_CLEAR -f 0x01000000

```

***

- 其他命令

```makefile
停止应用
adb shell am force-stop com.android.launcher


查看am和堆列表信息：
adb shell am stack list

查看将要启动或退出app的包名
adb shell am monitor

获取配置信息
adb shell am get-config

强制生成进程的内存镜像 分析OOM
adb shell am dumpheap PIDxxx /data/xxx.hprof
```


***

# pm 命令


```makefile

显示应用列表：
adb  shell pm list package

查看包名和文件名对应表
adb shell pm list packages -f

显示应用位置路径：
adb  shell pm path com.jamdeo.tv.vod
清除应用数据：
adb  shell pm clear com.jamdeo.tv.vod


查看disable packagename
adb shell pm list packages -d

查看enable packagename
adb shell pm list packages -e

查看system app packagename
adb shell pm list packages -s

查看第三方app packagename
adb shell pm list packages -3

查看features列表
adb shell pm list features

# Enable/Disable package
adb shell pm enable [packagename]
adb shell pm disable [packagename]
adb shell pm enable com.android.provision/com.android.provision.WelcomActivity

给应用开权限：
adb shell pm grant com.android.kkkkk android.permmision.xxxx
```

***

# dumpsys 命令

```makefile
adb shell dumpsys activity activities
adb shell "dumpsys activity | grep -A 35 -i 'from top to'"
adb shell "dumpsys | grep -i -A 4 'mCurrentFocus'"

获取电池信息
adb shell dumpsys battery

如何获取正在运行的服务列表
// List all services
adb shell dumpsys activity services

查看亮度值：
adb shell dumpsys display
mBrightness=
```

***


# git命令

- 代码提交
```makefile
git pull ./
git add .
git commit -m ""
git push
git push master HEAD:refs/for/master
```

***

# 常用命令

## 电池相关的命令

- 电池电量值
```makefile
adb shell cat /sys/class/power_supply/battery/capacity

```

- 使用adb查看和修改电池信息
```makefile
1,获取电池信息
adb shell dumpsys battery

Current Battery Service state:
AC powered: false　　　　　　　　//false表示没使用AC电源
USB powered: true　　　　　　　　//true表示使用USB电源
Wireless powered: false　　　　 //false表示没使用无线电源
status: 2　　　　　　　　　　　　　//2表示电池正在充电，1表示没充电
health: 2　　　　　　　　　　　　　//2表示电池状态优秀
present: true　　　　　　　　　　 //true表示已安装电池
level: 63　　　　　　　　　　　　　//电池百分比
scale: 100　　　　　　　　　　　　　//满电量时电池百分比为100%（不确定是否正确）
voltage: 3781　　　　　　　　　　　//电池电压3.781V
temperature: 250　　　　　　　　　//电池温度为25摄氏度
technology: Li-ion　　　　　　　　//电池类型为锂电池

2,电池信息设置格式
$ adb shell dumpsys battery
　　set [ac|usb|wireless|status|level|invalid] <value>
　　unplug　　//模拟断开充电
　　reset　　　//复位

3,设置为AC/USB/Wireless充电
adb shell dumpsys battery set ac/usb/wireless 1
4,设置电池为充电状态
adb shell dumpsys battery set status 2
5,设置电池为非充电状态
adb shell dumpsys battery set status 1
6,设置电量百分比
adb shell dumpsys battery set level 100
7,设置断开充电（Android 6.0以上）
adb shell dumpsys battery unplug
8,复位，恢复实际状态
adb shell dumpsys battery reset
```

***



```makefile

```




***


# 参考资料



***

[<font face='黑体' color=#ff0000 size=40 >跳转到文章开始</font>](#common-commands-for-android)


***

![image_02](./images/image_02.png)
