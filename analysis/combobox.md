@ combobox 源码组织
# 一、功能需求
我们需要一个组件式的开发，一个简易的调用，即可魔法出一个完善的功能组件。

easyui 提供的combobox 是一个下拉组件，由可编辑的文本框和下拉列表组成。

实际中它涵盖了理想的下拉框功能 —— 多选、单选等

# 二、插入jQuery中
combobox 是独立放入jQuery 对象中的，并捆绑在DOM 元素对象的方法中，这样可以直接使用jQuery('selector').combobox() 调用
```js
// 自古套路是jser
(function($){
	// 全局 作用域
	$.fn.combobox = function(options, param){
        ...
    };
})(jQuery)
```

# 三、作用域的规划
3.1 作用域的规划
整体来看，这个插件进行了如下作用域划分

* 全局
  * 公用方法
    * 被封装在内部，仅供内部使用的公用方法
    * 传入event 事件的方法
    * 传入target 元素以及配置数据的方法
    * 通过$.fn 暴露的公用方法
  * 公用对象
    * 被封装在内部，仅供内部使用的公用对象
    * 通过$.fn 暴露的公用对象
  * 调用入口【暴露出去的方法】
* 私有
  * 私有方法
    * $.fn.combobox.parseData 中有一个_parseItem；
以上区分有几个关键点

1、通过$.fn 暴露的公用方法可被其他组件轻易调用【可复用】
2、为了避免将所有可服用的方法相互污染，最通用的手段是放入一个对象中
3、被封装在内部，仅供内部使用的公用对象是一个定时炸弹，因为插件式封装，和 new 一个方法不同，没有自己独立的作用域。所以你可能需要使用一个自加计算的游标，来保证你的实例的唯一。

3.2 插件式注意点
* jQuery插件如何保证实例的唯一
* jQuery插件如何将组件基础的方法暴露出来【供上层、同层建筑使用】

# 四、需求设计与功能设计
网络上没有设计图，所以我写一些重点吧

4.1 功能列表
请参考 $.fn.combobox.defaults 和 $.fn.combo.defaults

4.2 环境
* jQuery 选择器对象

4.3 核心设计
4.3.1 在元素上绑定数据
easyui + jQuery 通用的作风是，在元素对象上绑定data。

以下是使用做法和风格。

from ：http://api.jquery.com/jquery.data/

// selector 选择的元素 === element

// key 字符串，寻址作用，键名

// value *，存储的数据，值，可为空

jQuery(selector).data( key, value )

jQuery(selector).data( key )

jQuery.data(element, key, value)

jQuery.data(element, key)

# 五、组件调用
5.1 调用暴露函数
combobox 有两个参数

// options 字符串、为combobox 内部方法

// param 对象、使用方法的参数

// 注意：combobox 对combo 存在依赖，并且，优先使用上层建筑的方法
```js
$.fn.combobox = function(options, param){
  if (typeof options == 'string'){
    var method = $.fn.combobox.methods[options];
      if (method){
        return method(this, param);
      } else {
        return this.combo(options, param);
      }
  }
  ...
}
```
