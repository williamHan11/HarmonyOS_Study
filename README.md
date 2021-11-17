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
    - 该问题等待华为云论坛解决

4. 使用Hiburn进行烧录时，点击Connect提示连接超时？
    - 在点击connect后需要按下开发板上的RST按键便可完成烧录
