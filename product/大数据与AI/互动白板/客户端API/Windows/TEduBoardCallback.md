## TEduBoardCallback
白板事件回调接口 

## 通用事件回调

### onTEBError
白板错误回调 
``` C++
virtual void onTEBError(TEduBoardErrorCode code, const char *msg)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| code | TEduBoardErrorCode | 错误码，参见TEduBoardErrorCode定义  |
| msg | const char * | 错误信息，编码格式为UTF8  |


### onTEBWarning
白板警告回调 
``` C++
virtual void onTEBWarning(TEduBoardWarningCode code, const char *msg)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| code | TEduBoardWarningCode | 错误码，参见TEduBoardWarningCode定义  |
| msg | const char * | 错误信息，编码格式为UTF8  |



## 基本流程回调

### onTEBInit
白板初始化完成回调 
``` C++
virtual void onTEBInit()=0
```
#### 介绍
收到该回调后表示白板已处于可正常工作状态（此时白板为空白白板，历史数据尚未拉取到） 


### onTEBHistroyDataSyncCompleted
白板历史数据同步完成回调 
``` C++
virtual void onTEBHistroyDataSyncCompleted()
```

### onTEBSyncData
白板同步数据回调 
``` C++
virtual void onTEBSyncData(const char *data)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| data | const char * | 白板同步数据（JSON格式字符串） |

#### 介绍
收到该回调时需要将回调数据通过信令通道发送给房间内其他人，接受者收到后调用AddSyncData接口将数据添加到白板以实现数据同步 该回调用于多个白板间的数据同步，使用腾讯云IMSDK进行实时数据同步时，不会收到该回调 


### onTEBUndoStatusChanged
白板可撤销状态改变回调 
``` C++
virtual void onTEBUndoStatusChanged(bool canUndo)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| canUndo | bool | 白板当前是否还能执行Undo操作  |


### onTEBRedoStatusChanged
白板可重做状态改变回调 
``` C++
virtual void onTEBRedoStatusChanged(bool canRedo)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| canRedo | bool | 白板当前是否还能执行Redo操作  |


### onTEBOffscreenPaint
白板离屏渲染回调 
``` C++
virtual void onTEBOffscreenPaint(const void *buffer, uint32_t width, uint32_t height)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| buffer | const void * | 白板像素数据，大小为width * height * 4，像素以白板左上方为原点从左到右从上到下按BGRA排列  |
| width | uint32_t | 白板像素数据的宽度  |
| height | uint32_t | 白板像素数据的高度 |

#### 介绍
该回调只有在启用离屏渲染时才会被触发 



## 涂鸦功能回调

### onTEBImageStatusChanged
白板图片状态改变回调 
``` C++
virtual void onTEBImageStatusChanged(const char *boardId, const char *url, TEduBoardImageStatus status)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardId | const char * | 白板ID  |
| url | const char * | 白板图片URL  |
| status | TEduBoardImageStatus | 新的白板图片状态  |


### onTEBSetBackgroundImage
设置白板背景图片回调 
``` C++
virtual void onTEBSetBackgroundImage(const char *url)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| url | const char * | 调用SetBackgroundImage时传入的URL |

#### 介绍
只有本地调用SetBackgroundImage时会收到该回调 收到该回调表示背景图片已经上传或下载成功，并且显示出来 


### onTEBBackgroundH5StatusChanged
设置白板背景H5状态改变回调 
``` C++
virtual void onTEBBackgroundH5StatusChanged(const char *boardId, const char *url, TEduBoardBackgroundH5Status status)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardId | const char * | 白板ID  |
| url | const char * | 白板图片URL  |
| status | TEduBoardBackgroundH5Status | 新的白板图片状态  |



## 白板页操作回调

### onTEBAddBoard
增加白板页回调 
``` C++
virtual void onTEBAddBoard(const TEduBoardStringList *boardList, const char *fileId)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardList | const TEduBoardStringList * | 增加的白板页ID列表（使用后不需要自行调用Release方法释放，SDK内部自动释放）  |
| fileId | const char * | 增加的白板页所属的文件ID（目前版本只可能为::DEFAULT）  |


### onTEBDeleteBoard
删除白板页回调 
``` C++
virtual void onTEBDeleteBoard(const TEduBoardStringList *boardList, const char *fileId)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardList | const TEduBoardStringList * | 删除的白板页ID（使用后不需要自行调用Release方法释放，SDK内部自动释放）  |
| fileId | const char * | 删除的白板页所属的文件ID（目前版本只可能为::DEFAULT）  |


### onTEBGotoBoard
跳转白板页回调 
``` C++
virtual void onTEBGotoBoard(const char *boardId, const char *fileId)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardId | const char * | 跳转到的白板页ID  |
| fileId | const char * | 跳转到的白板页所属的文件ID  |


### onTEBGotoStep
白板页动画步数回调 
``` C++
virtual void onTEBGotoStep(uint32_t currentStep, uint32_t totalStep)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| currentStep | uint32_t | 当前白板页动画步数，取值范围 [0, totalStep)  |
| totalStep | uint32_t | 当前白板页动画总步数  |



## 文件操作回调

### onTEBFileTranscodeProgress
文件转码进度回调 
``` C++
virtual void onTEBFileTranscodeProgress(const char *path, const char *errorCode, const char *errorMsg, const TEduBoardTranscodeFileResult &result)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| path | const char * | 正在转码的本地文件路径  |
| errorCode | const char * | 文件转码错误码，无异常时为空字符串 ""  |
| errorMsg | const char * | 文件转码错误信息，无异常时为空字符串 ""  |
| result | const TEduBoardTranscodeFileResult & | 文件转码结果  |


### onTEBAddTranscodeFile
增加转码文件回调 
``` C++
virtual void onTEBAddTranscodeFile(const char *fileId)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * | 增加的文件ID |

#### 介绍
文件加载完成后才会触发该回调 


### onTEBDeleteFile
删除文件回调 
``` C++
virtual void onTEBDeleteFile(const char *fileId)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * | 删除的文件ID  |


### onTEBSwitchFile
切换文件回调 
``` C++
virtual void onTEBSwitchFile(const char *fileId)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * | 切换到的文件ID  |


### onTEBFileUploadProgress
文件上传进度回调 
``` C++
virtual void onTEBFileUploadProgress(const char *fileId, int currentBytes, int totalBytes, int uploadSpeed, double percent)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * | 正在上传的文件ID  |
| currentBytes | int | 当前已上传大小，单位bytes  |
| totalBytes | int | 文件总大小，单位bytes  |
| uploadSpeed | int | 文件上传速度，单位bytes  |
| percent | double | 文件上传进度，取值范围 [0, 1]  |


### onTEBFileUploadStatus
文件上传状态回调 
``` C++
virtual void onTEBFileUploadStatus(const char *fileId, TEduBoardUploadStatus status, int errorCode, const char *errorMsg)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * | 正在上传的文件ID  |
| status | TEduBoardUploadStatus | 文件上传状态  |
| errorCode | int | 文件上传错误码  |
| errorMsg | const char * | 文件上传错误信息  |



