### 插件插入方式
* $.fn.xxx 以jQuery插件的方式定义

* $.fn.xxx.defaults 定义插件默认属性值

* $.fn.xxx.methods 定义插件方法集合

* $.fn.xxx.parseOptions 定义解析插件配置的方法

* $.fn.xxx.parseData 部分插件才有的解析数据的方法

### 对象
* 属性 在 jQuery.fn.xxx.defaults

* 方法 在 jQuery.fn.xxx.methods, 调用方式为$("selector").xxx('methodName',methodParams)，每个方法两个参数，第一个为jQuery对象（必传），第二个为传入的参数

* 事件 在 jQuery.fn.xxx.defaults
