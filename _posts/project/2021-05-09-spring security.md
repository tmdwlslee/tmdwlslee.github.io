---
title: "스프링 시큐리티"
layout: post
date: '2021-05-09 16:41:00 +0900'
categories: project
tags: githubpage
comments: true
---

- 목차
    - [1. 스프링 시큐리티](#1-스프링-시큐리티)
    - [2. 구현 내용](#2-구현-내용)
    - [3. 실행 결과](#3-실행-결과)
<br>
<br>
<br>

## 1. 스프링 시큐리티
---
스프링 시큐리티는 스프링 기반의 애플리케이션 보안(인증, 인가 등)을 담당하는 스프링 하위 프레임워크<br>
toy 프로젝트에서 시큐리티를 사용하여 OAuth2를 접목 시키기위해서 사용 중<br>

 - 인증 : 사용자가 누구인지 확인하는 절차(로그인)
 - 인가 : 사용자가 요청(Request)을 보냈을 때 권한이 있는지 확인하는 절차

 
<br>
<br>
<br>

## 2. 구현 내용
---
OAuth2를 커스터 마이징 하기 전 시큐리티 기본 동작을 이해하기 위해서 작성한 코드<br>

```java

@RequiredArgsConstructor
@EnableWebSecurity // 1
public class WebSecurityConfig extends WebSecurityConfigurerAdapter { // 2

    //customer 서비스 선언
    OAuth2LoginService oAuth2LoginService;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests() // 3
                    .antMatchers("/") // 4
                    .permitAll() // 5
                    .anyRequest() // 6
                    .authenticated() // 7
                .and()
                    .formLogin() // 8
                    .loginPage("/register") // 9
                    .permitAll(); 
    }
}
```
<br>

1. @EnableWebSecurity : 선언 시 해당 클래스를 통한 웹 보안 설정을 활성<br>
2. WebSecurityConfigurerAdapter : 인증, 인가 등을 지원하는 메서드를 가진 클래스<br>
3. .authorizeRequests() : HttpRequest 요청에 대해서 권한 설정<br>
4. .antMatchers("url") : 해당 url로 요청이 들어온 것이 있다면<br>
5. .permitAll() : 모든 권한 부여<br>
6. .anyRequest() : 다른 요청에 대해서는 <br>
7. .authenticated() : 인증 절차를 진행해야함<br>
8. .formLogin() : 로그인 절차를 진행<br>
9. .loginPage("url") : 로그인 페이지 url로 리다이렉트<br> 


<br>
<br>
<br>

## 3. 실행 결과

루트 경로("/")로 접속 후 "hello" 클릭 시 -> **"/hello"** 호출<br>

![ex_screenshot](/assets/img/security1.PNG)<br>

<br>

**"/hello"**는 인가되지 않은 요청임으로 **"/register"**로 리다이렉트<br>

![ex_screenshot](/assets/img/security2.PNG)<br>
