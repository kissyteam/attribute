### get(name)

获取名为name的属性值。

#### 使用例子

[demo](https://jsfiddle.net/minghe36/7sjze0jq/)

    demo.get('name');
    
配置的属性如下：

    {
        ATTRS:{
            name: {
                value: '明河'
            }
        }
    }

配置 getter 后，get() 会取 getter 函数的返回值：

[demo](https://jsfiddle.net/minghe36/7sjze0jq/)

    {
        ATTRS:{
            msg:{
                value:'',
                getter: function(v){
                    var name = this.get('name');
                    return name + '很2';
                }
            }
        }
    } 

getter函数的上下文 this 指向类的实例，所以可以使用 this.get('name') 获取类的属性。

获取 msg 属性：

    demo.get('msg')
    
   
### set(name, value, opts)

设置名为 name 的属性值为 value。

opts 参数一般不用配置，有一些特殊的场景可能会用到：

* silent {boolean} - 默认 false , 是否触发 change 系列事件
* error {Function} - 验证失败的回调，包括失败原因
* force {boolean} - 是否强制触发 change 事件，默认值为 false，当值发生变化时才触发

#### 使用例子

[demo](https://jsfiddle.net/minghe36/pz8bgozu/)

    var Demo = Base.extend({
        initializer: function(){
    
        }
    },{
        ATTRS:{
            name: {
                value: '明河',
                setter: function(v){
                    $('body').append('<p>name:'+v+'</p>');
                    return v;
                }
            }
        }
    });
    
设置属性：

    var demo = new Demo();
    demo.set('name','剑平');
    
set() 会触发 属性的 setter 方法。

留意，当设置的值与旧的值相同时，不会触发 setter ，比如再次设置：

    demo.set('name','剑平');

是没有执行 setter 的，想要触发需要使用 force 配置：

    demo.set('name','剑平',{force:true});

### reset(name,opts)

重置属性 name 为初始值. (调用一次 set() ) 。

    demo.reset('name');

### hasAttr(name)

判断是否有名为 name 的属性。

    demo.hasAttr('name');

### removeAttr(name)
移除名为 name 的属性，很少用。

    demo.removeAttr('name');

### addAttr(name, attrConfig )

给宿主对象增加一个属性。不推荐采用这种方式给宿主增加属性。

### addAttrs()

批量添加属性。不推荐采用这种方式给宿主增加属性。

### beforeAttrNameChange(ev)

attribute 模块强大之处是会给所有的属性注入 change 事件，方便监听每个属性值的改变。

属性改变事件，名为 “attrName” 的属性, 在改变它的值之前触发该事件。

[demo](https://jsfiddle.net/minghe36/c7g2n2qn/)

    var demo = new Demo();
    demo.on('beforeNameChange',function(ev){
        $('body').append('<h2>beforeNameChange:</h2>');
        $('body').append('<p>ev.newVal:'+ev.newVal+'</p>');
        $('body').append('<p>ev.prevVal:'+ev.prevVal+'</p>');
        $('body').append('<p>ev.attrName:'+ev.attrName+'</p>');
    });
    
    demo.set('name','剑平');

ev 对象：

* newVal 将要改变到的属性值
* prevVal 当前的属性值
* attrName 当前的属性名

### afterAttrNameChange(ev)

属性改变事件，名为 “attrName” 的属性, 在改变它的值之后触发该事件。

[demo](https://jsfiddle.net/minghe36/c7g2n2qn/)

    var demo = new Demo();
    demo.on('afterNameChange',function(ev){
        $('body').append('<h2>afterNameChange:</h2>');
        $('body').append('<p>ev.newVal:'+ev.newVal+'</p>');
        $('body').append('<p>ev.prevVal:'+ev.prevVal+'</p>');
        $('body').append('<p>ev.attrName:'+ev.attrName+'</p>');
    });

