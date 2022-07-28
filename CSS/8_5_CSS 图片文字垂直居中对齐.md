[toc]

## vertical-align

CSS的`vertical-align`属性的使用场景：经常设置图片或者表单（行内块元素）和文字垂直对齐。

只针对==行内元素或行内块元素起效==

语法：

```css
vertical-align: baseline | top | middle | bottom
```

| 值       | 描述                                 |
| -------- | ------------------------------------ |
| baseline | 默认。元素放置在父元素的基线上。     |
| top      | 把元素顶端与行中最高元素的顶端对齐   |
| middle   | 把元素放置在父元素的中部             |
| bottom   | 把元素的底端与行中最低元素的底端对齐 |

<div>
    <textarea cols="30" rows="5"></textarea>默认基线对齐
</div>
<div>
    <textarea cols="30" rows="5" style="vertical-align:top;"></textarea>顶端对齐
</div>
<div>
    <textarea cols="30" rows="5" style="vertical-align:bottom;"></textarea>底端
</div>
<div>
    <textarea cols="30" rows="5" style="vertical-align:middle;"></textarea>垂直对齐
</div>
## 图片底测空白缝隙

看上面第一个文本域与第二个文本域存在空白缝隙，这是因为行内块元素和文字的基线对齐

解决：

1. 给行内块元素加上 <span style="color:red">vertical-align</span>即可
2. 将行内块（图片）转换为块级元素<span style="color:red">display: block;</span>                                                                                                                                        hai 





