---
title: "Spring Security学习笔记"
date: 2024-04-28 13:17:00
tag: "java"
---

## 通过 security 实现自定义用户认证

1. 密码加密器（PasswordEncoder） 作用：对用户输入的密码进行加密和校验
2. 用户登录服务组件 （UserDetailsService） 作用：根据用户名，获取用户的主题
3. 自定义认证和授权的配置 （SecurityFilterChain） 作用：自定义过滤器链，通过提供的方法来进行过滤器的配置

```java
    // 用户认证
        security.formLogin(configurer ->
            configurer.loginPage() //默认登录页面
        );
    
    // 用户退出
        security.logout(configurer -> configurer.logoutUrl());
```

## 易错点

Spring Security 出现 ‘login.html?error’ is not a valid redirect URL 异常

#### 报错代码

```java
        // 用户认证
        security.formLogin(configurer ->
            configurer.loginPage("users/login")
                    .loginProcessingUrl("/users/login")
                    .failureUrl("/users/error") // 重定向到失败页面
        );
```

### 错误原因

在security的路径配置中，必须在代码前缀加上 "/"

#### 修改后

```java
        // 用户认证
        security.formLogin(configurer ->
            configurer.loginPage("/users/login")
                    .loginProcessingUrl("/users/login")
                    .failureUrl("/users/error") // 重定向到失败页面
        );
```

### 错误分析

security 的登录失败页面是在原重定向的页面拼接上 "?error" 然后对拼写的页面进行检查然后在类
org.springframework.security.web.authentication.SimpleUrlAuthenticationFailureHandler#setDefaultFailureUrl 中进行检查

```java
public void setDefaultFailureUrl(String defaultFailureUrl) {
	Assert.isTrue(UrlUtils.isValidRedirectUrl(defaultFailureUrl),
			() -> "'" + defaultFailureUrl + "' is not a valid redirect URL");
		this.defaultFailureUrl = defaultFailureUrl;
}
```

使用 UrlUtils.isValidRedirectUrl(defaultFailureUrl) 进行判断

```java
public static boolean isValidRedirectUrl(String url) {
	return url != null && (url.startsWith("/") || isAbsoluteUrl(url));
}
```

如果返回 true 就加入后缀 如果返回 false 就抛出异常



