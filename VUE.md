### MVC设计模式

![img](https://upload-images.jianshu.io/upload_images/2740074-85540a7b55ba1fad?imageMogr2/auto-orient/strip|imageView2/2/w/601/format/webp)

### MVVM开发模式

![image-20200725143935264](C:\Users\CMS\AppData\Roaming\Typora\typora-user-images\image-20200725143935264.png)

MVVM分为Model、View、ViewModel三者。
Model 代表数据模型，数据和业务逻辑都在Model层中定义；
View 代表UI视图，负责数据的展示；
ViewModel 负责监听 Model 中数据的改变并且控制视图的更新，处理用户交互操作；
Model 和 View 并无直接关联，而是通过 ViewModel 来进行联系的，Model 和 ViewModel 之间有着双向数据绑定的联系。因此当 Model 中的数据改变时会触发 View 层的刷新，View 中由于用户交互操作而改变的数据也会在 Model 中同步。
这种模式实现了 Model 和 View 的**数据自动同步**，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作 dom。

### Vue的响应式原理

当一个Vue实例创建时，vue会遍历data选项的属性，用 Object.defineProperty 将它们转为 getter/setter并且在内部追踪相关依赖，在属性被访问和修改时通知变化。

![img](https://segmentfault.com/img/remote/1460000018225711)

#### get

属性的 getter 函数，如果没有 getter，则为 `undefined`。当**访问该属性**时，会调用此函数。执行时不传入任何参数，但是会传入 `this` 对象（由于继承关系，这里的`this`并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值。


#### set

属性的 setter 函数，如果没有 setter，则为 `undefined`。当**属性值被修改**时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 `this` 对象。

```js
var bValue = 38;
Object.defineProperty(o, "b", {
  // 使用了方法名称缩写（ES2015 特性）
  // 下面两个缩写等价于：
  // get : function() { return bValue; },
  // set : function(newValue) { bValue = newValue; },
  get() { return bValue; },
  set(newValue) { bValue = newValue; },
  enumerable : true,
  configurable : true
});

o.b; // 38
// 对象 o 拥有了属性 b，值为 38
// 现在，除非重新定义 o.b，o.b 的值总是与 bValue 相同
```


### 监控某个属性值的变化

```js
watch: {
   'obj.a': {
      handler (newName, oldName) {
        console.log('obj.a changed')
      }
   }
  }
```

```js
computed: {
    a1 () {
      return this.obj.a
    }
}
```

利用计算属性的特性来实现，当依赖改变时，便会重新计算一个新值。

当有一些数据需要随着另外一些**数据变化**时，建议使用computed。
当有一个通用的响应数据变化的时候，要执行一些**业务逻辑或异步操作**的时候建议使用watcher



###  Vue中给data中的对象属性添加一个新的属性

在Vue实例创建时，obj.b并未声明，因此就没有被Vue转换为响应式的属性，自然就不会触发视图的更新，这时就需要使用Vue的全局api $set()：

```vue
addObjB () {
      // this.obj.b = 'obj.b'  已经添加但不会显示
      this.$set(this.obj, 'b', 'obj.b')
      console.log(this.obj)
    }
```



### delete和Vue.delete删除数组的区别

delete只是被删除的元素变成了 empty/undefined 其他的元素的键值还是不变。[1,empty,3,4]
Vue.delete直接删除了数组 改变了数组的键值。[1,3,4]





### 组件中 data 为什么是函数

如果`data`是一个单纯的对象，JS 里对象是引用关系，这样作用域没有隔离，那么就会造成无论在哪个组件里改变了data，其他组件里的data也会变

组件不管被复用了多少次，组件中的data数据都应该是相互隔离，互不影响的，组件每复用一次，`data`数据就应该被复制一次

组件不管被复用了多少次，组件中的`data`数据都应该是相互隔离，互不影响的

而 new Vue 的实例，是不会被复用的，因此不存在引用对象的问题。



