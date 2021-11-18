# HarmonyOS_Study
鸿蒙操作系统的自学之路

# 1. 实验环境搭建中遇到的问题总结
1. 在VM中为虚拟机配置一个虚拟网卡后导致Linux系统无法打开，并报错：不能为虚拟电脑打开一个新任务E_FAIL (0x80004005)
    - 打开电脑的“网络和共享中心”->“更改适配器设置”
    - 选择标有“VirtualBox Host-Only...”的网络连接
    - 右键禁用，再重新打开
    - 之后就可以重新打开虚拟机了

2. 在Window下使用DevEco Device Tool进行烧录时，提示超时？
    - https://developer.huawei.com/consumer/cn/forum/topic/0203712817890840125?fid=0103702273237520029
    - 将upload_speed配置为115200即可解决

3. 接第2个问题，在可以进行烧录的情况下，只能烧录两个文件，进行第三个文件Hi3861_boot_signed_B.bin烧录时报错：Send head frame failed。
    - 该问题目前属于已知BUG，待解决
    - https://developer.huawei.com/consumer/cn/forum/topic/0204725298681650549?fid=0101587865002800104

4. 使用Hiburn进行烧录时，点击Connect提示连接超时？
    - 在点击connect后需要按下开发板上的RST按键便可完成烧录

# 2. 常用Linux指令

```
ssh harmonyos@<虚拟机IP地址> //在Window下访问Linux

sudo docker start ohos -i //启动docker（进行源码编译前的步骤）
```

# 3. 内核代码理解相关知识
1. void rtosv2_printer_main(void *arg)中：其中void* arg表示可以传入任意类型的指针，这样的好处是可以传入任意的参数，如结构体指针，对象的指针等。
