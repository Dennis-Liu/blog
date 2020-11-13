### 取元素top10
一次项目bug修复过程，因为用到了一些不常用的function，所以记录一下以备后用
#### 背景
>一个app随着数据的增加，在选择列表页面出现卡顿/闪退现象。
#### 分析
>因为元数据涉及到第三方视图，所以没有做后台top10分页/分步加载处理，所以在前台页面渲染并显示10000条数据时会卡住
#### 方案
>数据还是保持一次性传输到前台页面，只是默认显示前10条，通过jquery实现模糊查询，查询结果同样只显示前10条
#### 遇到的问题
>通过```$("#projectBody li")```可以获取到所有的元素，但是怎么取前10条
#### 尝试1 
>因为项目使用的freemaker模板，所以尝试在for循环加载li时只显示top10，但是模糊查询后的数据处理已经脱离freemaker的控制范围，所以放弃
#### 尝试2
>在页面循环加载li的时候，给第10个li赋一个唯一的className，通过这个prevAll()获取并前面的li  
```JavaScript
$("#projectBody li").hide().filter(".topend").prevAll().show();
```
>此时页面第一次加载后可以完成top10的显示，但是经过模糊查询过后的li显示有问题，因为这里对prevAll()的理解有误，prevAll()只是选中DOM结构上的前面的元素，而不是基于jquery filter后的结果集数组中的前面的元素，所以放弃
#### 尝试3
>根据之前对jquery的理解，通过css选择器选择的元素是一个数组
```JavaScript
$('#sidebar_categories li')
```
```
n.fn.init(11) [li, li, li, li, li, li, li, li, li, li, li, prevObject: n.fn.init(1), context: document, selector: "#sidebar_categories li"]
```
>如果取出数组中的第一个值，拿到的其实是DOM对象，而非jquery对象，这是直接调用jquery的function会报错
```JavaScript
$('#sidebar_categories li')[0]
```
```
<li style="display: list-item;"></li>
```
```JavaScript
$('#sidebar_categories li')[0].show()
```
```
VM1376:1 Uncaught TypeError: $(...)[0].show is not a function
```
>需要用$()包起来才会变回jquery对象
```JavaScript
$($('#sidebar_categories li')[0]).show()
```
```
n.fn.init [li, context: li]
```
>所以当时考虑通过slice获取数组top10后再转成jquery，但是没想到jquery直接提供了slice function，省去了转换的步骤，最终代码
```JavaScript
$("#projectBody li").hide().filter(":contains('" + $(this).val() + "')").slice(0,10).show();
```
