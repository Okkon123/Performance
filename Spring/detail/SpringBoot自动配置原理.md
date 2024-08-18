1. 它会根据定义在classpath下的类，自动的给你生成一些Bean，并加载到Spring的Context中。
2. 核心是`EnableAutoConfiguration`注解，注解会引导Spring Boot根据项目中的依赖和配置，自动配置Spring应用程序
3. SpringBootApplication -> EnableAutoConfiguration ->EnableAutoConfigurationImportSelector -> Configuration