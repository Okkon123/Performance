1. AOP即面向切面编程，将公共的逻辑抽取出来，复用能力，减少代码开发。
2. 核心是动态代理
	1. JDK动态代理，接口
	2. Cglib动态代理，继承
3. 实现是在Spring初始化Bean时(BeanPostProcessors后置处理)创建代理对象