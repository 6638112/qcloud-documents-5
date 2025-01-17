## 创建销毁实例

### CreateTEduBoardController
创建白板控制类实例 
``` C++
EDUSDK_API TEduBoardController* CreateTEduBoardController(bool disableCefInit=false, const char *cefRenderPath=nullptr)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| disableCefInit | bool | 是否禁用CEF框架初始化，通常传默认值即可  |
| cefRenderPath | const char * | 使用SDK内部的CEF初始化时，用于指定自定义Render进程可执行程序的路径，UTF8编码，为空或nullptr表示使用SDK内置Render进程  |

#### 返回
白板控制类实例指针 

#### 警告
该接口必须在主线程调用 

>? 由于SDK基于CEF框架(BSD-licensed)实现，若您的程序中也使用了CEF框架，可能会存在冲突，我们为您提供了冲突解决方案：
> 1. 选用以下两种方法中的一种来启用自己的Render进程
> 	- 令 disableCefInit = false，cefRenderPath 指向您自己的Render进程
> 	- 令 disableCefInit = true，自行实现CEF初始化
> 2. 按下面说明，在您的Render进程内调用SDK的RenderProcessHandler
> 	- Render进程启动后调用接口获取一个sdkHandler实例，CefRefPtr<CefRenderProcessHandler> sdkHandler = (CefRenderProcessHandler*)GetTEduBoardRenderProcessHandler();
> 	- 在Render进程的CefApp中重写GetRenderProcessHandler方法，每次都返回以上sdkHandler
> 	- 若您需要自定义CefRenderProcessHandler，步骤B可返回自定义Handler，然后在自定义Handler的下面几个方法中，调用sdkHandler的对应方法
> 		- OnBrowserCreated
> 		- OnBrowserDestroyed
> 		- OnContextCreated 


### DestroyTEduBoardController
销毁白板控制类 
``` C++
EDUSDK_API void DestroyTEduBoardController(TEduBoardController **ppBoardController)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| ppBoardController | TEduBoardController ** | 指向白板控制类指针 |

#### 介绍
ppBoardController 指针会被自动置空 



## 日志相关接口

### GetTEduBoardVersion
获取SDK版本号 
``` C++
const EDUSDK_API char* GetTEduBoardVersion()
```
#### 返回
SDK版本号

#### 介绍
返回值内存由SDK内部管理，用户不需要自己释放 


### SetTEduBoardLogFilePath
设置白板日志文件路径 
``` C++
EDUSDK_API bool SetTEduBoardLogFilePath(const char *logFilePath)
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| logFilePath | const char * | 要设置的白板日志文件路径，包含文件名及文件后缀，UTF8编码，为空或nullptr表示使用默认路径  |

#### 返回
设置白板日志文件路径是否成功 

#### 警告
该接口必须要在第一次调用CreateTEduBoardController之前调用才有效，否则将会失败

#### 介绍

- 默认路径，Windows下为："%AppData%\\..\\Local\TIC\\teduboard.log"
- 默认路径，Linux下为："~/TIC/teduboard.log" 



## 高级功能接口

### EnableTEduBoardOffscreenRender
启用白板离屏渲染 
``` C++
EDUSDK_API bool EnableTEduBoardOffscreenRender()
```
#### 返回
启用离屏渲染是否成功 

#### 警告
该接口必须要在第一次调用CreateTEduBoardController之前调用才有效，否则将会失败

#### 介绍
启用离屏渲染时，SDK不再创建白板VIEW，而是通过onTEBOffscreenPaint回调接口将白板离屏渲染的像素数据抛出 


### GetTEduBoardRenderProcessHandler
获取SDK内部的CefRenderProcessHandler 
``` C++
EDUSDK_API void* GetTEduBoardRenderProcessHandler()
```
#### 返回
SDK内部的CefRenderProcessHandler 

#### 介绍
本接口详细使用方法参见CreateTEduBoardController接口说明 



## TEduBoardController
白板控制器 

## 设置 TEduBoardCallback 回调

### AddCallback
设置事件回调监听 
``` C++
virtual void AddCallback(TEduBoardCallback *callback)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| callback | TEduBoardCallback * | 事件回调监听  |

