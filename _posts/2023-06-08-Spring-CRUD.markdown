---
layout: post
title:  "Spring CRUD"
date:   2023-06-08 18:03:36 +0530
categories: java spring
---

### Spring CRUD
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


### Student 목록 출력하기
model / StudentDto.java
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

templates / create.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Create Student</title>
</head>
<body>
<h1>Create Student</h1>
<form th:action="@{/create}" method="post">
<!--  사용자가 제공할 데이터에 알맞은 input과 label을 만든다.-->
  <label for="name-input">Name: <input id="name-input" name="name"></label><br>
  <label for="email-input">Email: <input id="email-input" name="email"></label><br>
<!--  데이터 제출 버튼-->
  <input type="submit"/>
</form>
<a href="/home">Back</a>
</body>
</html>
```

templates / home.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Student List</title>
</head>
<body>
    <h1>Student List</h1>
    <!--등록된 학생이 없을 떄 -->
    <div th:if="${studentList.isEmpty()}">
        <p> No Student Here ... </p>
    </div>
    <div th:unless="${studentList.isEmpty()}" th:each="student: ${studentList}">
        <p>번호 : [[${student.id}]]</p>
        <p>이름 : [[${student.name}]]</p>
        <p>이메일 : [[${student.email}]]</p>
    </div><hr>
<a th:href="@{/create-view}">Create</a>
</body>
</html>
```

StudentService.java
```java
import com.example.crud.model.StudentDto;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.List;

@Service
public class StudentService {
    // 복수의 StudentDto를 담는 변수
    List<StudentDto> studentDtoList = new ArrayList<>();
    private Long nextId = 1L;

    // 새로운 Student를 생성하는 메소드
    public StudentDto createStudent(String name, String email){
        StudentDto studentDto = new StudentDto(nextId++, name, email);
        studentDtoList.add(studentDto);
        return studentDto;
    }

    public List<StudentDto> readStudentAll(){
        return studentDtoList;
    }
}
```

StudentController.java
```java
import com.example.crud.model.StudentDto;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class StudentController {
    // StudentService를 Controller 에서
    private StudentService studentService;

    public StudentController(StudentService studentService) {
        this.studentService = studentService;
    }

    @GetMapping("/create-view")
    public String createView(){
        return "create";
    }

    @PostMapping("/create")
    public String create(@RequestParam("name") String name, @RequestParam("email") String email){
        StudentDto studentDto = studentService.createStudent(name, email);
        System.out.println(studentDto.toString());
        return "redirect:/home";
    }

    @GetMapping("/home")
    public String home(Model model) {
        model.addAttribute("studentList", studentService.readStudentAll());
        return "home";
    }
}
```

### 결과 화면
![image](https://github.com/jsl1113/gitblog/assets/55522275/ecdabc85-0dfa-446e-875c-ee1ac39b808a)

