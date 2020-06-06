

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

```
//1
<a href="#" id="hash" onclick="fn1();"></a> 

function fn() {
  alert('0');
}


//2
var btn = document.getElementById('hash');

btn.addEventListener('click',function(){
  alert('1')
},false);

btn.addEventListener('click',function(){
  alert('2')
},false);

```

注意，相比dom0事件，绑定同一事件不会相互覆盖，按顺序执行。
