## 在Vue中使用样式

### 使用class样式

1. 数组
```html
<!-- 第一种使用方式，直接传递一个数组，注意： 这里的 class 需要使用  v-bind 做数据绑定 -->
<h1 :class="['thin', 'italic']">这是一个很大很大的H1</h1>
```

2. 数组中使用三元表达式
```html
<h1 :class="['thin', 'italic', flag?'active':'']">这是一个很大很大的H1</h1>
```

3. 数组中嵌套对象
```html
<h1 :class="['thin', 'italic', {'active':flag} ]">这是一个很大很大的H1</h1>
```

4. 直接使用对象
```html
<h1 :class="{red:true, italic:true, active:true, thin:true}">这是一个邪恶的H1</h1>
```

### 使用内联样式

1. 直接在元素上通过 `:style` 的形式，书写样式对象
```html
<h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>
```

2. 将样式对象，定义到 `data` 中，并直接引用到 `:style` 中
 + 在data上定义样式：
```json
data: {
	h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' }
}
```
 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```
<h1 :style="h1StyleObj">这是一个善良的H1</h1>
```

3. 在 `:style` 中通过数组，引用多个 `data` 上的样式对象
 + 在data上定义样式：
```json
data: {
	h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' },
	h1StyleObj2: { fontStyle: 'italic' }
}
```
 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```html
<h1 :style="[h1StyleObj, h1StyleObj2]">这是一个善良的H1</h1>
```





# 总结

1. MVC 和 MVVM 的区别
2. 学习了Vue中最基本代码的结构
3. 插值表达式   v-cloak   v-text   v-html   v-bind（缩写是:）   v-on（缩写是@）   v-model   v-for   v-if     v-show
4. 事件修饰符  ：  .stop   .prevent   .capture   .self  .once
5. el  指定要控制的区域    data 是个对象，指定了控制的区域内要用到的数据    **methods 虽然带个s后缀，但是是个对象**，这里可以自定义了方法
6. 在 VM 实例中，如果要访问 data 上的数据，或者要访问 methods 中的方法， 必须带 this
7. 在 v-for 要会使用 key 属性 （只接受 string / number）
8. **v-model 只能应用于表单元素**
9. 在vue中绑定样式两种方式  v-bind:class   v-bind:style 