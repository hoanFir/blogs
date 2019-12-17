🐾 实现平滑过渡的transition

🕘 2019.10.18 由 hoanfirst 编辑

通过使用transition或animations定义动画过程，可以指定时间内和设置关键，css会自动补全中间动画过程中的样式属性的变化。

`css transition`存在的缺陷：只能指定属性的开始和终点以及实现平滑过渡，不能实现更为复杂的效果（使用[css animation](https://github.com/hoanFir/blogs/blob/master/%E8%BF%87%E6%B8%A1%E4%B8%8E%E5%8A%A8%E7%94%BB/animation.md)）。

### transition

实现平滑过渡。

```

transition: property duration timing-function delay;

transition-property: none | all | css propertys;
transition-duration: time;
transition-timing-function: ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(n,n,n,n);
transition-delay: time; 

timing-fcuntion 速度曲线
ease: cubic-bezier(0.25,0.1,0.25,1); 过渡以慢速开始变快然后变慢结束
linear: cubic-bezier(0,0,1,1);  过渡以默认速度从开始持续到结束
ease-in: cubic-bezier(0.42,0,1,1); 过渡以慢速开始加速到默认速度持续到结束
ease-out: cubic-bezier(0,0,0.58,1); 过渡以默认速度开始再以慢速结束
ease-in-out: cubic-bezier(0.42,0,0.58,1); 过渡以慢速开始再到默认速度最后以慢速结束

cubic-bezier(P0, P1, P2, P3);
P1、P2是起点调整区域的x轴位置和y轴位置
P3、P4是终点调整区域的x轴和y轴

```

