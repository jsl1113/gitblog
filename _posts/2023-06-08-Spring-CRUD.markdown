---
layout: post
title:  "Spring CRUD"
date:   2023-06-08 18:03:36 +0530
categories: java spring
---

## Spring CRUD
Create(생성), Read(읽기), Update(갱신), Delete(삭제)


### HTTP Methods
인터넷을 통해 보내는 데이터가 어떤 특정한 형식을 가져야 하는지를 정의한 것
- GET: 어떤 데이터를 조회하기 위한 요청이라는 의미의 메소드
- POST: 어떤 데이터를 전송하기 위한 요청이라는 의미의 메소드


### `@RequestParam`
`form` 에 첨부된 `input` 의 데이터중 `name="message"` 인 데이터를 `String message` 매개변수에 할당해 달라는 어노테이션


### `@GetMapping` 과 `@PostMapping`
- `@RequestMapping` 의 경우, 별다른 설정 없이 사용될 때, HTTP 메소드에 상관없이 작동하게 됨
    → 원치 않는 문제를 야기할 수 있음


## Student 목록 출력하기
StudentDto.java
```java
package com.example.crud.model;

public class StudentDto {
    private Long id;
    private String name;
    private String email;

    public StudentDto() {
    }

    public StudentDto(Long id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "StudentDto{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}
```


