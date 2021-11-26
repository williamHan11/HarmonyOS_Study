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

5. 在Ubuntu系统下搭建鸿蒙编译环境后，使用hb build进行编译时，提示permissionError：[Errno 13]，如何解决该问题？
    - 出现该问题的原因可能是在升级pip时提示的警告：
    - Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead.
    - 所以，解决方法有两个：一个是直接使用华为云提供的docker环境进行编译，另一个是使用非root权限进行编译。

# 2. 常用Linux指令

```
ssh harmonyos@<虚拟机IP地址> //在Window下访问Linux

sudo docker start ohos -i //启动docker（进行源码编译前的步骤）
```

# 3. 内核代码理解相关知识
1. void rtosv2_printer_main(void \*arg)中：其中void* arg表示可以传入任意类型的指针，这样的好处是可以传入任意的参数，如结构体指针，对象的指针等。

# 4. 子系统的理解
鸿蒙系统包含了子系统ABCDEF......每个子系统又分别包含了独立的组件如Aa/Ab/Ac、Ba/Bb/Bc/Bd等等，比如内核子系统kernel，包含了互相独立的LiteOS_M、LiteOS_A、Linux(目前是这三个)。

具体的芯片/开发平台，根据需要进行裁剪，比如Hi3861开发平台，选Aa组件不选Ab组件，完全不要B子系统(如多媒体子系统)，内核选LiteOS_M(硬件资源受限，跑不了LiteOS_A、Linux内核)。

而Hi3516开发平台，多媒体子系统、AI子系统等等，都会选上，内核也可以选LiteOS_A或Linux。

//build/lite/components/ 目录下，是鸿蒙系统能够提供的所有子系统列表，每一个json文件就是一个子系统，文件内又列出了本子系统提供的所有独立组件。

//vendor/hisilicon/ 目录下，是具体开发平台的配置需求文件，比如：

//vendor/hisilicon/hispark_pegasus/config.json 描述了Hi861平台跑起来所需要的子系统+组件列表，

//vendor/hisilicon/hispark_taurus/config.json 描述了Hi516平台跑起来所需要的子系统+组件列表.

编译代码的时候，会根据这些配置进行代码模块的裁剪，选择性地编译需要的模块。