#### 警告
建议在Init之前调用该方法以支持错误处理 


### RemoveCallback
删除事件回调监听 
``` C++
virtual void RemoveCallback(TEduBoardCallback *callback)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| callback | TEduBoardCallback * | 事件回调监听  |



## 基本流程接口

### Init
初始化白板 
``` C++
virtual void Init(const TEduBoardAuthParam &authParam, uint32_t roomId, const TEduBoardInitParam &initParam=TEduBoardInitParam())=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| authParam | const TEduBoardAuthParam & | 授权参数  |
| roomId | uint32_t | 课堂ID  |
| initParam | const TEduBoardInitParam & | 可选参数，指定用于初始化白板的一系列属性值  |

#### 警告
使用腾讯云IMSDK进行实时数据同步时，只支持一个白板实例，创建多个白板实例可能导致涂鸦状态异常

#### 介绍
可用 initParam.timSync 指定是否使用腾讯云IMSDK进行实时数据同步 initParam.timSync == true 时，会尝试反射调用腾讯云IMSDK作为信令通道进行实时数据收发（只实现消息收发，初始化、进房等操作需要用户自行实现），目前仅支持IMSDK 4.3.118及以上版本 


### GetBoardRenderView
获取白板渲染View 
``` C++
virtual WINDOW_HANDLE GetBoardRenderView()=0
```
#### 返回
白板渲染View 


### AddSyncData
添加白板同步数据 
``` C++
virtual void AddSyncData(const char *data)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| data | const char * | 接收到的房间内其他人发送的同步数据 |

#### 介绍
该接口用于多个白板间的数据同步，使用内置IM作为信令通道时，不需要调用该接口 


### SetDataSyncEnable
设置白板是否开启数据同步 
``` C++
virtual void SetDataSyncEnable(bool enable)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| enable | bool | 是否开启 |

#### 介绍
白板创建后默认开启数据同步，关闭数据同步，本地的所有白板操作不会同步到远端和服务器 


### IsDataSyncEnable
获取白板是否开启数据同步 
``` C++
virtual bool IsDataSyncEnable()=0
```
#### 返回
是否开启数据同步，true 表示开启，false 表示关闭 


### Reset
重置白板 
``` C++
virtual void Reset()=0
```
#### 介绍
调用该接口后将会删除所有的白板页和文件 


### SetBoardRenderViewPos
设置白板渲染View的位置和大小 
``` C++
virtual void SetBoardRenderViewPos(int32_t x, int32_t y, uint32_t width, uint32_t height)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| x | int32_t | 要设置的白板渲染View的位置X分量  |
| y | int32_t | 要设置的白板渲染View的位置Y分量  |
| width | uint32_t | 要设置的白板渲染View的宽度  |
| height | uint32_t | 要设置的白板渲染View的高度 |

#### 介绍
白板渲染View有父窗口时，(x, y) 指定相对其父窗口的位置 


### GetSyncTime
获取同步时间戳 
``` C++
virtual uint64_t GetSyncTime()=0
```
#### 返回
毫秒级同步时间戳 


### SyncRemoteTime
同步远端时间戳 
``` C++
virtual void SyncRemoteTime(const char *userId, uint64_t timestamp)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| userId | const char * | 远端用户ID  |
| timestamp | uint64_t | 远端用户毫秒级同步时间戳  |


### CallExperimentalAPI
调用白板实验性接口 
``` C++
virtual const char* CallExperimentalAPI(const char *apiExp)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| apiExp | const char * | 要执行的白板相关JS代码  |

#### 返回
JS执行后的返回值转换而来的字符串 



## 涂鸦相关接口

### SetDrawEnable
设置白板是否允许涂鸦 
``` C++
virtual void SetDrawEnable(bool enable)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| enable | bool | 是否允许涂鸦，true 表示白板可以涂鸦，false 表示白板不能涂鸦 |

