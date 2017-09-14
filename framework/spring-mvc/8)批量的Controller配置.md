#批量的Controller配置

#一、概述
多数Controller都需要配置如异常处理，参数自定义转换等需求，SpringMVC提供了`@ControllerAdvice`来进行批量或全局的Controller配置。

被`@ControllerAdvice`注解的类不会作为`Controller`，但其中定义的某些方法会被执行，作为整体配置。

#二、@ControllerAdvice选择Controller

可通过Controller的类名或类上的注解来选择。如:

~~~
@ControllerAdvice(annotations=RequestMapping.class,
				  basePackages={"com.handler1","com.handler2"})
public class ControllerAdvice{

}
~~~
此Advice仅选择com.handler1或com.handler2包下的且类上具有`@RequestMapping`注解的Controller

**若注解中无任何值，则选择所有Controller**



#三、全局WebDataBinder配置

~~~java
@ControllerAdvice
public class MyAdvice{
@InitBinder
public void binder(WebDataBinder binder){
		//进行配置
}
~~~

#四、全局异常处理配置

~~~java
@ControllerAdvice
public class JsonpAdvice{
	@ExceptionHandler
	public String handlerGlobalException(Throwable e){
		//记录日志之类。。。
		return "error";
	}
}
~~~

#总结
