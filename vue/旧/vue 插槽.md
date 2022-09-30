# Vue 插槽

## 概述

在组件中，有些部分的内容是不确定的，这样就可以在使用组件的时候在组件中自定要想要的内容，这就是插槽。

也就是说插槽是定义在组件中的，它允许使用组件的人像插槽中自定义一些内容。

## 使用

在组件中，插槽通过 `slot` 标签进行声明。

组件 MySlot：

```vue
<template>
	<p>下面是插槽</p>
	<!-- 如果插槽中有后背内容时，如果插槽没有被使用则会输出后背内容，如果无后备内容，插槽不被使用时会无视插槽 -->
	<slot>后备内容</slot>
	<p>上面是插槽</p>
</template>
```

使用组件 MySlot：

```vue
<MySlot>
    <!-- 如果组件内没有定义插槽，那么下面的内容无效，将会被丢弃 -->
	<p>我是插槽</p>
</MySlot>
```

## 具名插槽

如果在组件中需要用到多个插槽时，则需要给每个插槽指定 name 名称。这种带有具体名称的插槽叫做具名插槽。

当只有一个插槽时，不需要指定名称，因为其会有一个默认值名称 default。不需要我们指定，可以省略。

```vue
<template>
	<p>下面是插槽</p>
	<slot name="slot01"></slot>
	<slot name="slot02"></slot>
	<p>上面是插槽</p>
</template>
```

**使用具名插槽**时，需要将自定义内容外包裹 `tamplate` 标签然后使用 `v-slot` 属性指定要填充的插槽。

`v-slot` 可以被 `#` 代替。

```vue
<MySlot>
	<tamplate v-slot: slot01>
    	<p>插槽1</p>
    </tamplate>
    
   <tamplate #slot02>
    	<p>插槽2</p>
    </tamplate>
</MySlot>
```

向插槽使用者传递值：

```vue
<template>
	<p>下面是插槽</p>
	<slot name="slot01" user="root", id="777"></slot>
	<slot name="slot02" :obj="{name:'brokyz',age:21"></slot>
	<p>上面是插槽</p>
</template>


<MySlot>
	<tamplate v-slot: slot01="scope">
    	<p>插槽1</p>
        <p>
            {{scope}}
        </p>
    </tamplate>
    
   <tamplate #slot02="{obj}">
    	<p>插槽2</p>
       <p>
           {{obj.age}}
       </p>
    </tamplate>
</MySlot>
```

