





## 1、thymeleaf模板引擎

用这个引擎或者其他引擎都是一样，由动态页面渲染成静态页面。这个过程具体细节由不同标签决定，但是对于大概流程基本上都是一样的。

好处：现在为止只知道可以通过配置进行基本路径访问名的修改，修改后如果在页面通过th:href = "{/....}"访问资源的话，不需要像jsp那样在头中加base等引入标签。

Tips：开发的时候需要禁止模板引擎的缓存功能，不然修改了都不显示结果。

## 2、LocaleResolver

LocaleResolver接口是用于实现一个国际化解析器的接口，其一个主要的方法：

```java
		@Bean
		@ConditionalOnMissingBean
		@ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
		public LocaleResolver localeResolver() {
			if (this.mvcProperties
					.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
				return new FixedLocaleResolver(this.mvcProperties.getLocale());
			}
			AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
			localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
			return localeResolver;
		}
```

用于通过request获取的信息返回一个自定义的Locale对象（封装了国家代码和语言代码）。



## 3、RestFul风格

演示图：

![1545911415615](1545911371(1).jpg)

以后就用这种风格了，写代码让人耳目一新。

| 实验功能     | 请求方式 | 请求URI  |
| ------------ | -------- | -------- |
| 查询所有员工 | get      | emps     |
| 查询单个员工 | get      | emp/{id} |
| 删除员工信息 | delete   | emp/{id} |
| 来到添加页面 | get      | emp      |
| 添加某个员工 | post     | emp      |
| 修改员工信息 | put      | emp/{id} |



## 4、防止表单重复提交

可以在客户端着手解决，也可以在服务端着手解决，客户端的话主要是通过js或者脚本隐藏掉提交按钮，或者把提交按钮设置为不可灰色不可提交状态，服务端解决办法挺多的，但是最好的还是如果可以重定向的话就重定向，如果实在不行就在session中添加token字段或者数据库设置主键（如果是插入数据的话）。



## 5、防止表单数据丢失

在开发过程中，经常会出现表单出错而返回页面的时候填写的信息全部丢失的情况，为了支持页面回跳，可以通过以下两种方法实现。

1．使用header头设置缓存控制头Cache-control。

header('Cache-control: private, must-revalidate'); //支持页面回跳

2．使用session_cache_limiter方法。

session_cache_limiter('private, must-revalidate'); //要写在session_start方法之前

下面的代码片断可以防止用户填写表单的时候，单击“提交”按钮返回时，刚刚在表单上填写的内容不会被清除：

session_cache_limiter('nocache');

session_cache_limiter('private');

session_cache_limiter('public');

session_start();



## 6、拦截器实现登录校验

springboot的拦截器实现登录校验很简单，因为springboot已经为我们设置好了静态资源访问，所以我们配置拦截器访问路径的时候就可以不加上不拦截静态资源的路径了，而只是需要加上拦截路径。



## 7、前端公共代码抽取



