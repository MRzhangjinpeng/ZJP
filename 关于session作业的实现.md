# 关于session作业的实现

关于三个python文件：

### __ init ___：

为应用app文件夹的主方法，是在它中导入flask的包，并定义一个主题声明函数提供给 manager 进行导入调用，且因为 session 不同于 cookie 的地方，得附加上一个 **app.config['SECRET_key']='110'** 作为session在主方法里的一个配置文件，

主方法的声明就是：定义一个函数，并在其中定义一个实例 = Flask（__ name   __ ），算是主函数的初始化，__ name __ 是系统变量。然后return 到实例中，也就是实例的目的是传递 __ name __ 。

### manager：

为主文件，调用app文件里的views和__ init __。作为主体来运行。

首先先调用 flask-script（提供插入外部脚本） 包的 Manager

再导入 App 文件夹里 __ init __ 里定义的主函数（其中输入 App 就可以 import了）

再导入 App.views 里的blue

 代码主体：

​			再次定义 app 实例 = create_app()  （前面定义的主函数）

​					manager = Manager(app=app)  （声明manager为app实例）

​					app.register_blueprint(blueprint=blue)（进行实例的蓝图注册）

​			if __ name __ ==' __ main __':

​					manager.run（） （中心主体进行运行）

### views：

作为最复杂的视图函数，其中定义了多个不同的路由进行访问，还有蓝图进行跳转。

先定一个蓝图的实例，进行初始化蓝图，其中依然带有 __ name __ 系统变量

blue = blueprint('first'，__ name __)

然后下面进行对蓝图路由的定义。

@blue.route('/') 为蓝图路由的声明

其中的/ /中定义资源路径

如要跳转网页用 render_template 进行模板视图的跳转

**定义第一个蓝图**

作为第一个首页面的加载

**定义第二个蓝图**

作为第一个网页的数据提取，并提交给第二个网页重定向的赋值

如果需要默认不支持的 post 请求，得在资源参数后加上 methods 进行声明

如 methods=['post']

可以使用 request 对网页标签中的值进行获取，

request.form.get('name')  就是获取 from 表单中 name 为 name 的值

session ['name'] = name 用session方法进行name的值的传递

然后 后面可以进行重定向 redirect(url_for('first.index.html'))

类似于 render_template 的方法使用，但是这样的话，会造成一些问题，其中重定向的使用是一些网站进行登录的安全保护的，反向解析还是获取请求资源路径。主要是因为前面的资源获取是一个网站，而后面的传参有是另一个网站的缘故吧。

**然后再定义第三个蓝图**

