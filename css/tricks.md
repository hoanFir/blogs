
一、border-radius box-shadow

    <div id="plat-to-cooperation">
      <div class="fb-btn-blue">
        <div class="fb-btn-content">我要合作</div>
      </div>
    </div>

    #plat-to-cooperation {
        width: 208px;
        position: relative;
      
        border-radius: 35px;
        box-shadow: 0 4px 0 0 #2aa7e5; /*搭配border-radius实现椭圆阴影效果*/
    }
    
    .fb-btn-blue {
        padding: 10px 28px;
        border-radius: 35px;
        background-color: #59c4f9;
        border-color: #59c4f9; /*解决div白边问题*/
      
        color: #fefefe;
        text-align: center;
        white-space: nowrap;
        vertical-align: middle;
        cursor: pointer;
    }


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190506113635871.png)

二、text-align vertical-align

    <div id="view">
      <div class="title">平台合作</div>  
      <div class="subtitle">
        <div class="line"></div>
        cooperation
        <div class="line"></div>
      </div>
    </div>

    #view {
        position: relative;
        width: 100%;
        text-align: center;
        padding: 85px 0;
        border-top: 1px solid #f3f3f3;
    }
    .title {
        font-family: Arial Regular;
        color: #4a565c;
        font-size: 28px;
    }
    .subtitle {
        font-family: Arial Regular;
        color: #4a565c;
        opacity: .7;
        font-size: 20px;
    }
    .line {
        display: inline-block;
        margin: 0 15px;
        height: 1px;
        width: 66px;
        vertical-align: middle; /*垂直居中*/
        background-color: #cccfd1;
    }

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190506113752284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hvYW5GaXI=,size_16,color_FFFFFF,t_70)

三、white-space background-position

    <span class="error">用户名不合法</span>

    .error {
        white-space: nowrap; /*文字超出不会自动换行，即元素的宽虽然比文字长度小，但文字内容不会换行*/
      
        height: 34px;
        line-height: 34px;
        color: red;
        background-image: url(/public/img/error.png);
        background-repeat: no-repeat;
        padding-left: 20px;
        background-position: 0 11px; /*将图标位置下移，与文字水平*/
    }

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190506113655452.png)

四、text-align right

    <div class="form-group"><label for="name"><i class="star"></i> 姓名：</label></div>
    <div class="form-group"><label for="name"><i class="star"></i> 电子邮箱：</label></div>

    label {
        display: inline-block;
        font-size: 14px;
        font-weight: 500; /*label默认为700*/
        width: 100px;
        text-align: right;
        background: #ddd;
    }
    .star {
        display: inline-block;
        height: 10px;
        width: 10px;
        background-image: url(/public/img/customized_star.png);
        background-repeat: no-repeat;
        margin-right: 3px;
    }

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190506113731354.png)

五、text-shadow

    <span>标题</span>

    span {
        position: absolute;
        white-space: nowrap;
        font-size: 26px;
        transition: all .3s ease-in-out;
        cursor: default;
        color: #80888d;
    }
    span:hover {
      color: #FCFF00;
      text-shadow: 0 0 0.2em #FCFF00, 0 0 0.2em #FCFF00; /* 偏距为0时, 实现一个周围会发光的字母. 如果单一的阴影不够强烈, 那就重复同样的阴影几次 */
    /*   text-shadow: 0 0 0.2em #FCFF00; */
    }

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019050611380257.png)

六、rightbottom flip

    <div class="form-content">
      <p class="title">账户登录</p>
      <form></form>
      <div class="third-login">第三方平台登录</div>
      <hr class="split">
      <div class="rightbottombg"></div>
    </div>

    .form-content {
        position: relative;
        width: 380px;
        border-radius: 4px;
        background: #4e4e4e;
        padding: 32px 36px 70px;
    }
    form {
      width: 308px;
      height: 242px;
      background: #fff;
    }
    .third-login {
      width: 308px;
      height: 17px;
      margin: 20px 0 40px;
    }
    .split {
      margin: 5px 0;
      border: 0;
      border-top: 1px solid #eee;
    }
    .rightbottombg {
        position: absolute;
        right: 0;
        bottom: 0;
        width: 74px;
        height: 55px;
        background: url("/public/right-bottom.png");
    }

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190506114049322.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hvYW5GaXI=,size_16,color_FFFFFF,t_70)
