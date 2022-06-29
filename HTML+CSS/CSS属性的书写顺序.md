[toc]

## 属性书写顺序

1. 布局定位属性：display / position / float / clear / visibility / overflow
2. 自身属性：width / height / margin / padding / border / background
3. 文本属性：color / foont / text-decoration / text-align / vertical-align / white-space / break-word
4. 其他属性（CSS3）：content / cursor / border-radius / box-shadow / text-shadow /background:linear-gradient…



## 页面布局的整体思路

为了提高网页制作的效率，布局时通常有以下的整体思路：

1. 必须确定页面的版心（可视区），我们测量可得知。
2. 分析页面中的行模块，以及每个行模块中的列模块。其实页面布局第一准则.
3. 一行中的列模块经常浮动布局，先确定每个列的大小之后确定列的位置.页面布局第二准则
4. 制作TML结构。我们还是遵循，先有结构，后有样式的原则。结构永远最重要
5. 所以，先理清楚布局结构，再写代码尤为重要这需要我们多写多积累



## 确定版心