#### 介绍
白板创建后默认为允许涂鸦状态 


### IsDrawEnable
获取白板是否允许涂鸦 
``` C++
virtual bool IsDrawEnable()=0
```
#### 返回
是否允许涂鸦，true 表示白板可以涂鸦，false 表示白板不能涂鸦 


### SetAccessibleUsers
设置允许操作哪些用户绘制的图形 
``` C++
virtual void SetAccessibleUsers(const char **users, uint32_t userCount)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| users | const char ** | 指定允许操作的用户集，为nullptr表示不加限制  |
| userCount | uint32_t | 指定users参数包含的用户个数 |

#### 介绍
该接口会产生以下影响：
1. ERASER 工具只能擦除users参数列出的用户绘制的涂鸦，无法擦除其他人绘制的涂鸦
2. POINTSELECT、SELECT 工具只能选中users参数列出的用户绘制的涂鸦，无法选中其他人绘制的涂鸦
3. clear 接口只能用于清空选中涂鸦以及users参数列出的用户绘制的涂鸦，无法清空背景及其他人绘制的涂鸦
4. 白板包含的其他功能未在本列表明确列出者都可以确定不受本接口影响 


### SetGlobalBackgroundColor
设置所有白板的背景色 
``` C++
virtual void SetGlobalBackgroundColor(const TEduBoardColor &color)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| color | const TEduBoardColor & | 要设置的全局背景色 |

#### 介绍
调用该接口将导致所有白板的背景色发生改变 新创建白板的默认背景色取全局背景色 


### GetGlobalBackgroundColor
获取白板全局背景色 
``` C++
virtual TEduBoardColor GetGlobalBackgroundColor()=0
```
#### 返回
全局背景色 


### SetBackgroundColor
设置当前白板页的背景色 
``` C++
virtual void SetBackgroundColor(const TEduBoardColor &color)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| color | const TEduBoardColor & | 要设置的背景色 |

#### 介绍
白板页创建以后的默认背景色由SetDefaultBackgroundColor接口设定 


### GetBackgroundColor
获取当前白板页的背景色 
``` C++
virtual TEduBoardColor GetBackgroundColor()=0
```
#### 返回
当前白板页的背景色 


### SetToolType
设置要使用的白板工具 
``` C++
virtual void SetToolType(TEduBoardToolType type)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| type | TEduBoardToolType | 要设置的白板工具  |


### GetToolType
获取正在使用的白板工具 
``` C++
virtual TEduBoardToolType GetToolType()=0
```
#### 返回
正在使用的白板工具 


### SetCursorIcon
自定义白板工具鼠标样式 
``` C++
virtual void SetCursorIcon(TEduBoardToolType type, const TEduBoardCursorIcon &icon)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| type | TEduBoardToolType | 要设置鼠标样式的白板工具类型  |
| icon | const TEduBoardCursorIcon & | 要设置的鼠标样式  |


### SetBrushColor
设置画笔颜色 
``` C++
virtual void SetBrushColor(const TEduBoardColor &color)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| color | const TEduBoardColor & | 要设置的画笔颜色 |

#### 介绍
画笔颜色用于所有涂鸦绘制 


### GetBrushColor
获取画笔颜色 
``` C++
virtual TEduBoardColor GetBrushColor()=0
```
#### 返回
画笔颜色 


### SetBrushThin
设置画笔粗细 
``` C++
virtual void SetBrushThin(uint32_t thin)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| thin | uint32_t | 要设置的画笔粗细 |

#### 介绍
画笔粗细用于所有涂鸦绘制，实际像素值取值(thin * 白板的高度 / 10000)px，如果结果小于1px，则涂鸦的线条会比较虚 


### GetBrushThin
获取画笔粗细 
``` C++
virtual uint32_t GetBrushThin()=0
```
#### 返回
画笔粗细 


### SetTextColor
设置文本颜色 
``` C++
virtual void SetTextColor(const TEduBoardColor &color)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| color | const TEduBoardColor & | 要设置的文本颜色  |


