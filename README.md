# GazeScroll.js - 滚动显示插件

#### 介绍
可高度自定义的滚动显示插件，该插件由 Ganxiaozhe 开发，使各类多个自定义事件效果随着页面滚动而“生效”。

GazeScroll.js 依赖 gQuery v1.4.7 及以上版本。

website: [https://www.gquery.net/plugins/gazescroll/](https://www.gquery.net/plugins/gazescroll/)


#### 安装教程

你需要引入以下文件：（请勿在开发环境下使用压缩版本，否则将失去错误相关警告!）
```html
<script type="text/javascript" src="gquery.min.js"></script>
<script type="text/javascript" src="gazescroll.gquery.js"></script>
```

#### 使用说明

GazeScroll.js 通过 $.extend 将其方法拓展进 gQuery 方法链，你可以通过传入对象的方式对单个或多个元素进行绑定。
```javascript
$.gazescroll({
    sel:String,
    calc:Function,
    show:Function,
    hide:Function
});
```

- calc 函数在元素在浏览器可视范围内滚动时重复执行，并会传入一个包含各项数据的对象。
- show 函数在元素进入浏览器可视范围内时执行一次。
- hide 函数在元素离开浏览器可视范围内时执行一次。

```javascript
calcData => {
    h:Number,   // 该元素高度
    pos:Object,   // 该元素左上角相对文档的{top, left}，以及右下角的{bottom}
    dist:Object,   // 该元素左上角相对浏览器的{top, left}
    pp:Number,   // 该元素距离滚出可视范围的百分比，会根据滚动方向调整
    rpp:Number,   // 该元素距离滚出可视范围的百分比，不会根据滚动方向调整
}
```

对于已经绑定的元素，可用 $.removeGazescroll([sel:String]) 进行移除。不进行穿参调用时，将移除全部绑定事件。

#### 示例

```javascript
$.gazescroll({
    sel: '.gl-progress-bar',
    calc: function(data){
        let $obj = $(this);
        let $prog = $obj.find('.gl-progress-inner'), pr = {
            type: $obj.attr('data-type'), rpp: data.rpp*100+'%', pp: data.pp*100+'%'
        };

        switch(pr.type){
            case 'rpp':$prog.width(pr.rpp);break;
            case 'pp':$prog.width(pr.pp);break;
        }
    }
});
```

```javascript
$.gazescroll({
    sel: '#ex-words',
    calc: function(data){
        let $obj = $(this), pp = Math.floor(data.pp*10), words = '隐私，安全可控'.split('');
        pp>7 && (pp=7);
        for(let i = 0; i<=6-pp; i++){
            words[i] = '*';
        }

        $obj.text(words.join(''));
    }
});

$.gazescroll({
    sel: '#ex-words-re',
    calc: function(data){
        let $obj = $(this), pp = Math.floor(data.pp*10), words = '隐私，安全可控'.split('');
        pp>7 && (pp=7);
        for(let i = 6; i>=pp; i--){
            words[i] = '*';
        }

        $obj.text(words.join(''));
    }
});
```
