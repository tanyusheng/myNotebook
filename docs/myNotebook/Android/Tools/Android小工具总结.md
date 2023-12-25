# adb的使用以及抓取trace的方法

### 如何安装adb

1. 从android官网下载adb工具包 [adb工具包](https://developer.android.com/studio/releases/platform-tools?hl=zh-cn#downloads)

2. 解压到本地，并复制文件文件，添加到系统环境变量中；
3. USB连接手机设备，终端输入`adb devices`如果看到手机的device ID则说明adb安装成功。
```
PS C:\Users\swift> adb devices
List of devices attached
bd04384d        device
```
> 如果adb连不上手机，请确保开发者模式中USB调试已打开，并尝试执行adb kill-server与adb start-server，并重启系统；

>  Xiaomi手机烧录系统镜像的步骤：
>
> 1.在平台下载对应rom
>
> 2.解压文件
>
> ```
> tar -xzvf 文件名.tgz
> ```
>
> ![01](localpicbed/Android小工具总结.assets/01.png)
>
> > 对应脚本文件解释：
> > sh文件为Linux下脚本，bat文件为Windows中执行的脚本；
> > flash_all表示刷机并清除全部数据；flash_all_except_storage表示保留用户数据（app、相册）、flash_all_lock表示刷机并上锁手机
>
> 3. 通过重启bootloader打开fastboot
>
> ```
> adb reboot bootloader
> ```
>
> 4. 执行烧录脚本
>
> ```
> ./flash_all_except_storage.sh
> ```
>

### adb常用命令

查看当前界面的Activity

```shell
adb shell dumpsys activity top | grep ACTIVITY
# 或者
adb shell dumpsys window windows | grep mObscuringWindow
```

启动对应的Activity

```shell
adb shell am start -n Activity全称
```

### 抓取trace的方法

1、创建一个config.pbtx文件

```
buffers: {
    size_kb: 63488
    fill_policy: DISCARD
}
buffers: {
    size_kb: 2048
    fill_policy: DISCARD
}
data_sources: {
    config {
        name: "linux.process_stats"
        target_buffer: 1
        process_stats_config {
            scan_all_processes_on_start: true
        }
    }
}
data_sources: {
    config {
        name: "android.log"
        android_log_config {
        }
    }
}

data_sources: {
    config {
        name: "linux.sys_stats"
        sys_stats_config {
            stat_period_ms: 1000
            stat_counters: STAT_CPU_TIMES
            stat_counters: STAT_FORK_COUNT
        }
    }
}
data_sources: {
    config {
        name: "linux.ftrace"
        ftrace_config {
            ftrace_events: "sched/sched_switch"
            ftrace_events: "power/suspend_resume"
            ftrace_events: "sched/sched_wakeup"
            ftrace_events: "sched/sched_wakeup_new"
            ftrace_events: "sched/sched_waking"
            ftrace_events: "power/cpu_frequency"
            ftrace_events: "power/cpu_idle"
            ftrace_events: "power/gpu_frequency"
            ftrace_events: "raw_syscalls/sys_enter"
            ftrace_events: "raw_syscalls/sys_exit"
            ftrace_events: "sched/sched_process_exit"
            ftrace_events: "sched/sched_process_free"
            ftrace_events: "task/task_newtask"
            ftrace_events: "task/task_rename"
            ftrace_events: "mm_event/mm_event_record"
            ftrace_events: "kmem/rss_stat"
            ftrace_events: "ion/ion_stat"
            ftrace_events: "dmabuf_heap/dma_heap_stat"
            ftrace_events: "kmem/ion_heap_grow"
            ftrace_events: "kmem/ion_heap_shrink"
            ftrace_events: "lowmemorykiller/lowmemory_kill"
            ftrace_events: "oom/oom_score_adj_update"
            ftrace_events: "ftrace/print"
            atrace_categories: "gfx"
            atrace_categories: "input"
            atrace_categories: "view"
            atrace_categories: "webview"
            atrace_categories: "wm"
            atrace_categories: "am"
            atrace_categories: "sm"
            atrace_categories: "audio"
            atrace_categories: "video"
            atrace_categories: "camera"
            atrace_categories: "hal"
            atrace_categories: "res"
            atrace_categories: "dalvik"
            atrace_categories: "rs"
            atrace_categories: "bionic"
            atrace_categories: "power"
            atrace_categories: "pm"
            atrace_categories: "ss"
            atrace_categories: "database"
            atrace_categories: "network"
            atrace_categories: "adb"
            atrace_categories: "vibrator"
            atrace_categories: "aidl"
            atrace_categories: "nnapi"
            atrace_categories: "rro"
            atrace_categories: "binder_driver"
            atrace_categories: "binder_lock"
            atrace_apps: "lmkd"
        }
    } 
}
duration_ms: 10000
```

2、将配置文件push到手机目录下

```shell
adb push config.pbtx /data/local/tmp/config.pbtx
```

3、下载自动录制trace脚本

```shell
curl -O https://raw.githubusercontent.com/google/perfetto/master/tools/record_android_trace
```

4、执行脚本抓取trace，脚本会自动将trace导入浏览器通过perfetto打开；

```shell
python3 record_android_trace.py -c config.pbtx -o trace_file.perfetto-trace
```

### 常用工具推荐

抓取Android屏幕的工具：[scrcpyls](https://github.com/Genymobile/scrcpyls)

> 参考文章：
>
> https://blog.csdn.net/lezhang123/article/details/117216140
>
> https://zhuanlan.zhihu.com/p/508526020
