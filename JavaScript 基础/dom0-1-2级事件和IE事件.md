

## dom0

dom0级事件是element.onclick。

```
//1
<a href="#" id="hash" onclick="fn1();"></a> 

function fn() {
  alert('0');
}


//2
var btn = document.getElementById('hash');

btn.onclick = function() {
  alert('1');
};
btn.onclick = function() {
  alert('2');
};

```

- 事件直接写在标签内进行绑定

- 通过JavaScript获取元素，绑定事件

**注意，绑定同一个元素的 onclick 事件，后面的会覆盖前面的。**


## dom2

dom2级事件是addEventListener、removeEventListener。


## ie

ie事件是attachEvent、detachEvent。
另外，dom2级方法添加事件处理程序的主要好处是可以添加多个事件处理程序。
