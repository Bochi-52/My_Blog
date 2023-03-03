---
title: SpringBoot+Vue前后端分离指南
date: 2021-09-20
tags:
 - SpringBoot
 - Vue
categories:
 - 使用指南
---

# SpringBoot+Vue



## 创建vue工程

```
$ vue ui //vue 3.0以上支持的图形界面创建Vue

>npm run serve //启动vue工程
```



## 新建SpringBoot应用

> 组件选择
>
> - Lombok
> - Spring web
> - Spring Data JPA
> - MySQL Driver

**resources**中application.**properties**文件改为application.**yml**

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/sql?useUnicode=true&characterEncoding=utf-8&useSSL=true&serverTimezone=UTC
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
  jpa:
    show-sql: true
    properties:
      hibernate:
        format_sql: true
server:
  port: 8181

```

> 不设置端口默认也是8080和vue工程端口冲突



## 绑定数据表信息,测试端口

创建实体类com.xin.entity

```java
package com.xin.entity;

import lombok.Data;

import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
@Data	//Lombok
public class Book {
    @Id  //主键
    private Integer id;
    private String name;
    private String author;
}

```

com.xin.repository

> JpaRepository<实体类类型,主键类型> 中 集成了许多方法

```java
package com.xin.repository;

import com.xin.entity.Book;
import org.springframework.data.jpa.repository.JpaRepository;

public interface BookRepository extends JpaRepository<Book,Integer>{

}

```

com.xin.controller

```java
package com.xin.controller;

import com.xin.entity.Book;
import com.xin.repository.BookRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
@RequestMapping("/book")
public class BookController {
    @Autowired
    private BookRepository bookRepository;

    @GetMapping("/findAll")
    public List<Book> findAll(){
        return bookRepository.findAll();
    }
}
```

**测试repository**

```java
@SpringBootTest
class SpringbootApplicationTests {

	@Autowired
	private BookRepository bookRepository;
	@Test
	void findAll() {
		System.out.println(bookRepository.findAll());
	}

}
```

**测试controller接口**

> http://localhost:8181/book/findAll



## axios实现前端调用后端接口

vue中安装axios

```
> vue add axios
```

前端调用

```js
created(){
    const _this =this
    
    axios.get('http://localhost:8181/book/findAll')
    .then(function(res) {
        _this.books=res.data
    })
    
}
```



## 跨域问题的解决

创建配置类com.xin.config

```java
package com.xin.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CrosConfig implements WebMvcConfigurer {
    @Override
    public void  addCorsMappings(CorsRegistry registry){
        registry.addMapping("/**")
                .allowedOriginPatterns("*")       
      		    .allowedMethods("GET","HEAD","POST","PUT","DELETE","POTIONS")
                .allowCredentials(true)
                .maxAge(3600)
                .allowedHeaders("*");
    }
}

```



## Vue集成ElementUI

> ElementUI	https://element.faas.ele.me/#/zh-CN/component/installation



## Vue router动态构建菜单

```vue
<el-menu>
        <el-submenu v-for="(item, index) in $router.options.routes" :key="item" :index="index + ''">
          <template slot="title"><i class="el-icon-menu"></i>{{ item.name }}</template>
          <el-menu-item v-for="(item2, index2) in item.children" :key="item2" :index="index + '-' + index2">
            {{ item2.name }}</el-menu-item>
        </el-submenu>
      </el-menu>
```

```js
const routes = [
{ path: '/',
  name: '导航一',
  component: index,
  children:[
    {path:'/page1',name:'页面一',component:page1},
    {path:'/page2',name:'页面二',component:page2}
  ]
},
{ path: '/navigation',
  name: '导航二',
  component:index,
  children:[
    {path:'/page3',name:'页面三',component:page3},
    {path:'/page4',name:'页面四',component:page4}  
  ]
}
]
```



## 菜单与router的绑定

1. el-menu 标签添加router属性

   ```vue
   <el-menu router>
   </el-menu>
   ```

2. 页面中添加<router-view></router-view>，动态渲染选择的router

3. el-menu-item 标签**index**的值就是要跳转的router

   ```vue
   <el-menu-item v-for="item2 in item.children" 
                 :key="item2" 
                 :index="item2.path">
    	{{ item2.name }}
   </el-menu-item>
   ```

   

## 分页

Vue ElementUI

```vue
<el-pagination background 
                   v-if="total!=null"
                   layout="prev, pager, next" 
                   :page-size="pageSize"
                   :total="total"
                   @current-change="page">
</el-pagination>
```

```js
data() {
      return {
        total:null,
        pageSize:5,
        tableData:null
      }
},
 methods: {
      page(currentchange){
        const _this=this
        axios.get('http://localhost:8181/book/findAll/+'+currentchange+'/5')
             .then(function(res){
                _this.tableData=res.data.content
                _this.total=res.data.totalElements
              })
      }
},
created(){
      const _this=this
      axios.get('http://localhost:8181/book/findAll/1/5')
      .then(function(res) {
        _this.tableData=res.data.content
        _this.total=res.data.totalElements
      })
}
```

```java
package com.xin.controller;

@RestController
@RequestMapping("/book")
public class BookController {
    @Autowired
    private BookRepository bookRepository;

    @GetMapping("/findAll/{page}/{size}")//获取前端传来的数据
    public Page<Book> findAll(@PathVariable("page") Integer page,@PathVariable("size") Integer size){
        Pageable pageable= PageRequest.of(page-1,size);
        return bookRepository.findAll(pageable);
    }
}

```

## 增

> Vue ElementUI 提供表单校验功能
>
> 定义rules对象，在rules对象中设置表单各个选项的校验规则
>
> - :model="ruleForm"    数据绑定
> - :rules="rules"          校验绑定

```vue
  <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
    
    <el-form-item label="书名" prop="name">
      <el-input v-model="ruleForm.name"></el-input>
    </el-form-item>
    
    <el-form-item label="作者" prop="author">
      <el-input v-model="ruleForm.author"></el-input>
    </el-form-item>

    <el-form-item>
      <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
      <el-button @click="resetForm('ruleForm')">重置</el-button>
    </el-form-item>

 </el-form>
```

```js
  data() {
    return {
      ruleForm: {
        name: "",
        author: ""
      },
      rules: {
        name: [
          { required: true, message: "请输入活动名称", trigger: "blur" },
          { min: 3, max: 5, message: "长度在 3 到 5 个字符", trigger: "blur" },
        ],
        author: [
          { required: true, message: "请选择活动区域", trigger: "change" },
        ]
      }
    }
  }
```

- required: true 是否为必填项
- message: "提示信息"
- trigger: "blur"  触发事件





## 删



## 改



## 查















