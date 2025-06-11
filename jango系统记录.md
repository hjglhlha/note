### 一、系统记录

该系统并没有数据库。所有数据的存放都是在本地（Deepfake->app01->static）

1、前端界面的修改：html文件存放位置：Deepfake->app01->templates

2、运行结果存放位置：Deepfake->app01->static

3、核心文件：views.py

位置：Deepfake->app01->views.py

1)views.py里的一些文件与urls.py（对应界面的网址）里的模块相对应

2）get：前端界面简单点击

3）post：在前端页面上传文件等的请求

4）919-931行代码（好像每个方法都会有这个）：定义字典，存放前端页面所需要的参数

5）调用其他方法中的方法：先在views.py的开头import。之后再直接用方法计算

4、Django->Data里面存放数据集

5、bug：页面尽量别缩放；上传文件后点击刷新后需要重新上传文件；该项目不能直接运行的原因：项目里所有文件写的都是绝对路径而不是相对路径，最好改过来

### 二、台式机也可以用VScode远程连接

并且深度强化学习方面的可视化需要，VScode也是完全可以胜任的

并且也是完全可以用VScode远程连接台式机，进行Django系统的开发

### 三、代码目录结构

- idea：这是JetBrains系列IDE（如PyCharm）的配置文件目录

- app01：

  这是项目中的一个Django应用，通常一个Django项目会包含一个或多个应用，每个应用负责**实现项目中的特定功能模块**。

  - migrations：存放数据库迁移文件的目录，每当应用的模型（models.py中的类）发生变化时，Django会生成迁移文件来记录这些变化，以便能够将数据库结构更新为与模型相匹配的状态，其下的*pycache*目录存放的是迁移文件的缓存。

  - static

    ：用于**存放静态文件**，如CSS样式表、JavaScript脚本、图片等，这些文件在网页前端会被引用，用于实现页面的样式和交互功能。该目录下有多个子目录，分别对应不同的功能模块或页面的静态资源，例如：

    - acc、analysis、Blend_result等目录分别存放与相应功能模块相关的静态资源，包括各自的css、js、images等子目录来分类存放不同类型的静态文件。
    - keyframe、keyframe_batch_result、keyframe_batch_video等目录可能与关键帧相关的视频处理或展示功能有关，存放对应的视频文件或其他相关资源。
    - output目录下有多个子目录，如000、996_056等，每个子目录下又有mri、plain_frames等子目录，这种结构可能是用于存放某种特定处理流程的输出结果，比如医学影像（mri）相关的处理结果以及普通帧（plain_frames）的存储。

  - templates：**存放HTML模板文件**的目录，Django通过模板引擎将视图（views.py中的函数或类）中的数据渲染到这些模板中，生成最终的HTML页面发送给客户端浏览器，从而实现动态网页的展示。

  - *pycache*：存放该应用编译后的Python字节码文件，以加快程序的运行速度，当Python解释器运行应用中的Python代码时，会将源代码编译成字节码缓存于此。

- Deepfake：这是**项目的配置目录**，通常包含项目的配置文件，如settings.py（项目配置）、urls.py（URL路由配置）等，用于**定义项目的全局设置和URL路径与视图函数的映射关系**，其下的*pycache*目录同样用于存放编译后的字节码文件。



### 3、在Windows系统里面跑起来

1、先创建一个虚拟环境

conda create -n django python=3.8

pip install django==3.2

2、在pycharm中应用这个虚拟环境

conda install -c anaconda mysqlclicent

在安装好django和mysql后运行以下指令即可成功运行：

```
启动Django服务器的指令：
python manage.py runserver

启动后访问的网址：
http://127.0.0.1:8000/Space_Upload_1/
```



### 4、在linux系统里面跑起来

1、先创建一个虚拟环境

conda create -n django python=3.8

pip install django==3.2

2、在pycharm中应用这个虚拟环境

linux系统里面的pycharm需要手动激活新创建的虚拟环境

conda install -c anaconda mysqlclicent

3、改相对路径，在views.py中改\,改到1766行



### 5、系统流程摸索

1、学习添加模块至系统的流程，大致如下：

2、对一个feature_Upload_1.html文件理解：

