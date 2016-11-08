#html5特性解读
---
>最近闲着没事，手痒看了一本html5的书，为了加强记忆，就准备总结一下html5的新增加的特性.

##结构化元素
* 结构化元素严格地讲是标记给浏览器以及搜索引擎解析用的标签.在html4的编程过程中，包括我在内，都特别喜欢使用`<div>`，很早以前,那时候准备写一些前端，就去请教了weidiao学长，学长告诉我"其实前端很容易，一个div和一个span就够了"，学长的话影响我了很长的时间，我在之后的学习中一直按照这种方法进行布局和代码编写(学长对我的帮助是很大的).后来慢慢的代码写多了，开始有自己的感悟了，自己开始用类header,content,footer等去进行语义化布局.再后来，接触了html5.
* html5的结构化元素(以下几个含义过于简单，就不解释了)

    |#|标签|
    |----|----|
    |1|`<nav>`|
    |2|`<header>`|
    |3|`<article>`|
    |4|`<section>`|
    |5|`<aside>`|
    |6|`<footer>`|
    
##内容标记方法
* html5新增加的内容标记方法，为了有效地处理日趋复杂的图文版本.
* 绘图元素
    
    |#|标签|作用|
    |----|----|----|
    |1|`<canvas>`|参见下文说明.|

    * 说明： 在我原来的blog中有详细的介绍，如果近期有空，我会重新整理一份.
* 分组元素
    
    |#|标签|作用|
    |----|----|----|
    |1|`<figure>`|这个标签可以声明一组包含图片标题、信息的分组.浏览器会把`<figure>`内的所有元素（包括文字和图像）视为一个对象.|
    |2|`<figcaption>`|是`<figure>`的子标签，内部的文字代表图片的标题.
* 文字层级元素
    
    |#|标签|作用|
    |----|----|----|
    |1|`<bdi>`|防止在此标签中内的文字，可以具有独立的方向格式，适合用来标记方向不明的文字.比如拉丁文.|
    |2|`<dialog>`|使用此标签可以在画面中启动文字提示框.在chrome中测试有效，firefox暂时无效.![screenshot from 2016-11-08 16-41-52](https://cloud.githubusercontent.com/assets/16068384/20095424/64a4c57c-a5e0-11e6-9d84-e63374c23b6b.png)|
    |3|`<mark>`|将部分文字加上荧光标记.![screenshot from 2016-11-08 16-41-37](https://cloud.githubusercontent.com/assets/16068384/20095422/64a3122c-a5e0-11e6-97bb-c5cb0643814a.png)|
    |4|`<ruby>与<rt>/<rp>`|`<rt>/<rp>`都是`<ruby>`下的子标签，主要用于排版“注释”和“发音”，可将补充信息排版在主题的上方![screenshot from 2016-11-08 16-43-47](https://cloud.githubusercontent.com/assets/16068384/20095421/64a12714-a5e0-11e6-993b-eeee4d2b123c.png).`<rp>`排版到右方.|
    |5|`<time>`|加注在此标签中的文字信息会被浏览器视为“时间格式”进行解析。若是使用datetime属性，代表在html中为此文字加上时间标志.|
    |6|`<wbr>`|代表可能发生的换行时机.|
* 交互式元素
    * `<details>与<summary>`
        * details与summary是一起使用的标签，配合可以设计出信息隐藏和展开的互动效果.
        * 举例
            
            ```
            <details>  
                <summary>NEU</summary>  
                <p>  
                  CS is the best major.  
                </p>  
            </details>  
            ```
            ![screenshot from 2016-11-08 16-56-07](https://cloud.githubusercontent.com/assets/16068384/20095419/64a017d4-a5e0-11e6-99f5-d348e913bb56.png)
    * `<menu>与<command>`
        * 放在command标签中的文字具有“按钮”功能，配合command的属性可以制定点击事件等.
        * 实例
            
            ```
            <menu>  
                <command onclick="alert(456)">123</command>  
            </menu>  
            ```
            ![screenshot from 2016-11-08 17-00-56](https://cloud.githubusercontent.com/assets/16068384/20095420/64a0800c-a5e0-11e6-8d0e-dddf5b7c4c43.png)

##多媒体应用
* html5增加的多媒体标签，让浏览器在播放影音时不需要安装额外的播放插件.
* `<audio>`
    * 用法
    `<audio controls autoplay loop>not support</audio>`  
    其中controls属性代表用户可用音乐播放器的面板控制音频文件的播放，其他参数按字面意思理解.  
    图片6    
* `<video>`
    * 用法类似audio，增加参数post(预留字母的位置),height/width制定播放器的高度和宽度(单位：像素).
* `<source>`
    * 由于当今浏览器支持的视频格式不同，但是src属性只能指定一个视频格式，所以使用source标签，用来一次指派多种格式.
* `<track>`
    * 在video中加入track，可以为视频加载WebVTT字幕文件，播放器也会增加"cc"字幕选项.
    * kind属性定义文字内容的类型，常用subtitles（对白的翻译）/captions（无声时所补充的信息）,src,srclang,label识别字幕文件的语言，会出现在字幕选择器的选单中,default制定优先加载的字幕文件，用于具有多种语言的字幕时.
* `<embed>`
    * 使用外部的播放程序嵌入网页来播放音频.浏览器会自动选择播放器.
    
##Web应用程序
* `<datalist>`
    * 简单的说，就是为input添加下拉菜单
    * 实例
        
        ```
        <input list="movie">
      <datalist id="movie">
        <option value="123"/>
        <option value="456"/>
        <option value="789"/>
      </datalist>
        ```
   ![screenshot from 2016-11-08 17-31-35](https://cloud.githubusercontent.com/assets/16068384/20095423/64a4626c-a5e0-11e6-8046-4f2bc86b65e7.png)
![screenshot from 2016-11-08 17-31-49](https://cloud.githubusercontent.com/assets/16068384/20095426/6526876a-a5e0-11e6-8292-94acef750fd0.png)
* `<keygen>`
    * 在送出form数据时会产生一组验证秘钥，私钥会保存在客户端，公钥则会传输到服务端，可在需要验证身份时使用.
    * 属性
        * autofocus: 在网页加载时，自动获得focus
        * disabled: 无法被选择
        * form: 指定所属的formID
        * id: 指定对象所属的ID
        * keytype: 指定密钥的算法，有RSA,DSA,EC可选
        * name: 数据字段名称
* `<output>`
    * 使用js输出计算后的结果，感觉用处一般，没遇到过这种场景
* `<meter>`
    * 显示统计表中的直方图
    * 用法
        `<meter value="9" min="0" max="10">9/10</meter>`  
        ![screenshot from 2016-11-08 18-19-32](https://cloud.githubusercontent.com/assets/16068384/20095425/64d87e44-a5e0-11e6-9274-618eea5d5260.png)
    * 属性
        * max/min/value
        * 当直方图无法显示时，会以文字代替
* `<progress>`
    * 和meter类似

    
