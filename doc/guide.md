### 使用说明

attribute 让类拥有统一的处理属性的方式，让属性拥有getter与setter方法，弥补javascript在属性处理上的不足。

attribute 模板不推荐单独混入使用，有属性需求的，请使用 [base](./base.html) 模块。

### 基本使用例子

    var Base = require('base');
    var Tip = Base.extend({
        initializer: function(){
            var self = this;
            var $target = self.get('$target');
            S.log($target.length);
        }
    },{
         ATTRS:{
            $target:{
                value:'body',
                getter:function(v){
                    return $(v);
                }
            }
         }
     });
     
ATTRS 是 Base 用于添加类属性/参数的 object。请看 base 讲解。

类的属性也可以 addAttr() 动态配置，但不推荐直接这么使用。

每个属性的配置项如下：

* value  (String|Number) 属性默认值，注意默认值请不要设置为复杂对象（通过自定义构造器 new 出来的），复杂对象可设置 valueFn 返回
* valueFn (Function) 如果是复杂对象，请使用 valueFn ，很少用，使用 value 就好
* setter (Function) 写属性时的处理函数，传入从 set() 参数得到的属性值和属性名，如果返回非 undefined 则作为新的属性设置值  
* getter (Function) 读属性时的处理函数 
* validator  (Function) 比较少用，写属性时的验证函数，传入从 set() 参数得到的属性值和属性名，返回 false 则不改变该属性值

务必留意，getter、setter 必须有返回值。
