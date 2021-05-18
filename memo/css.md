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
