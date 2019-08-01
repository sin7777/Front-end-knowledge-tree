# requestAnimationFrame

浏览器的刷新频率为60Hz，也就是每16.7ms刷新一次，可以保证时间的准确性，每次进行渲染之前执行一次 requestAnimationFrame 函数

```JS
// 进度条
 var oBtn = document.querySelector( "input" ),
    oBox = document.querySelector( "p" ),
    timer = null, curWidth = 0,
oBtn.onclick = function(){
    cancelAnimationFrame( timer );
    oBox.style.width = '0';
    timer = requestAnimationFrame( function go(){
        curWidth = parseInt( getStyle( oBox, 'width' ) );
        if ( curWidth < 1000 ) {
            oBox.style.width = oBox.offsetWidth + 10 + 'px';
            oBox.innerHTML = parseInt( getStyle( oBox, 'width' ) ) / 10 + '%';
            timer = requestAnimationFrame( go );
        }else {
            cancelAnimationFrame( timer );
        }
    } );
}
```

优点:

* CPU节能：使用setTimeout实现的动画，当页面被隐藏或最小化时，setTimeout 仍然在后台执行动画任务，由于此时页面处于不可见或不可用状态，刷新动画是没有意义的，完全是浪费CPU资源。而requestAnimationFrame则完全不同，当页面处理未激活的状态下，该页面的屏幕刷新任务也会被系统暂停，因此跟着系统步伐走的requestAnimationFrame也会停止渲染，当页面被激活时，动画就从上次停留的地方继续执行，有效节省了CPU开销。

* 函数节流：在高频率事件(resize,scroll等)中，为了防止在一个刷新间隔内发生多次函数执行，使用requestAnimationFrame可保证每个刷新间隔内，函数只被执行一次，这样既能保证流畅性，也能更好的节省函数执行的开销。一个刷新间隔内函数执行多次时没有意义的，因为显示器每16.7ms刷新一次，多次绘制并不会在屏幕上体现出来。