### GetTextColor
获取文本颜色 
``` C++
virtual TEduBoardColor GetTextColor()=0
```
#### 返回
文本颜色 


### SetTextSize
设置文本大小 
``` C++
virtual void SetTextSize(uint32_t size)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| size | uint32_t | 要设置的文本大小 |

#### 介绍
实际像素值取值(size * 白板的高度 / 10000)px 


### GetTextSize
获取文本大小 
``` C++
virtual uint32_t GetTextSize()=0
```
#### 返回
文本大小 


### SetTextStyle
设置文本样式 
``` C++
virtual void SetTextStyle(TEduBoardTextStyle style)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| style | TEduBoardTextStyle | 要设置的文本样式  |


### GetTextStyle
获取文本样式 
``` C++
virtual TEduBoardTextStyle GetTextStyle()=0
```
#### 返回
文本样式 


### SetLineStyle
设置直线样式 
``` C++
virtual void SetLineStyle(const TEduBoardLineStyle &style)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| style | const TEduBoardLineStyle & | 要设置的直线样式  |


### GetLineStyle
获取直线样式 
``` C++
virtual TEduBoardLineStyle GetLineStyle()=0
```
#### 返回
直线样式 


### SetOvalDrawMode
设置椭圆绘制模式 
``` C++
virtual void SetOvalDrawMode(TEduBoardOvalDrawMode drawMode)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| drawMode | TEduBoardOvalDrawMode | 要设置的椭圆绘制模式  |


### GetOvalDrawMode
获取椭圆绘制模式 
``` C++
virtual TEduBoardOvalDrawMode GetOvalDrawMode()=0
```
#### 返回
椭圆绘制模式 


### Clear
清空当前白板页涂鸦 
``` C++
virtual void Clear(bool clearBackground=false, bool clearSelectedOnly=false)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| clearBackground | bool | 是否同时清空背景色以及背景图片  |
| clearSelectedOnly | bool | 是否只清除选中部分涂鸦  |

#### 警告
目前不支持清除选中部分的同时清除背景 


### SetBackgroundImage
设置当前白板页的背景图片 
``` C++
virtual void SetBackgroundImage(const char *url, TEduBoardImageFitMode mode)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| url | const char * | 要设置的背景图片URL，编码格式为UTF8  |
| mode | TEduBoardImageFitMode | 要使用的图片填充对齐模式 |

#### 介绍
当URL是一个有效的本地文件地址时，该文件会被自动上传到COS 


### SetBackgroundH5
设置当前白板页的背景H5页面 
``` C++
virtual void SetBackgroundH5(const char *url)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| url | const char * | 要设置的背景H5页面URL |

#### 介绍
该接口与SetBackgroundImage接口互斥 


### Undo
撤销当前白板页上一次动作 
``` C++
virtual void Undo()=0
```

### Redo
重做当前白板页上一次撤销 
``` C++
virtual void Redo()=0
```


## 白板页操作接口

### AddBoard
增加一页白板 
``` C++
virtual const char* AddBoard(const char *url=nullptr)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| url | const char * | 要使用的背景图片URL，编码格式为UTF8，为nullptr表示不指定背景图片  |

#### 返回
白板ID 

#### 警告
白板页会被添加到默认文件（文件ID为::DEFAULT)，自行上传的文件无法添加白板页

#### 介绍
返回值内存由SDK内部管理，用户不需要自己释放 


### DeleteBoard
删除一页白板 
``` C++
virtual void DeleteBoard(const char *boardId=nullptr)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardId | const char * | 要删除的白板ID，为nullptr表示删除当前页  |

#### 警告
只允许删除默认文件（文件ID为::DEFAULT）内的白板页，且默认白板页（白板ID为::DEFAULT）无法删除 


### PrevStep
上一步 每个Step对应PPT的一个动画效果，若当前没有已展示的动画效果，则该接口调用会导致向前翻页 
``` C++
virtual void PrevStep()=0
```

### NextStep
下一步 
``` C++
virtual void NextStep()=0
```
#### 介绍
每个Step对应PPT的一个动画效果，若当前没有未展示的动画效果，则该接口调用会导致向后翻页 


### PrevBoard
向前翻页 
``` C++
virtual void PrevBoard(bool resetStep=false)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| resetStep | bool | 指定翻到指定页以后是否重置PPT动画步数 |

