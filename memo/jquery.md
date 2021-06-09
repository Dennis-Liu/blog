## select根据value值 取得text值

>var value= data.value;
>opts= $("#provid").find("option[value="+ value +"]").text();


## 根据option的text值获取选中的value值  
使用jQuery根据option中的text，获取select下拉框中的value值，很多人都是直接使用
>$('#selectId option[text="test"]').val();  

但是，在某些情况下，这种方式并不能获得想要的结果，在这里提供一种其他的方法
>$('#selectId option:contains(“test”)').val();

## JS控制select2选定某个值
>$('#position').select2().val($('#position').find('option:contains("CMS")').val()).trigger('change');

## 模糊定位
>^ 匹配开头
```javascript
div[class^='col']
div[id^='col\\]']
```

>$ 匹配结尾  
>\* 匹配包含

## 可见性过滤器
>div:hidden 匹配使用属性display:none隐藏的元素   
>div::visible 匹配所有可见元素

## 高级选择器
>children():匹配所有的子代元素  
>find():匹配所有的后代元素，必须传参  

>siblings():匹配所有的兄弟（上下兄弟）  
>nextAll():匹配下面所有的兄弟  
>prevAll():匹配上面所有的兄弟  
>next():匹配下一个相邻兄弟  
>prev():匹配上一个相邻兄弟  

>first():匹配第一元素  
>last():匹配最后一个元素  
>eq(2):匹配下标为2的元素  

## html()/text()
>html() 识别标签  
>text() 不识别标签  

## parent()/parents()/closest()
>parent():无参数，默认匹配子元素的直接父级   
>parents():可以传参  
* 不传参：默认匹配子元素的直接父级  
* 传参：匹配指定父级  
     		 
>closest()必须传参，匹配指定父级

```javascript
$("li:eq(1)") === $("li").eq(1)
$("li:last") === $('li').last()
$("li:first") === $('li').first()//只选中第一组第一个
$("li:first-child") 每一组的一个li
```



