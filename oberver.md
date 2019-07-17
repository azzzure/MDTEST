
## 3.OBS概览

### 3.1威胁模型 
    只关心第三方恶意代码的点击拦截 
### 3.2记录到anchor element 的访问
    最简单的拦截就是通过js更改超链接的目的,因此obs会记录之
    通过劫持DOM API来做到这一点,哪怕使用了第三方库比如jQuery也跑不掉
    通过url记录每段代码的ID
### 3.3跟踪动态元素的创建
#### 3.3.1 HTML Anchor Elements
    给每个DOM中的anchor element分配scriptID
    拦截所有元素创建的API
#### 3.3.2 JS
    JS也能动态创建
    远程加载的JS一样能被检测到,并且它的源URL被分配为调用代码创建者的源URl
    on-event handlers的源URL是创建者或是接受者

### 3.4 监控JS事件监听器
    OBS会监控JS代码中所有的事件监听器
    这是通过hook addEventListener（）这个api实现的。其他重要的API还有windows.open windows.location
    事件监听器可能会被调用多次，OBS会通过event.currentTarget object 和 event.target是否相同来判断。
### 3.5 实现
    基于Chromium
    基于google 的V8 hook了DOM APIs
    增加了几个新的属性
    确保JS更改不了这些属性


## 4.方法学
    爬了Alexa top 250k的网站
### 4.1 数据收集
    1.渲染完成后的原始数据（静置45秒）（包括长宽高）
    2.和网页互动之后的数据（能点的全部点一遍）（禁用导航？）
### 4.2 第三方内容检测
    1 通过域名判断
    2 通过DNS判断
    3 动态加载的能容取决于加载他的代码
### 4.3 点击拦截检测
    1.超链接
    2.事件处理器
    3.视觉欺骗
### 4.3.1 超链接
    1.更改已经存在的超链接：谁最后动过谁背锅（第一方改第三方的不算）
    2.制造一个巨大的超链接（占有屏幕的75%）
### 4.3.2 事件处理器
    不是所有的事件处理器都有问题，只在意监听 navigation-related APIs
    webkit中的实现
    LocalDOMWindows::open() -> windows.open 
    Location::SetLocation() -> windows.location
    只要调用了这两个C++函数，就认为是 navigation-related APIs

    只要第三方的代码影响了其他元素，并且调用了这两个函数，就认为是拦截代码

    75%
### 4.3.3 视觉欺骗
#### 模仿 
    通过CSS *计算相似性*
#### 覆盖
####






## 5.实际使用的拦截?

## 6.

## 其他
    点了不想点的
    pixel-wise 像素级别的