#### 介绍
若当前白板页为当前文件的第一页，则该接口调用无效 


### NextBoard
向后翻页 
``` C++
virtual void NextBoard(bool resetStep=false)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| resetStep | bool | 指定翻到指定页以后是否重置PPT动画步数 |

#### 介绍
若当前白板页为当前文件的最后一页，则该接口调用无效 


### GotoBoard
跳转到指定白板页 
``` C++
virtual void GotoBoard(const char *boardId, bool resetStep=false)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| boardId | const char * | 要跳转到的白板页ID  |
| resetStep | bool | 指定翻到指定页以后是否重置PPT动画步数 |

#### 介绍
允许跳转到任意文件的白板页 


### GetCurrentBoard
获取当前白板页ID 
``` C++
virtual const char* GetCurrentBoard()=0
```
#### 返回
当前白板页ID

#### 介绍
返回值内存由SDK内部管理，用户不需要自己释放 


### GetBoardList
获取所有文件的白板列表 
``` C++
virtual TEduBoardStringList* GetBoardList()=0
```
#### 返回
所有文件的白板列表 

#### 警告
返回值不再使用时不需要自行delete，但是务必调用其release方法以释放内存占用 


### SetBoardRatio
设置当前白板页宽高比 
``` C++
virtual void SetBoardRatio(const char *ratio)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| ratio | const char * | 要设置的白板宽高比 |

#### 介绍
格式如: "4:3"、"16:9" 


### GetBoardRatio
获取当前白板页宽高比 
``` C++
virtual const char* GetBoardRatio()=0
```
#### 返回
白板宽高比，格式与SetBoardRatio接口参数格式一致 


### SetBoardScale
设置当前白板页缩放比例 
``` C++
virtual void SetBoardScale(uint32_t scale)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| scale | uint32_t | 要设置的白板缩放比例 |

#### 介绍
支持范围: [100，300]，实际缩放比为: scale/100 


### GetBoardScale
获取当前白板页缩放比例 
``` C++
virtual uint32_t GetBoardScale()=0
```
#### 返回
白板缩放比例，格式与SetBoardScale接口参数格式一致 


### SetBoardContentFitMode
设置白板内容自适应模式 
``` C++
virtual void SetBoardContentFitMode(TEduBoardContentFitMode mode)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| mode | TEduBoardContentFitMode | 要设置的白板内容自适应模式 |

#### 介绍
设置自适应模式后会影响所有后续白板内容操作,受影响接口包括：AddTranscodeFile 


### GetBoardContentFitMode
获取白板内容自适应模式 
``` C++
virtual TEduBoardContentFitMode GetBoardContentFitMode()=0
```
#### 返回
白板内容自适应模式 



## 文件操作接口

### ApplyFileTranscode
发起文件转码请求 
``` C++
virtual void ApplyFileTranscode(const char *path, const TEduBoardTranscodeConfig &config=TEduBoardTranscodeConfig())=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| path | const char * | 要转码的文件路径，编码格式为UTF8  |
| config | const TEduBoardTranscodeConfig & | 转码参数  |

#### 警告
本接口设计用于在接入阶段快速体验转码功能，原则上不建议在生产环境中使用，生产环境中的转码请求建议使用后台服务接口发起 

#### 介绍
支持 PPT、PDF、Word文件转码 PPT文档默认转为H5动画，能够还原PPT原有动画效果，其它文档转码为静态图片 PPT动画转码耗时约1秒/页，所有文档的静态转码耗时约0.5秒/页 转码进度和结果将会通过onTEBFileTranscodeProgress回调返回，详情参见该回调说明文档 


