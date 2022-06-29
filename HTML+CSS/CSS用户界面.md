[toc]

## 鼠标样式 cursor

可以给元素添加`cursor`属性来控制经过其的鼠标样式。

```
{ cursor: pointer; }
```

| 属性        | 描述       |
| ----------- | ---------- |
| default     | 小白 默认  |
| pointer     | 小手       |
| move        | 移动十字架 |
| text        | 文本       |
| not-allowed | 禁止       |

```
<ul>
        <li style="cursor: default;">我是默认的小白鼠标样式</li>
        <li style="cursor: pointer;">我是小手鼠标样式</li>
        <li style="cursor: move;">我是鼠标移动鼠标样式</li>
        <li style="cursor: text;">我是鼠标文本样式</li>
        <li style="cursor: not-allowed;">我是鼠标禁止样式</li>
</ul>
```

<div><ul>
        <li style="cursor: default;">我是默认的小白鼠标样式</li>
        <li style="cursor: pointer;">我是小手鼠标样式</li>
        <li style="cursor: move;">我是鼠标移动鼠标样式</li>
        <li style="cursor: text;">我是鼠标文本样式</li>
        <li style="cursor: not-allowed;">我是鼠标禁止样式</li>
</ul></div>



## 表单的轮廓线 outline

给表单添加 `outline: 0;`或者`outline: none;`样式之后就可以去掉默认的蓝色边框。

```css
input {
            outline: none;
        }
```

未去除轮廓

<div><input type="text"></div>

去除轮廓

<div><input type="text" style="outline:none;"></div>



### 文本域拖动缩放 resize

```
textarea {
            resize: none;
            outline: none;
        }
```

未去除

<textarea name="" id="" cols="30" rows="10"></textarea>

去除后

<textarea name="" id="" cols="30" rows="10" style="outline:none;resize:none;"></textarea>

注意：文本域如果`<textarea></textarea>`不再同一行，那么显示出的文本框中会有自动的类似padding效果挤开里面的文字

<div>
    <textarea name="" id="" cols="30" rows="10" style="outline:none;resize:none;">

    </textarea>

</div>