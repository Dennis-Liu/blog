### Truncate text
```html
<p class="truncate-text">If I exceed one line's width, I will be truncated.</p>
```

```css
.truncate-text {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  width: 200px;
}
```
>If I exceed one line's width, ...


### CSS的@media规则

同一个CSS文件中，也可以根据不同的屏幕分辨率，选择应用不同的CSS规则。  
```css
@media screen and (max-device-width: 400px) {
  .column {
    float: none;
    width:auto;
  }
 
  #sidebar {
    display:none;
  }
}
```

### css隐藏元素方式
>display:none; 隐藏元素，不占位置  
>visibility:hidden；隐藏元素，占位置

### css透明度属性
>opacity:用于设置透明度
*	取值范围0-1
*	值越小，越透明
	
#### 为了兼容低版本ie，需要添加如下代码
>opacity:0.5;  
>filter:alpha(opacity=50);(取值范围：0-100)

