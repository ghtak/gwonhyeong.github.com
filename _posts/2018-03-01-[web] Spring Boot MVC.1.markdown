---
layout:     post
title:      "[web] Spring Boot MVC"
subtitle:   "Boot MVC"
date:       2018-03-01 00:00:00
author:     "gwonhyeong"
header-img: "img/home-bg.jpg"
---

Eclipse Spring Boot MVC 설정 

프로젝트 설정및 DB 연동까지 작업 내용 기록


<h2 class="section-heading">프로젝트 생성</h2>

Eclipse STS 로 기본적인 Web , JDBC , MyBatis 등을 설정하여

Spring Start Project 생성

{% highlight java %}

@SpringBootApplication
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
public class SpringbootmvcApplication {
	public static void main(String[] args) {
		SpringApplication.run(SpringbootmvcApplication.class, args);
	}
}

{% endhighlight %}

위와 같이 설정하여 우선 DB 비활성화

<h2 class="section-heading">JSP 확인</h2>

Pom.xml 에 다음 의존성을 추가

{% highlight java %}

<dependency>
  <groupId>org.apache.tomcat.embed</groupId>
  <artifactId>tomcat-embed-jasper</artifactId>
  <scope>provided</scope>
</dependency>

<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>jstl</artifactId>
</dependency>

{% endhighlight %}

테스트용 컨트롤러 생성

{% highlight java %}

package org.codex;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {

	private final Log log = LogFactory.getLog(getClass());
	
	@RequestMapping( "/hello")
	public String hello() {
		this.log.error("Hello");
		return "hello";
	}
}


{% endhighlight %}

application.properties 설정

{% highlight xml %}

spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp

{% endhighlight%}

WEB-INF/views 디렉터리를 src/main/webapp 에 추가 및

테스트용 hello.jsp 생성

<a href="#">
    <img src="{{ site.baseurl }}/img/2018_03_01/dir.png" alt="Directory Structure">
</a>


http://localhost:8080/hello 에 접속하여 동작 확인

<a href="#">
    <img src="{{ site.baseurl }}/img/2018_03_01/hello.png" alt="">
</a>


UTF-8 필터 설정
{% highlight java %}

package org.codex;

import java.nio.charset.Charset;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.converter.HttpMessageConverter;
import org.springframework.http.converter.StringHttpMessageConverter;
import org.springframework.web.filter.CharacterEncodingFilter;
import javax.servlet.Filter;

@Configuration
public class AppConfiguration {
    @Bean
    public Filter characterEncodingFilter() {
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();
        characterEncodingFilter.setEncoding("UTF-8");
        characterEncodingFilter.setForceEncoding(true);
        return characterEncodingFilter;
    }
}

{% endhighlight %}

<h2 class="section-heading">DB 생성</h2>

Docker 를 이용하여 테스트용 DB 생성

{% highlight xml %}

docker-compose.yml

version: '2.1'

services:
  database:
    image: postgres:10
    volumes:
      - ./data/postgres:/var/lib/postgresql/data
    ports:
      - 5432:5432
    environment:
      - POSTGRES_USER=test
      - POSTGRES_PASSWORD=test_pw
      - POSTGRES_INITDB_ARGS=--encoding=UTF-8

docker-compose up -d 로 수행

{% endhighlight %}

외부 접속 허용

{% highlight java %}

pg_hba.conf 
host all all 0.0.0.0/0 trust

postgresql.conf
listen_addresses='*'

{% endhighlight %}

권한 처리

{% highlight java %}

docker exec -it CONTAINER_NAME psql -U postgres

alter role test CREATEDB;

{% endhighlight %}


DB 생성 및 테이블 생성

{% highlight xml %}

pgAdmin 에서 수행

test_spring_mvc db 생성

create table BOARD(
	board_id bigserial primary key , 
	board_title varchar(60) NOT NULL , 
	board_content text NOT NULL , 
	board_add_date timestamp default NULL 
);

{% endhighlight %}

<h2 class="section-heading">DB 연동</h2>

{% highlight xml %}

  postgresql dependency

  <dependency>
    <groupId>postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>9.1-901-1.jdbc4</version>
  </dependency>

  application.properties

  spring.datasource.driver-class-name=org.postgresql.Driver
  spring.datasource.url=jdbc:postgresql://192.168.1.10:5432/test_spring_mvc
  spring.datasource.username=test
  spring.datasource.password=test_pw

  
  @SpringBootApplication
  //@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
  public class SpringbootmvcApplication {
    public static void main(String[] args) {
      SpringApplication.run(SpringbootmvcApplication.class, args);
    }
  }

{% endhighlight %}

이후 Spring 재시작 하여 정상 접속 여부 확인


<h2 class="section-heading">MyBatis 연동</h2>

VO & Mapper 생성

{% highlight java %}

package org.codex.mybatis.domain;

public class Board {
	private long board_id;
	private String board_title;
	private String board_content;
	private java.util.Date board_add_date;
	
	public long getBoard_id() {
		return board_id;
	}
	public void setBoard_id(long board_id) {
		this.board_id = board_id;
	}
	public String getBoard_title() {
		return board_title;
	}
	public void setBoard_title(String board_title) {
		this.board_title = board_title;
	}
	public String getBoard_content() {
		return board_content;
	}
	public void setBoard_content(String board_content) {
		this.board_content = board_content;
	}
	public java.util.Date getBoard_add_date() {
		return board_add_date;
	}
	public void setBoard_add_date(java.util.Date board_add_date) {
		this.board_add_date = board_add_date;
	}
}

//----------------------------------------------------

package org.codex.mybatis.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.codex.mybatis.domain.Board;
import org.springframework.stereotype.Repository;

@Mapper
public interface BoardMapper {
	 List<Board> getBoardLists();
}


{% endhighlight %}

resources/mybatis-config.xml & resources/mapper/board.xml 생성

{% highlight xml %}

mybatis-config.xml 

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases>
        <package name="org.codex.mybatis.domain"/>
    </typeAliases>
    <mappers>
        <mapper resource="mapper/board.xml"/>
    </mappers>
</configuration>


//------------------------------------------------------
mapper/board.xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="org.codex.mybatis.mapper.BoardMapper">
    <select id="getBoardLists" resultType="Board">
        SELECT * FROM board
    </select>
</mapper>


{% endhighlight %}

HelloController 로 테스트

{% highlight java %}

package org.codex;

import javax.servlet.http.HttpServletRequest;
 
import org.codex.mybatis.mapper.BoardMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {

	@Autowired
	BoardMapper mapper;
	
	@RequestMapping( "/hello")
	public String hello(HttpServletRequest req , Model model) {
		model.addAttribute("value",mapper.getBoardLists());
		return "hello";
	}
}

//-------------------------
hello.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Hello JSP</title>
</head>
<body>

<c:forEach items="${value}" var="board">
	<tr>
		<td>${board.board_title}</td></br>
		<td>${board.board_content}</td>
	</tr>
</c:forEach>

</body>
</html>

{% endhighlight %}

<a href="#">
    <img src="{{ site.baseurl }}/img/2018_03_01/mybatis.png" alt="Directory Structure">
</a>