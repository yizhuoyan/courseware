#Model的构建

#一、概述
HandlerAdapter最终需要返回一个ModelAndView，View的获取有各种策略来实现，那么Model如何构建呢?

#二、Model构建方式

##1、直接返回Map/Model/ModelAndView

handler方法直接返回以上类型的值都会作为模型和视图绑定。


~~~
@RequestMapping("/test.do")
public Map dodo(){
	Map map=new HashMap();
	map.put("data","123");
	return  map;
}
~~~
此次请求的视图名称则只能从`RequestToViewNameTranslator`策略获取，默认是`/test.do`==>`test`

##2、方法参数Map/Model/ModelMap

handler方法的参数声明为以上类型也会被作为模型和视图绑定。
常用此种方式来构建模型。

~~~
@RequestMapping("/test.do")
public String dodo(Map){
	map.put("data","123");
	return "ok";
}
~~~

此次请求的视图名称为ok，模型为map

##3、@ModelAttribute注解在方法上
所有注解了@ModelAttribute的方法都会在`@RequestMapping`方法执行前执行。@ModelAttribute的方法用于构建本次请求模型。

~~~
 @ModelAttribute("user")
public User addAccount() {
  return new User("jz","123");
}

@RequestMapping("/test")
public String helloWorld() {
  return "ok";
}
~~~
本次请求的视图名称是ok，模型是user对象。


**注意:如果@RequestMapping和@ModelAttribute同时注解在一个方法上，则方法的返回值就作为模型了。**


##4、@ModelAttribute注解在方法参数上
两个作用

0. 用于获取模型中的值注入给指定参数
如从session中获取等
0. 如果不存在，则新建后放入模型中

~~~
@RequestMapping(value = "/test")
public String dodo(@ModelAttribute("user") User user) {
           return "ok";
}
~~~

此次请求会尝试获取user的数据，如果未找到，则通过参数构建User对象并放入模型，名称为user


##5、方法参数的bean
和使用`@ModelAttribute`注解在参数上类似，只是此时模型的名称默认为类名首字母小写。

~~~java
@RequestMapping(value = "/test")
public String dodo(User user) {
           return "ok";
}
~~~





#总结