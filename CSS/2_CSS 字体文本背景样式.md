[toc]

## font 字体属性

|    属性     |       表示       |                            注意点                            |
| :---------: | :--------------: | :----------------------------------------------------------: |
|  font-size  |       字号       |                       通常单位是px像素                       |
| font-family |       字体       |      字体之间用逗号隔开，如果字体名字有空格需要用单引号      |
| font-weight |     字体粗细     |    加粗是700或blod，不加粗是normal或400，数字后面不加单位    |
| font-style  |     字体样式     |                 斜体是italic，不倾斜是normal                 |
|    font     | 字体复合属性连写 | 属性连写有顺序style>weight>size/height>family。其中字号和字体必须出现，否则无效。 |



## text 文本属性

| 属性            | 表示     | 注意点                                                   |
| --------------- | -------- | -------------------------------------------------------- |
| color           | 文本颜色 | 通常用十六进制，可以写简写，如#aa00ff可以写成#a0f        |
| test-align      | 文本对齐 | 可以设定文字水平对齐方式                                 |
| text-indent     | 文本缩进 | 通常用于段落缩进两个字的距离 2em                         |
| text-decoration | 文本修饰 | 添加下滑线和取消下划线                                   |
| line-height     | 行高     | 控制行与行之间的距离，其中又上边距，字体大小，下边距组成 |



## background 背景属性

| 属性                  | 作用         | 值                                                 |
| --------------------- | ------------ | -------------------------------------------------- |
| background-color      | 背景颜色     | 预定义的颜色值/十六进制/RGB代码                    |
| background-image      | 背景图片     | url(图片路径)                                      |
| background-repeat     | 是否平铺     | repeat/no-repeat/repeat-x/repeat-y                 |
| background-position   | 背景位置     | length/position 分别是 x 和 y 坐标                 |
| background-attachment | 背景附着     | scroll（背景滚动）/fixed（背景固定）               |
| 背景简写              | 书写更简单   | 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置； |
| 背景色半透明          | 背景颜色透明 | background: rgba(0,0,0,0.3); 后面必须是4个值       |

