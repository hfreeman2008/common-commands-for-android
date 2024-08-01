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

# git 命令

- 代码提交
```makefile
git pull ./
git add .
git commit -m ""
git push
git push master HEAD:refs/for/master
```

- git reset

```makefile
1.git reset --soft HEAD^
未push但commit回退,会保留代码

2.git reset --hard HEAD^
不会保留未提交的代码，回退到上个版本

代码还原和清除修改
git reset --hard && git clean -df
git reset --hard id && git clean -df
git reset --hard id && git clean -df && git reset --hard HEAD^
git reset --hard id
```

- git rebase
```makefile
中止一个正在进行的 git rebase操作，并将分支恢复到操作之前的状态
git rebase --abort
```

***

# repo forall 全局搜索所有的git 库

```makefile
repo forall -p -c git log --grep 'edddd'
repo forall -p -c git log --after=2020-10-30 --before=2020-10-xx --author=xiaoming.he

repo forall -p -c git log --after=2023-10-19 --before=2023-10-24 > log_notification_code.txt

repo forall -p -c git diff


修改时间：Mon Feb 27 10:12:21 2023 +0800
./repo forall -c 'commitID=`git log --before "2023-2-27 10:12" -1 --pretty=format:"%H"`;git reset --hard $commitID'

批量取到特定日期修改
repo forall -c 'commitId=`git log --before "2019-09-12 07:00" -1 --pretty=format:"%H"`; git reset --hard $commitId'


批量清除目录中的临时文件/目录
repo forall -c "git clean -xdf"
repo forall -c "git clean -df && git reset --hard"
repo forall -c "git clean -df && git reset --hard && git reset --hard HEAD^"
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

## 如何查看应用的 oom_adj

以launcher为例：
```makefile
adb shell ps -ef | grep launcher
u0_a48         1611    442 0 01:42:20 ?     00:00:03 com.android.launcher3

得到应用id:1611

查看：
adb shell cat /proc/1611/oom_adj
adb shell cat /proc/1611/oom_score
adb shell cat /proc/1611/oom_score_adj


事实上应用的许多信息都在/proc/1611/目录下：
adb shell cat /proc/1611/cgroup
```



***

## aapt-查看apk的信息：

```makefile
aapt d badging ./Ctrip_RY.apk > aapt_info.txt
```

***

## 进入9008模式：


命令：
```makefile
adb reboot edl
```



```makefile
user版本进入9008刷机模式：
手机关机
长按电源键+音量上+音量下键
手机震动开机后，马上松开电源键和音量上键，也就是只按住音量下键，就可以进入9008刷机模式

对于执法记录仪：
长按电源，让机器开机，在开机时，长按一侧的中间的拍照和录音这二个键22秒，就可以进入9008刷机模式
```

*** 

## 获取屏幕亮度：

```makefile
adb root
cat /sys/class/leds/wled/brightness
adb shell cat /sys/class/leds/wled/brightness

