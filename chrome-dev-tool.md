# chrome dev tools

## **一 打开方式**

1. F12
2. crtl+shift+I
3. chrome 菜单中更多工具 > 开发者工具

## **二 了解常用面板**

> #### 1 设备模式 Device Mode

- 模拟移动设备 窗口和功能介绍

- 通过 Throttle 列表 可以限制网络流量和 cpu 占有率 来模拟一些弱网及性能稍差的手机[[1](#mark1)]

  - Mid-tier mobile 可模拟快速 3G 网络，并限制 CPU 占用率，以使模拟性能比普通性能低 4 倍[[2](#mark2)]
  - Low-end mobile 可模拟慢速 3G 网络，并限制 CPU 占用率，以使模拟性能比普通性能低 6 倍

- 自适应视口模式，也可以通过 show media queries 来调整视口宽度
- show device frame 展示手机边缘框架 可以看到大致的整体效果 （rotate 看整体横屏的效果）
- 替换地理位置 自定义和控制 devtools > More tools > sensors 可以方便移动端调试
  ![](imgs/location.png)

> #### 2. 元素面板

- 直接在页面上修改 dom 和 styles

  - dom 可以直接点击进行拖动或者`[Ctrl]+[⬆]/ [ctrl] + [⬇]`来移动元素的位置

  - 可以给 dom 做断点，如属性断点会停留在改变属性的 js 处  
    vue 的元素渲染 如 v-if 展示和隐藏 是通过插入和移除元素完成的

    ```
        function insertBefore (parentNode, newNode, referenceNode) {
        parentNode.insertBefore(newNode, referenceNode);
      }

      function removeChild (node, child) {
        node.removeChild(child);
      }
    ```

  - dom 选择元素 按下 h 键 来切换被选择元素的显示和隐藏内容 （用途比如 截图不想看到一些敏感信息）

  - 修改样式时可以使用 color 选择器 / 阴影编辑器 / Cubic bezier(贝塞尔) 编辑器(针对动画效果的) 以及样式后面的插入样式规则的按钮去自定义添加 style
    添加完样式可以直接点击对应的文件 -->右 click local modifications...去查看你该文件里所有的改变

> #### 3. 源代码面板

- 断点调试 可以进行添加条件断点

  ```
    let myVars = 0
    Array.from(Array(20))
      .map(Math.random)
      .forEach((v, i) => {
        myVars = myVars + v * i
      })
  ```

- XHR/fetch 断点

  - 当你想捕获已发送的`ajax`请求中的特定时刻，可以使用 XHR/fetch breakpoint,这些只能在 Source 面板中设置,可以添加部分 `URL`作为触发器或监听任何请求
    ![](imgs/xhrFetch.png)

- Filesystem add floder 后允许在页面编辑就会实时更新到文件里面

  - 用于直接修改本地文件
  - chrome 会智能的将 filesystem 与 localhost 请求绑定(link to filesystem),
  - 绑定后有一个绿色的标志，这样就不会 cache 请求，(127.0.0.1)网络请求就会实时生效。

  ![](https://user-gold-cdn.xitu.io/2018/12/29/167f5b37db4e23ac?imageslim)

* snippets 可以新建一个存放的代码片段 方便在 console 中运行 关闭浏览器后不会消失. 浏览器刷新时也不会执行，除非手动执行
  ```
   console.log($$('li')
    .map(el => parseInt(el.innerText))
    .reduce((total, value) => total + value));
  ```

> #### 4. 网络面板

- 记录网络行为，加载过程的屏幕快照（Filmstrip）可以被放大查看
  ![](imgs/screenShot.png)

- 查看 DOMContentLoaded 和 load 两个事件

  - DOMContentLoaded dom 内容加载完毕 从页面空白到页面展示出内容会触发 DOMContentLoaded 事件，HTML 文档被加载和解析完成；[[4](#mark4)]
  - load 页面所有资源度被加载完后才会触发 load 事件

- 查看单个资源的详细情况

  - Headers HTTP headers 相关的信息
  - Preview 响应数据的预览 （json image ，text 等）
  - Response HTTP 响应数据
  - Timing 资源请求生命周期的细粒度细分 详见文章：[[3](#mark3)]

- filter 如下面几种情况

  - larger-than:2k
  - status-code:200
  - domain:\*.com
    ![](imgs/filter.png)

- copy save clear block request 信息
  - 选择一个资源或者接口数据测试每一个功能

<!-- > #### 5. 内存面板

- 在 javascript profiler 面板 里面记录 cup profile
- 在 memory 面板 记录
  - 了解内存使用量 -->

> #### 5. 应用面板

- 检查和管理存储、数据库与缓存
  - 清除缓存，可以清除本域名下的不同内容
    ![](imgs/clearCatch.png)

> #### 6. 安全面板

- 使用安全面板调试混合内容问题，证书问题等等
  - 只要页面有 http 请求就会显示是 `This page is not secure.`
    ![](imgs/mixureSecure.png)
  - 页面只有纯 https 请求显示 `This page is secure (valid HTTPS).`
    ![](imgs/Secure.png)
    
> #### 7. 内存面板memory
 -浏览器方法查看内存占用情况
    打开开发者工具，选择 Memory
    在右侧的Select profiling type字段里面勾选 timeline
    点击左上角的录制按钮
    在页面上进行各种操作，模拟用户的使用情况
    一段时间后，点击左上角的 stop 按钮，面板上就会显示这段时间的内存占用情况


> #### 7. 最常用的控制台面板

- 展现形式 面板和抽屉式两种 使用 Esc 切换
- 消息堆叠 DevTools 设置中启用 Show timestamps 去给 log 加上时间戳 切换为独立的行条目
- 历史消息
  - 清除 按钮 clear console/clear()/ctrl_L
  - 保存 可以进行错误信息的堆栈跟踪 console
- \$
  - \$0 当前我们选中的节点 \$1 上一次选择的节点 ... \$2 \$3 \$4 以此类推
  - \$\_ 是对上次指向结果的引用
  - \$ 查询元素返回一个 js 对象， \$('div')返回查询到的第一个 div 元素的，类似 `document.querySelector` 功能
    注：JS 对象，是一个名值对的无序集合。jquery 对象，是 jquery 特有的对象，只有调用 jquery 框架才存在。其实 jquery 对象，也是一种 js 对象。jquery 对象和 js 对象可以相互转换
  - \$\$ 查询元素返回符合条件的数组， \$\$('div')返回所有 div 元素组成的数组 类似 `Array.from(document.querySelectorAll('div')) === \$\$('div')`的功能
  - 在 Chrome 插件:Console Importer 的帮助之下，你可以快速的在 console 中引入和把玩一些 npm 库。
- copy & store
  - copy(变量名/\$\_/other)
  - store as global variable 之后会创建一个临时变量，可以进行任何操作，原变量不会被复写
- console

  - console.table()
    ```
    console.table($$('div'),['textContent','className'])
    console.table($$('th div'))
    ```
  - console.assert(boolean,'need to console content') 当需要在特殊情况下打印一些信息时很有用,boolean 为 fasle 时后面的消息将会被输出到控制台，且不会中断代码 但是会在控制台打印错误

    ```
    var value = null
    if(!value){
      console.log('"value" is null')
    }
    ------------------
    console.assert(value,'"value" is null')
    ```

  - console.log 加上样式 给打印的内容加上'%c' 第二个参数就变为 css 规则了

    ```
    console.log('%cMyDefinedConsole','color:#f00;background:#ff0')
    let consoleStleVars = 'MyDefinedConsole'
    console.log(`%c${consoleStleVars}`, 'color:#f00;background:#ff0')
    ```

  - console 同时打印多个对象不好区分可以使用{}
    ```
    let varone = 'name'
    let vartwo = 'age'
    let varthree = 'role'
    console.log({ varone, vartwo, varthree })
    console.table({ varone, vartwo, varthree })
    ```

- 只需按下 "眼睛" 符号，你就可以在那里定义任何 JavaScript 表达式。 它会不断更新，所以表达的结果将永远

* queryObjects() 去查找构造函数下面有多少个实例对象
  ```
    class Person {
    constructor(name, role) {
      this.name = name
      this.role = role
    }
    }
    const john = new Person('Jonh', 'mum')
    let kids = [new Person('Mary', 'daughter'), new Person('jodan', 'son')]
    new Person('Lucius', 'uncle')
    console.log('How many people are do we have?')
  ```
* monitor functions 镜像函数
  monitor 是 DevTools 的一个方法, 它能够让你 spy(潜入) 到任何 "function calls(方法的调用)" 中：每当一个 spied 被潜入 的方法运行的时候，console 控制台 会把它的实例打印出来，包含函数名以及调用它的参数。

  ```
  class Person {
    constructor(name, role) {
      this.name = name
      this.role = role
    }
    greet() {
      return getMessage('greeting')
    }
    getMessage(type) {
      if (type === 'greeting') {
        return `Hello, I'm ${this.name}`
      }
    }
  }
  =============
  console 中去执行
  John = new Person('john')
  mary = new Person('mary')
  monitor(John.getMessage) //对getMessage进行追踪
  John.greet() //可以正常的打印 没有上面的不能正常的被打印内容
  ```

### **截屏 只针对该网页的**

1. F12 打开开发者工具
2. ctrl+shift+p 打开命令行
3. 输入 screen 会出现对应的截屏方式

   - `capture area screenshot` 自己选择区域
   - `capture full size screenshot` 全屏长图截屏
   - `capture screenshoot` 捕捉当前屏幕截图
   - `capture node screenshot` 捕捉节点网页屏幕截图 （????????????不太理解）

## **三 一些常用的快捷键操作**

1. 聚焦 `dev` 后 `esc` 可以打开抽屉式窗格
2. `ctrl + shift + D` 切换 devTools 位置（在当前和之前的位置进行切换）
3. `ctrl + [ / ctrl + ]`分别从当前面板向左和向右切换面板
   或者通过 setting 设置 使用`ctrl + 1...9`切换面板（上面的仍然有效）
4. `alt + click` 快速全部打开 `dom` 各个节点 或者 元素上右键选择 expand recursively

## **四 抽屉式窗格内容**

1. coverage 查看代码冗余
  它用绿色的线条标记运行和用红色的线条标记未运行。

2. changes 展示修改的内容
  如你在页面上修改了什么 css 样式

3. sensors
  设置设备 Location 信息

#### **参考文章地址**

<span>console: https://developer.mozilla.org/zh-CN/docs/Web/API/Console </span><br>
<span>掘金: https://juejin.im/post/5c26f43ae51d4563b12460c1 </span><br>
<span>dev 官网: https://developers.google.com/web/tools/chrome-devtools/?hl=zh-cn </span>

###### 备注

<span id="mark1">[1]: 在 Network 设置单独的网络限制，在性能面板两者都可以设置</span><br>
<span id="mark2">[2]: 限制是相对于笔记本电脑或桌面设备的普通性能而</span><br>
<span id="mark3">[3]: https://developers.google.com/web/tools/chrome-devtools/network/understanding-resource-timing?hl=zh-cn </span><br>
<span id="mark4">[4]:在解析 html 的过程中，html 的解析会被中断，这是因为 javascript 会阻塞 dom 的解析。当解析过程中遇到 script 标签的时候，便会停止解析过程，转而去处理脚本，如果脚本是内联的，浏览器会先去执行这段内联的脚本，如果是外链的，那么先会去加载脚本，然后执行。在处理完脚本之后，浏览器便继续解析 HTML 文档</span>