```
这个前端页面其实是典型的基于 Bootstrap + 原生 JS + Django 模板语法 的交互页面
1. 用户打开页面，选择多个视频文件
2. 前端自动读取并展示视频信息 + 播放预览
3. 用户点击“开始检测”
    -> AJAX 上传视频
    -> 后端开始处理
4. 前端启动 `startDetection()`，轮询进度条
5. 检测完成后（100%），隐藏进度条
6. 可跳转到结果页 or 展示结果

```

**3、开发系统流程：**

```
1、创建一个和Deepfake同级的文件夹，并且在这里面放入核心视频处理代码
2、在views.py中创建一个方法：这里面用来填写前端页面的处理逻辑，以及调用核心代码来处理视频（后端控制中心）
3、urls.py中放相应的访问链接（设置views的访问地址）
4、创建新的html前端交互页面（里面也可以设置一些类似views.py中用来交互的script）
```

4、感到的一些疑惑：

```
1、html页面是不是一般的情况都为：一些css的语句+scri脚本
HTML 负责结构；CSS 负责样式；JavaScript 负责交互
2、既然有了后端的views.py的交互，还需要javaScript的交互吗
更快交互
3、方法上面的@csrf_exempt是用来干什么的
跳过这个安全验证
4、系统开发和语言无关，思路大致相似
无论是 Python、Java、Go，本质都是： 输入 → 处理 → 输出
请求 → 处理 → 响应
模块 → 接口 → 流程控制
```



### 6、对已有代码中某些功能理解



一、Django系统修改

#### 1、前后交互过程

前端**html页面中通过script脚本**通过 **GET 请求访问 `/get_detection_progress` 路由**，所以 Django 后端必须有一个对应的 view 函数和 URL 映射

#### 2、页面开始检测按钮

views.py中get_detection_progress：

所有模块的开始检测都会**通过script调用该方法**。不同模块具体执行的方法通过detect_method 值区分走不同分支

**但只是模拟了进度，没有实际检测**

**点击“开始检测”后，报错修改：**

```
进去 views.py 文件的第 1207 行，把这一行：start = time.clock()  
改成：start = time.perf_counter()  
views.py 文件的第 1222 行，将end = time.clock()
改成：end = time.perf_counter()

```

#### 3、对Django系统前后端交互的理解：

**一个模块页面**的**所有动作交互**可以通过**一个在views.py中的对应视图方法**就可以完成。若是想为**某个特定的前端动作**交互**单独再在views.py中开一个视图方法**也是可以

#### 4、主页面结构以及功能分析（Space_Upload_1）

**只有两个功能：加载页面+对视频进行处理**

request.method为get的之后就仅仅根据log加载该html页面。而当request.method为post的时候就对视频进行处理并且保存日志文件


**用get和post区别处理改页面方式**

点击前端的任何一个 URL 请求，Django 系统都会自动转为 GET 或 POST 并传给 views.py 的方法

3）、

**浏览器根据html内容返回get或者post**

浏览器根据html中的内容自动判别，并且发给Django系统

有很多样式有默认的请求方式，当然也可以通过method方式（form、javaScript）来指定





5.7

1、对于Django系统其实就抓住几个核心文件即可：

```
前端样式的修改：html文件

后端逻辑修改：views.py文件

前后端：url文件
```

2、页面的后端代码

```
1）：我负责的“野外光照”模块对应在views.py中的577-683行的：def Blend_Upload_1(request):
2）：前端页面中对应下方结果跳转对应views.py中的687-707行的：def Blend_result_history(request):
```

Blend_Update上传视频后立即显示检测结果

Blend_batch_Update在“批量历史记录页面”点击某条记录时

各函数关系：

```
前端上传视频
    ↓
Blend_Upload_1（保存到 static/video/）
    ↓
用户点击“开始检测”
    ↓
Blend_detect（读取视频，执行检测）
    ↓
Blend_Update（前端发送 rowData[]，获取图片+置信度等）		局部页面更新
    ↓
前端显示检测结果卡片
    ↓
用户点击“查看详情”
    ↓
Blend_result_history（从日志中读历史记录，渲染页面）		返回新的html页面

```

3、页面前端代码：

```
视频上传部分前端：Blend_Upload_1.html
结果生成部分前端：Blend_deepfake_result.html
```

