## TEduBoardDelegate-p
白板事件回调接口 

## 通用事件回调

### onTEBError:msg:
白板错误回调 
``` Objective-C
- (void)onTEBError:(TEduBoardErrorCode)code msg:(NSString *)msg 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| code | TEduBoardErrorCode | 错误码，参见TEduBoardErrorCode定义  |
| msg | NSString * | 错误信息，编码格式为UTF8  |


### onTEBWarning:msg:
白板警告回调 
``` Objective-C
- (void)onTEBWarning:(TEduBoardWarningCode)code msg:(NSString *)msg 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| code | TEduBoardWarningCode | 错误码，参见TEduBoardWarningCode定义  |
| msg | NSString * | 错误信息，编码格式为UTF8  |



## 基本流程回调

### onTEBInit
白板初始化完成回调 
``` Objective-C
- (void)onTEBInit
```
#### 介绍
收到该回调后表示白板已处于可正常工作状态（此时白板为空白白板，历史数据尚未拉取到） 


### onTEBHistroyDataSyncCompleted
白板历史数据同步完成回调 
``` Objective-C
- (void)onTEBHistroyDataSyncCompleted
```

### onTEBSyncData:
白板同步数据回调 
``` Objective-C
- (void)onTEBSyncData:(NSString *)data 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| data | NSString * | 白板同步数据（JSON格式字符串） |

#### 介绍
收到该回调时需要将回调数据通过信令通道发送给房间内其他人，接受者收到后调用AddSyncData接口将数据添加到白板以实现数据同步 该回调用于多个白板间的数据同步，使用腾讯云IMSDK进行实时数据同步时，不会收到该回调 


### onTEBUndoStatusChanged:
白板可撤销状态改变回调 
``` Objective-C
- (void)onTEBUndoStatusChanged:(BOOL)canUndo 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| canUndo | BOOL | 白板当前是否还能执行Undo操作  |


### onTEBRedoStatusChanged:
白板可重做状态改变回调 
``` Objective-C
- (void)onTEBRedoStatusChanged:(BOOL)canRedo 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| canRedo | BOOL | 白板当前是否还能执行Redo操作  |



## 涂鸦功能回调

### onTEBImageStatusChanged:url:status:
白板图片状态改变回调 
``` Objective-C
- (void)onTEBImageStatusChanged:(NSString *)boardId url:(NSString *)url status:(TEduBoardImageStatus)status 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardId | NSString * | 白板ID  |
| url | NSString * | 白板图片URL  |
| status | TEduBoardImageStatus | 新的白板图片状态  |


### onTEBSetBackgroundImage:
设置白板背景图片回调 
``` Objective-C
- (void)onTEBSetBackgroundImage:(NSString *)url 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| url | NSString * | 调用SetBackgroundImage时传入的URL |

#### 介绍
只有本地调用SetBackgroundImage时会收到该回调 收到该回调表示背景图片已经上传或下载成功，并且显示出来 


### onTEBBackgroundH5StatusChanged:url:status:
设置白板背景H5状态改变回调 
``` Objective-C
- (void)onTEBBackgroundH5StatusChanged:(NSString *)boardId url:(NSString *)url status:(TEduBoardBackgroundH5Status)status 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardId | NSString * | 白板ID  |
| url | NSString * | 白板图片URL  |
| status | TEduBoardBackgroundH5Status | 新的白板图片状态  |



## 白板页操作回调

### onTEBAddBoard:fileId:
增加白板页回调 
``` Objective-C
- (void)onTEBAddBoard:(NSArray *)boardIds fileId:(NSString *)fileId 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardIds | NSArray * | 增加的白板页ID列表（使用后不需要自行调用Release方法释放，SDK内部自动释放）  |
| fileId | NSString * | 增加的白板页所属的文件ID（目前版本只可能为::DEFAULT）  |


### onTEBDeleteBoard:fileId:
删除白板页回调 
``` Objective-C
- (void)onTEBDeleteBoard:(NSArray *)boardIds fileId:(NSString *)fileId 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardIds | NSArray * | 删除的白板页ID（使用后不需要自行调用Release方法释放，SDK内部自动释放）  |
| fileId | NSString * | 删除的白板页所属的文件ID（目前版本只可能为::DEFAULT）  |