### GetFileTranscodeProgress
主动查询文件转码进度 
``` C++
virtual void GetFileTranscodeProgress(const char *taskId)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| taskId | const char * | 通过onTEBFileTranscodeProgress回调拿到的转码任务taskId  |

#### 警告
该接口仅用于特殊业务场景下主动查询文件转码进度，调用ApplyFileTranscode后，SDK内部将会自动定期触发onTEBFileTranscodeProgress回调，正常情况下您不需要主动调用此接口 

#### 介绍
转码进度和结果将会通过onTEBFileTranscodeProgress回调返回，详情参见该回调说明文档 


### AddTranscodeFile
添加转码文件 
``` C++
virtual const char* AddTranscodeFile(const TEduBoardTranscodeFileResult &result)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| result | const TEduBoardTranscodeFileResult & | 文件转码结果  |

#### 返回
文件ID 

#### 警告
当传入文件的URL重复时，文件ID返回为空字符串 
在收到对应的onTEBAddTranscodeFile回调前，无法用返回的文件ID查询到文件信息 

#### 介绍
本接口只处理传入参数结构体的title、resolution、url、pages字段 调用该接口后，SDK会在后台进行文件加载，期间用户可正常进行其它操作，加载成功或失败后会触发相应回调 文件加载成功后，将自动切换到该文件 


### DeleteFile
删除文件 
``` C++
virtual void DeleteFile(const char *fileId)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * | 要删除的文件ID |

#### 介绍
文件ID为nullptr时表示当前文件，默认文件无法删除 


### SwitchFile
切换文件 
``` C++
virtual void SwitchFile(const char *fileId, const char *boardId=nullptr, int32_t stepIndex=-1)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * | 要切换到的文件ID  |
| boardId | const char * | 切换文件并跳转到这个白板页  |
| stepIndex | int32_t | 跳转到白板页并切换到这个动画  |

#### 警告
该接口仅可用于文件切换，如果传入的fileId为当前文件ID，SDK会忽略其它参数，不做任何操作 

>? 文件ID为必填项，为nullptr或空字符串将导致文件切换失败 


### GetCurrentFile
获取当前文件ID 
``` C++
virtual const char* GetCurrentFile()=0
```
#### 返回
当前文件ID 


### GetFileInfo
获取白板中指定文件的文件信息 
``` C++
virtual TEduBoardFileInfo GetFileInfo(const char *fileId)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * |  |

#### 返回
文件信息 

#### 警告
每次调用该接口的返回值内容都指向同一块内存，若需要保存返回信息，请在拿到返回值后及时拷贝走 


### GetFileInfoList
获取白板中上传的所有文件的文件信息列表 
``` C++
virtual TEduBoardFileInfoList* GetFileInfoList()=0
```
#### 返回
文件信息列表 

#### 警告
返回值不再使用时不需要自行delete，但是务必调用其release方法以释放内存占用 


### GetFileBoardList
获取指定文件的白板ID列表 
``` C++
virtual TEduBoardStringList* GetFileBoardList(const char *fileId)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * | 文件ID  |

#### 返回
白板ID列表 

#### 警告
返回值不再使用时不需要自行delete，但是务必调用其release方法以释放内存占用 


### GetThumbnailImages
获取指定文件的缩略图，不支持默认文件（fileId=#DEFAULT） 
``` C++
virtual TEduBoardStringList* GetThumbnailImages(const char *fileId)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * | 文件ID  |

#### 返回
缩略图URL列表 

>? 用户在调用rest api请求转码时，需要带上 "thumbnail_resolution" 参数，开启缩略图功能，否则返回的缩略图url无效 


### ClearFileDraws
清空指定文件的所有白板涂鸦 
``` C++
virtual void ClearFileDraws(const char *fileId)=0
```
#### 参数

| 参数 | 类型 | 含义 |
| --- | --- | --- |
| fileId | const char * | 文件ID  |





