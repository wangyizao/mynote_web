# 项目架构

### 三种跨域方案：

- #### CORS跨域：

  服务端设置，前端直接调用。

  ![image-20220305200926997](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305200926997.png)

- #### JSONP跨域：

  前端适配，后台配合。安装jsonp插件，导入jsonp。jsonp是一个js脚本访问，不是XHR访问（不是一个真正的请求）。发送请求后，服务器会响应，信息返回到callback参数（jsonp内封装了一个参数，参数的值是随机生成的。）

  ![image-20220305200926997](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305200926997.png)

- #### 接口代理：

  通过修改nginx服务器配置来实现，前端修改，后台不动。先创建一个vue.config.js

  ```
  module.exports = {
      devServer:{
          host:'localhost',
          port:8080,
          proxy:{
              '/api':{
                  target:'https://coding.imooc.com/'
                  changeOrigin:true,
                  pathRewrite:{//
                  	'/api':''
                  }
              }
          }
      }
  }
  ```

  

### 需求梳理：

- 熟悉文档、查看原型、读懂需求

- 了解前端设计稿-设计前端业务架构

- 了解后台接口文档-制定相关对接规范

- 协调资源并搭建前端架构

  

  

### 目录结构设置：

​		从需求中需要得知：路由需要哪些；确定哪些页面，模块可以复用共同组件。

​		src文件的目录结构

```
src：

​	api--接口文件夹

​	assets--静态图片，小图片。

​	components--组件

​	storage--数据存储的工具箱

​	store--vux

​	util--公共机制：

​	route.js--存放路由

​	app.vue--

​	pages

​		home.vue--分好结构

​		index.vue--首页

​		product.vue--产品站

​		detail.vue--商品详情

​		order.vue--订单

​		orderconfirm.vue--订单确认

​		login.vue--登录页面

​		orderpay.vue--支付

​		alipay.vue--跳转页面
```



### 基础插件使用：

- ​	vue-cookie插件
- ​	vue-lazyload
- ​	element-ui
- ​	node-sass
- ​	sass-loader
- ​	vue-awesome-swiper
- ​	vue-axios/axios

### 路由封装：

#### 	Cookie、IocalStorage的不同

![IocalStorage](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305185405787.png) 

#### 	IocalStorage和sessionStorage的不同

​			1.  IocalStorage：能永久存储信息，且信息存储在浏览器端

 			2. sessionStorage：浏览器关闭，信息也会随之删除。信息存储在内存中

#### 为什么要封装Storage：可以把相同的部分内容放在同一个key中

![image-20220305185841696](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305185841696.png)

#### 	如何封装

```
/* 
Storage封装
*/
const STORAGE_KEY = 'mall';
export default {
  // 存储值
  setItem(key, value, modules_name) {
    if (modules_name) {
      var all = this.getItem(modules_name)
      all[key] = value;
      // 这里递归本方法将modules_name作为key(已有),再将当前添加进去的all放进此module_name中,此时的val就包含了所有值，最后直接添加即可
      this.setItem(modules_name, all)
    } else {
      //只传入两个参数：
      let val = this.getStorage();
      console.log(val);
      val[key] = value;
      window.sessionStorage.setItem(STORAGE_KEY, JSON.stringify(val));
    }
  },
  /*
  获取值(可以传入一个值也可传入两个值)
  一个参数：没有moudle直接单对象
  一个参数：获得moudle下的key对应的对象
  */
  getItem(key, modules_name) {
    if (modules_name) {
      let val = this.getItem(modules_name);
      if (val) return val[key]
    }
    return this.getStorage()[key];
  },
  // 获取整个数据(转为json格式):将数据转换为json格式。
  getStorage() {
    return JSON.parse(window.sessionStorage.getItem(STORAGE_KEY) || '{}')
  },
  // 删除某一个值
  clear(key, modules_name) {
    let val = this.getStorage();
    if (modules_name) {
      delete val[modules_name][key]
    } else {
      delete val[key]
    }
    window.sessionStorage.setItem(STORAGE_KEY, JSON.stringify(val));
  }
}

```



### 接口错误拦截：

#### 	错误类型

- 统一报错

- 未登录统一拦截

- 请求值、返回值统一处理

  前后端域名不一致：

```
const instance = axios.create({
	baseURL:'https://some-domin.com/api/',
	timeout:1000,//操作时间
	headers:{'X-Custom-Header':'foobar'}
})
```

 拦截器interceptor：

![image-20220305192649191](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305192649191.png)



解决前端同时发送多个请求问题：axios.all及Promise.all合并多个请求且都返回数据后进行其他操作。

### 接口环境设置：

#### 	原因

![image-20220305192750928](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305192750928.png)

#### 	环境模块化：env.js(process.env怎么用的？)

![image-20220305193341484](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305193341484.png)

![image-20220305194138748](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305194138748.png)



### mock设置：模拟后端数据，帮助前端开发

#### 	优点

![image-20220305194222058](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305194222058.png)

#### 	方案

- 建本地静态json文件：

  ![image-20220305194613438](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305194613438.png)

- easy-mock：

  ![image-20220305200003328](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305200003328.png)

- 本地集成mock js实现

  ​	1.安装

  ![image-20220305195254020](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305195254020.png)

  ​	2.定义开关

  ​	![image-20220305195535962](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305195535962.png)

  ​	3.在mock下建一个api.js

  ![image-20220305195430760](C:\Users\15118\AppData\Roaming\Typora\typora-user-images\image-20220305195430760.png)