### onTEBGotoBoard:fileId:
跳转白板页回调 
``` Objective-C
- (void)onTEBGotoBoard:(NSString *)boardId fileId:(NSString *)fileId 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardId | NSString * | 跳转到的白板页ID  |
| fileId | NSString * | 跳转到的白板页所属的文件ID  |


### onTEBGotoStep:totalStep:
白板页动画步数回调 
``` Objective-C
- (void)onTEBGotoStep:(uint32_t)currentStep totalStep:(uint32_t)totalStep 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| currentStep | uint32_t | 当前白板页动画步数，取值范围 [0, totalStep)  |
| totalStep | uint32_t | 当前白板页动画总步数  |



## 文件操作回调

### onTEBFileTranscodeProgress:path:errorCode:errorMsg:
文件转码进度回调 
``` Objective-C
- (void)onTEBFileTranscodeProgress:(TEduBoardTranscodeFileResult *)result path:(NSString *)path errorCode:(NSString *)errorCode errorMsg:(NSString *)errorMsg 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| result | TEduBoardTranscodeFileResult * | 文件转码结果  |
| path | NSString * | 正在转码的本地文件路径  |
| errorCode | NSString * | 文件转码错误码，无异常时为空字符串 ""  |
| errorMsg | NSString * | 文件转码错误信息，无异常时为空字符串 ""  |


### onTEBAddTranscodeFile:
增加转码文件回调 
``` Objective-C
- (void)onTEBAddTranscodeFile:(NSString *)fileId 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | NSString * | 增加的文件ID |

#### 介绍
文件加载完成后才会触发该回调 


### onTEBDeleteFile:
删除文件回调 
``` Objective-C
- (void)onTEBDeleteFile:(NSString *)fileId 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | NSString * | 删除的文件ID  |


### onTEBSwitchFile:
切换文件回调 
``` Objective-C
- (void)onTEBSwitchFile:(NSString *)fileId 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | NSString * | 切换到的文件ID  |


### onTEBFileUploadProgress:currentBytes:totalBytes:uploadSpeed:percent:
文件上传进度回调 
``` Objective-C
- (void)onTEBFileUploadProgress:(NSString *)fileId currentBytes:(int)currentBytes totalBytes:(int)totalBytes uploadSpeed:(int)uploadSpeed percent:(float)percent 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | NSString * | 正在上传的文件ID  |
| currentBytes | int | 当前已上传大小，单位bytes  |
| totalBytes | int | 文件总大小，单位bytes  |
| uploadSpeed | int | 文件上传速度，单位bytes  |
| percent | float | 文件上传进度，取值范围 [0, 1]  |


### onTEBFileUploadStatus:status:errorCode:errorMsg:
文件上传状态回调 
``` Objective-C
- (void)onTEBFileUploadStatus:(NSString *)fileId status:(TEduBoardUploadStatus)status errorCode:(int)errorCode errorMsg:(NSString *)errorMsg 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | NSString * | 正在上传的文件ID  |
| status | TEduBoardUploadStatus | 文件上传状态  |
| errorCode | int | 文件上传错误码  |
| errorMsg | NSString * | 文件上传错误信息  |


### onTEBH5FileStatusChanged:status:
H5文件状态回调 
``` Objective-C
- (void)onTEBH5FileStatusChanged:(NSString *)fileId status:(TEduBoardH5FileStatus)status 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | NSString * | 文件ID  |
| status | TEduBoardH5FileStatus | 文件状态  |


### onTEBVideoStatusChanged:status:progress:duration:
视频文件状态回调 
``` Objective-C
- (void)onTEBVideoStatusChanged:(NSString *)fileId status:(TEduBoardVideoStatus)status progress:(CGFloat)progress duration:(CGFloat)duration 
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | NSString * | 文件ID  |
| status | TEduBoardVideoStatus | 文件状态  |
| progress | CGFloat | 当前进度（秒）（仅支持mp4格式）  |
| duration | CGFloat | 总时长（秒）（仅支持mp4格式）  |



