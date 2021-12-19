# Flink流式处理平台使用说明

## 1.登陆管理页面

地址：http://192.168.10.66:8081/#/

**总览**

![1-flink-overview](..\..\images\flink\1-flink-overview.png)



## 2.作业

### 2.1 正在运行作业

![2-flink-jobs-running-jobs](..\..\images\flink\2-flink-jobs-running-jobs.png)



点击Job Name即可进入作业详情，可以查看作业状态，和删除作业。

![2-1-flink-jobs-job-detail](..\..\images\flink\2-1-flink-jobs-job-detail.png)



### 2.2 已完成作业

![2-flink-jobs-completed-jobs](..\..\images\flink\2-flink-jobs-completed-jobs.png)



### 3.任务管理者

查看当前任务占用内存，日志打印输出等

![3-flink-task-managers](..\..\images\flink\3-flink-task-managers.png)



### 4.作业管理

查看作业运行情况，包括内存，堆，配置、日志、打印输出等。

![4-flink-job-manager](..\..\images\flink\4-flink-job-manager.png)



### 5.提交作业

通过页面方式提交作业给实时计算平台。

![5-flink-submit-new-job](..\..\images\flink\5-flink-submit-new-job.png)

#### 5.1 新增作业

首先点击按钮“Add New”，在弹出的文件，选择JAR包

![5.1-flink-submit-add-new.png](..\..\images\flink\5.1-flink-submit-add-new.png)

等待上次完成。



#### 5.2 提交作业

在Entry Class编辑框中输入运行的作业类。如：com.ygai.StreamingJob。然后点击“Submit”按钮即可。

![5.2-flink-submit-job](..\..\images\flink\5.2-flink-submit-job.png)



YGAI系统平台依次运行如下Job

| 入口类                           | 作业名称      | 用途                       |
| -------------------------------- | ------------- | -------------------------- |
| com.ygai.flink.jobs.AbnormalJob  | Abnormal job  | 无定位、低电量、无CAN统计  |
| com.ygai.flink.jobs.AggregateJob | Aggregate job | 能耗和氢耗统计             |
| com.ygai.flink.jobs.AnalysisJob  | Analysis job  | 充放电、车辆上电未运行统计 |
| com.ygai.flink.jobs.FaultJob     | Fault job     | 故障统计                   |
| com.ygai.flink.jobs.StatusJob    | Status job    | 车辆实时状态               |
| com.ygai.flink.jobs.UpstreamJob  | Upstream job  | 上行数据处理，应答         |