views.py中遍历log值传入html



要干的事情：

```
还得研究一下光照方向是否能够固定？
1）将所有的结果转化成视频帧并且存好
2）稍微改一下views.py中blend的方法，或者自己重新写一个ReliTalk方法，来调用预生成结果并返回给前端
3）更改相应的前端页面，以适配后端即将返回给前端的结果
```

待优化：

将帧转化成更加连贯的视频



```
return render(request, 'Blend_deepfake_result.html',
                  {'vid': vid, 'pred': pred, 'time': run_time, 'TF': 'REAL',
                   'log': zip(log_1, log_2, log_3, log_4, log_5)})
```

5.13

1、解决PSNR值为0：两个视频路径错误，导致计算错误

2、待解决：的页面的下部分的列表，即log内容可以按行展示，不过上半部分好像只是个空壳，

**视频不能播放（浏览器兼容问题）解决如下：**

1)重新在系统里面下载ffmpeg：https://www.gyan.dev/ffmpeg/builds/#release-builds

本地文件对应位置：D:\BaiduNetdiskDownload\ffmpeg-7.0.2-full_build\bin

再使用以下指令进行转换：

```
ffmpeg -i light_video.mp4 -f lavfi -t 10.75 -i anullsrc=channel_layout=stereo:sample_rate=44100 -shortest -vcodec libx264 -pix_fmt yuv420p -acodec aac -strict -2 outlight_video.mp4
```

目前再前端写死了路径

3、待解决：就是在我的上述前端表格可以正常的显示我的log里面信息了，不过我想点击某一行去将结果加载想去跳转至。

成功解决：通过修改后端代码：def Blend_result_history(request):实现



5.15

1、到时候环境配置也是一个棘手的问题（已解决，可以统一为一个）

gpt给出的建议是我的：数据预处理、模型运行所需的虚拟环境是可以统一为一个虚拟环境的

集成版环境：（6分钟就装好了）

```
1、
conda create -n relitalk-env python=3.8 -y
conda activate relitalk-env
2、
sudo apt install ffmpeg
pip install torch==1.11.0+cu113 torchvision==0.12.0+cu113 torchaudio==0.11.0 --extra-index-url https://download.pytorch.org/whl/cu113
pip install fvcore
pip install --no-index --no-cache-dir pytorch3d -f https://dl.fbaipublicfiles.com/pytorch3d/packaging/wheels/py38_cu113_pyt1110/download.html
3、
pip install numpy scipy scikit-image opencv-python imageio==2.15.0 matplotlib trimesh
pip install chumpy torchfile ninja yacs kornia pyhocon
pip install face_alignment==1.3.5 facenet_pytorch==2.5.2 --no-cache-dir --no-deps
pip install wandb plotly
4、
还要安装gcc-7
```

整理一下运行ReliTalk代码笔记：笔记位置：C:\Users\future\Desktop\深度学习\2025-0211至.md



5.16

1、实验证明，我的ReliTalk代码只能够在服务器上运行

是否可以跨平台：

 1）gpt说法：
**CUDA 扩展编译失败，通常需要 Linux + 正确 CUDA 开发环境**

ReliTalk 依赖一个 **CUDA 自定义算子模块（`standard_rasterize_cuda`）**，而这个模块目前的构建环境不支持 Windows —— 它在构建过程中明确需要用到 `gcc-7` 这样的 Linux 下特定版本的编译器，还依赖 `ninja` 和 `nvcc` 来编译 CUDA kernel，这一整套流程 **在 Windows 上极难复刻甚至不可行**。

2）本质原因：

**ReliTalk 本身并不跨平台**：它的预处理模块（如 `DECA`）用了大量 Linux 原生 CUDA 编译逻辑；

2、之后安排

将RelilTalk的推理放在服务器运行，使用接口将其集成至系统







3、再考虑将模型核心代码集成进去。（直接写一个linux版本）且慢，等等其他人

```
完成，
但是还需要写一个脚本放入，到时候用来自动调用模型黑盒执行
```

4、再研究一下ReliTalk的代码，看看能否控制光照方向

5、再尝试一下是否能够集成为一个虚拟环境

可以，参考5.15

### end