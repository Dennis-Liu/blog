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
>* 匹配包含
