1. 添加相关依赖
2. 实现自动配置
	1. @ConfigurationProperties配置自动配置属性
	2. @Configuration标识自动配置类，在其中创建需要的的Bean
3. 创建配置类入口文件
	1. 在你的starter项目的src/main/resources下，
		1. 创建META-INF/spring目录，并且创建一个  org.springframework.boot.autoconfigure.AutoConfiguration.imports文件，内容为自动配置类
		3. 创建META-INF目录，在META-INF目录下创建文件spring.factories，key为org.springframework.boot.autoconfigure.EnableAutoConfiguratio，value为自动配置类绝对路径