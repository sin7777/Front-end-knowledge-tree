# 取消a标签默认行为

## javascript:void(0)

```HTML
<a href="javascript:void(0);"  οnclick= "func()">Click Me</a>
```

## href="#"

```HTML
<a href="#">  Click Me   </a>
```

## return false

如果必须写上跳转网站

```HTML
<a href="http://www.baidu.com"  οnclick= "func();return false ">Click Me</a>

<script>
func(){
    //todo
}
</script>
```

## preventDefault()

```HTML
<a href="http://www.baidu.com" id="test">  Click Me  </a>

<script type="text/javascript">
var test = document.getElementById('test');
test.onclick = function(e){
    if (e && e.preventDefault)
        e.preventDefault();
    else
        window.event.returnValue = false;  
}
</script>
```

## 在 React 中

在 React 中另一个不同点是你不能通过返回 false 的方式阻止默认行为。你必须显式的使用 `preventDefault`。

```JS
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }
  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```
