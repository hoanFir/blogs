🐾 并排div一固定一自适应

🕘 2019.10.28 由 hoanfirst 编辑

方式一：

设置父div(flex-container)：display: flex;

设置左div(flex-item): flex-basis: 100px; flex-shrink: 0; //缩小时不缩小

设置右div(flex-item): flex-grow: 1; //等分剩余空间

方式二：

