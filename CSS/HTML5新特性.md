[toc]

## HTML5 新增的语义化标签

```
1. <header> 头部标签
2. <nav> 导航标签
3. <article> 内容标签
4. <section> 定义文档某个区域
5. <aside> 侧边栏标签
6. <footer> 尾部标签
```

==注意：==

- 这些语义化标准主要是针对==搜索引擎==的
- 这些新标签页面中可以多次使用
- 在IE9中，需要把这些元素转化为块级元素
- 其实，移动端更喜欢使用这些标签
- HTML5还新增了其他标签



## HTML5 新增多媒体标签

HTML5 在不使用插件的情况下，也可以原生的支持视频格式文件的播放，当然，支持的格式时有限的。

### 视频 video

不同浏览器对视频格式支持的情况不同，但是大部分都对mp4兼容，因此尽量使用mp4格式的视频

当前， video 元素支持三种视频格式： MP4, WebM, 和 Ogg:

| 浏览器            | MP4                  | WebM | Ogg  |
| :---------------- | :------------------- | :--- | :--- |
| Internet Explorer | YES                  | NO   | NO   |
| Chrome            | YES                  | YES  | YES  |
| Firefox           | YES                  | YES  | YES  |
| Safari            | YES                  | NO   | NO   |
| Opera             | YES (从 Opera 25 起) | YES  | YES  |

- MP4 = 带有 H.264 视频编码和 AAC 音频编码的 MPEG 4 文件
- WebM = 带有 VP8 视频编码和 Vorbis 音频编码的 WebM 文件
- Ogg = 带有 Theora 视频编码和 Vorbis 音频编码的 Ogg 文件

video 元素支持多个 source 元素. source 元素可以链接不同的视频文件。浏览器将使用第一个可识别的格式

```html
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
您的浏览器不支持Video标签。
</video>
```

### video 常见属性

| 属性     | 值                    | 描述                                                         |
| -------- | --------------------- | ------------------------------------------------------------ |
| autoplay | autoplay              | 视频就绪自动播放（谷歌浏览器需要添加muted来解决自动播放的问题） |
| controls | controls              | 向用户显示播放控件                                           |
| width    | px                    | 设置播放器宽度                                               |
| height   | px                    | 设置播放器高度                                               |
| loop     | loop                  | 播放完是否继续播放此视频，循环播放                           |
| preload  | auto （预加载）/ none | 规定是否预加载视频（如果设置了autoplay 就忽略此属性）        |
| src      | url                   | 视频url地址                                                  |
| poster   | imgurl                | 加载等待的画面图片                                           |
| muted    | muted                 | 静音播放                                                     |

## 音频 audio

目前,  audio 元素支持三种音频格式文件: MP3, Wav, 和 Ogg:

| 浏览器               | MP3  | Wav  | Ogg  |
| :------------------- | :--- | :--- | :--- |
| Internet Explorer 9+ | YES  | NO   | NO   |
| Chrome 6+            | YES  | YES  | YES  |
| Firefox 3.6+         | YES  | YES  | YES  |
| Safari 5+            | YES  | YES  | NO   |
| Opera 10+            | YES  | YES  | YES  |

```
<audio controls>
  <source src="horse.ogg" type="audio/ogg">
  <source src="horse.mp3" type="audio/mpeg">
您的浏览器不支持 audio 元素。
</audio>
```

### audio 常见属性

| 属性     | 值       | 描述                                                 |
| -------- | -------- | ---------------------------------------------------- |
| autoplay | autoplay | 音频就绪后自动播放（谷歌浏览器禁止了自动播放的功能） |
| controls | controls | 显示控件                                             |
| loop     | loop     | 循环播放                                             |
| src      | url      | 音频地址                                             |



## HTML5 新增 input 类型



HTML5 拥有多个新的表单输入类型。这些新特性提供了更好的输入控制和验证。

- color
- date
- datetime
- datetime-local
- email
- month
- number
- range
- search
- tel
- time
- url
- week

<div>
    <form action="">
        <ul>
            <li>邮箱：<input type="email"></li>
            <li>网址：<input type="url" name="" id=""></li>
            <li>日期：<input type="date" name="" id=""></li>
            <li>时间：<input type="time" name="" id=""></li>
            <li>数量：<input type="number" name="" id=""></li>
            <li>电话号码：<input type="tel" name="" id=""></li>
            <li>搜索：<input type="search" name="" id=""></li>
            <li>颜色：<input type="color" name="" id=""></li>
            <li><input type="submit" value="submit"></li>
        </ul>
    </form>
</div>

验证时必须卸载form表单域中



### 新增表单属性

| 属性         | 值        | 说明                                                         |
| ------------ | --------- | ------------------------------------------------------------ |
| required     | required  | 表示表单内容不能为空                                         |
| placeholder  | 提示文本  | 表示提示信息，存在值则不显示（通过为元素修改样式 input::placeholder） |
| autofocus    | autofocus | 自动聚焦属性，页面加载完成后自动聚焦到指定区域               |
| autocomplete | off/on    | 当用户输入时，浏览器会根据之前输入过的值进行显示。默认时打开的。需要放在表单内，同时加上name属性，得成功提交才可以在之后显示。 |
| multiple     | multiple  | 可以多选文件提交                                             |

<div>
    <form action="">
        <ul>
            <li><input type="search" name="sear" placeholder="brokyz" required autocomplete></li>
            <li><input type="file" multiple></li>
            <li><input type="submit" value="submit"></li>
        </ul>
    </form>
</div>