adb root
adb shell cat /sys/class/backlight/panel0-backlight/brightness
adb shell echo 255 > /sys/class/backlight/panel0-backlight/brightness
```

查看亮度值：
```makefile
adb shell dumpsys display
mBrightness=
```

屏幕亮度：

```makefile
adb shell settings get system screen_brightness
adb shell settings get system screen_brightness_float
```

*** 

## ls-显示当前目录下的所有文件包括子目录下的文件

```makefile
ls -lhaR */*
```

*** 

## settings数据库

```makefile
usage: settings [–user NUM] get namespace key
settings [–user NUM] put namespace key value
settings [–user NUM] delete namespace key

‘namespace’ is one of {system, secure, global}, case-insensitive

If ‘–user NUM’ is not given, the operations are performed on the owner user.


修改数据库的键值：
adb shell settings get/set Global airplane_mode_on

adb shell settings put system/global [key] [value]
adb shell settings put system navigation_bar_key_mode 1
adb shell settings get system/global [key]

锁屏时间：
adb shell settings get system screen_off_timeout
屏幕亮度：
adb shell settings get system screen_brightness
adb shell settings get system screen_brightness_float
adb shell settings get secure screensaver_enabled
adb shell settings get system expand_screen_on


自动调节亮度：
adb shell settings get system screen_brightness_mode
```

默认settings数据库的存放位置：
```makefile
/data/user_de/0/com.android.providers.settings

adb shell ls -lha /data/misc/profiles/cur/0/com.android.providers.settings
adb shell ls -lha /data/misc/profiles/ref/com.android.providers.settings
adb shell ls -lha /data/data/com.android.providers.settings
adb shell ls -lha /data/user_de/0/com.android.providers.settings
adb shell ls -lha /config/sdcardfs/com.android.providers.settings


adb shell ls -lha /data/system/users/0
-rw------- 1 system system  13K 2018-10-15 09:02 settings_global.xml
-rw------- 1 system system 7.3K 2018-10-15 08:55 settings_secure.xml
-rw------- 1 system system 1.1K 2022-06-07 14:28 settings_ssaid.xml
-rw------- 1 system system  15K 2018-10-15 08:54 settings_system.xml
```


使用dumpsys读取settings数据：
```makefile
adb shell dumpsys settings
```

***

## input-模拟按键：

```makefile
模拟电源按键：
adb shell input keyevent 26
模拟menu按键：
adb shell input keyevent 1
模拟home按键：
adb shell input keyevent 3
模拟back按键：
adb shell input keyevent 4
模拟音量加按键：
adb shell input keyevent 24
模拟音量减按键：
adb shell input keyevent 25

KEYCODE_ENTER
adb shell input keyevent 66

adb shell sendevent /dev/input/event0 1 299 1 //按下menu
adb shell sendevent /dev/input/event0 1 299 0 //抬起menu
```


***

## svc 命令

```makefile
打开wifi，关闭wifi
Turn on/off Wi-Fi
adb shell svc wifi disable
adb shell svc wifi enable

打开数据连接，关闭数据连接
Turn on/off mobile data
adb shell svc data [enable|disable]

手机重启
adb shell svc power reboot

手机关机
adb shell svc power shutdown

打开和关闭蓝牙BT
adb root
adb shell svc bluetooth enable
adb shell svc bluetooth disable

打开和关闭NFC
adb root
adb shell svc nfc enable
adb shell svc nfc disable
```

***

## shutdown 命令

```makefile
shutdown -h +5    # 在5分钟后关机
shutdown -h +60
shutdown -h 20:00 # 在今天的20:00关机
```

***


## 启动关闭开机动画

```makefile
# 启动开机动画
adb shell setprop service.bootanim.exit 0
adb shell setprop ctl.start bootanim

# 关闭开机动画
adb shell setprop ctl.stop bootanim
# 或者
adb shell setprop service.bootanim.exit 1

开机动画时：
adb shell getprop ctl.stop bootanim
bootanim
```

***

## 查看 Service 信息

```makefile
查看系统服务列表：
adb shell service list
Found 1 services:
0 phone: [com.android.internal.telephony.ITelephony]

检查Service是否存在：
adb shell service check phone
Service phone: found

使用Service：
adb shell service call phone 2 s16 "10086"
```

如何获取正在运行的服务列表
```makefile
// List all services
adb shell dumpsys activity services
```

***
## 如何导出手机的apk(以chrome为例)

1.在手机上打开此应用

2.执行命令：
```makefile
adb shell "dumpsys | grep -i -A 75 'from top to'"

可以看到chrome的apk包名：com.android.chrome

Display #0 (activities from top to bottom):

Stack #113: type=standard mode=fullscreen
isSleeping=false
mBounds=Rect(0, 0 - 0, 0)
Task id #117
mBounds=Rect(0, 0 - 0, 0)
mMinWidth=-1
mMinHeight=-1
mLastNonFullscreenBounds=null
* TaskRecord{84350b5 #117 A=com.android.chrome U=0 StackId=113 sz=1}
userId=0 effectiveUid=u0a168 mCallingUid=u0a168 mUserSetupComplete=true mCallingPackage=com.android.chrome
affinity=com.android.chrome
intent={act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x14002000 cmp=com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity}
mActivityComponent=com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity
autoRemoveRecents=false isPersistable=true numFullscreen=1 activityType=1
rootWasReset=false mNeverRelinquishIdentity=true mReuseTask=false mLockTaskAuth=LOCK_TASK_AUTH_PINNABLE
Activities=[ActivityRecord{ee2b83d u0 com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity t117}]
askedCompatMode=false inRecents=true isAvailable=true
mRootProcess=ProcessRecord{16afbaf 8636:com.android.chrome/u0a168}
stackId=113
hasBeenVisible=true mResizeMode=RESIZE_MODE_RESIZEABLE mSupportsPictureInPicture=true isResizeable=true lastActiveTime=233617198 (inactive for 7s)
* Hist #0: ActivityRecord{ee2b83d u0 com.android.chrome/org.chromium.chrome.browser.ChromeTabbedActivity t117}
packageName=com.android.chrome processName=com.android.chrome
launchedFromUid=10168 launchedFromPackage=com.android.chrome userId=0
```

3.找到此apk的位置：
```makefile
adb shell pm path com.android.chrome
package:/data/app/com.android.chrome-p9BG4wfhjDRXOYVce8ZAFA==/base.apk
```

4.导出此apk：
```makefile
adb pull /data/app/com.android.chrome-p9BG4wfhjDRXOYVce8ZAFA==/base.apk ./chrome.apk
```


***

## screenrecord 录屏

```makefile
adb shell screenrecord /sdcard/screenrecord_black_play.mp4
adb pull /sdcard/screenrecord_black_play.mp4
```

***

## where 查看命令位置

查看adb命令位置：
```makefile
where adb
```

***

## date 查看时间

```makefile
adb shell date
Sat Sep 12 11:20:00 CST 2020
```

***

## 机器未解锁导致 adb remount 不成功

```makefile
1.设置--开发者选项--OEM解锁--on
2.adb reboot bootloader
3.fastboot flashing unlock (fastboot oem unlock)
4.fastboot getvar unlocked
5.fastboot reboot
6.adb root
7.adb disable-verity
8.adb reboot
9.adb wait-for-device
10.adb root
11.adb remount
```

***

## bootchart 使用命令

```makefile
adb shell ls -lha /data/bootchart
adb shell touch /data/bootchart/enabled
adb reboot
adb shell ls -lha /data/bootchart
adb root
adb remount
adb pull /data/bootchart/  bootchart/
adb shell rm /data/bootchart/enabled
adb shell ls -lha /data/bootchart
sudo apt-get install bootchart
sudo apt-get install pybootchartgui
tar -zcf bootchart.tgz *
bootchart bootchart.tgz
```

***


## 将查找到的文件全部删除

```makefile
find ./out/ -name "PvCanset*" -exec rm -rf {} \;
find ./out/ -name "PvCanset*" | xargs rm -rf
```

***

## chmod 命令

```makefile
chmod ug+w,o-w file1.txt file2.txt
chmod a+r file1.txt
chmod 777 file.txt

linux 权限:
r: 对应数值4
w: 对应数值2
x：对应数值1
－：对应数值0

owner-group-other

-rwx------:等于数字表示700。
-rwxr―r--:等于数字表示744。
-rw-rw-r-x:等于数字表示665。

who 用户类型    说明
u   user    文件所有者
g   group   文件所有者所在组
o   others  所有其他用户
a   all 所用用户,相当于 ugo
```


***

```makefile

```

```makefile

```

```makefile

```

```makefile

```

***


# 参考资料



***

[<font face='黑体' color=#ff0000 size=40 >跳转到文章开始</font>](#common-commands-for-android)


***

![image_02](./images/image_02.png)
