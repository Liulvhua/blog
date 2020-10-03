## 配置idea

### 配置插件

首先下载以下文件，均在群文件可以找到，如果发先导入失败，尝试导入其他版本号的同名文件，我都发在群里了

![](https://image.yangxiansheng.top/img/20200612182909.png?imagelist)

![](https://image.yangxiansheng.top/img/20200612182845.png?imagelist)

然后打开idea开发工具，进行以下配置

- 打开设置

  ![](https://image.yangxiansheng.top/img/20200612183012.png?imagelist)

- 点击插件区域

  ![](https://image.yangxiansheng.top/img/20200612183023.png?imagelist)



- 导入本地插件

  ![](https://image.yangxiansheng.top/img/20200612183047.png?imagelist)

![](https://image.yangxiansheng.top/img/20200612183057.png?imagelist)

点击完成之后，重启idea即可看到导入成功。

### 配置sql

首先本地创建好数据库

然后打开`idea`右侧的导航 `dataBase`

![](https://image.yangxiansheng.top/img/20200612183349.png?imagelist)

点击左侧的`加号`按钮，选中`mysql`

![](https://image.yangxiansheng.top/img/20200612183401.png?imagelist)

配置`mysql`账号密码（comment不填 用户名密码）

> 首次导入左下角需要下载驱动，点检download即可

![](https://image.yangxiansheng.top/img/20200612183412.png?imagelist)

配置完成点击应用即可

右侧看到这样的界面，就说明连接成功

![](https://image.yangxiansheng.top/img/20200612183730.png?imagelist)

### 配置maven

打开设置，搜索`maven`

![](https://image.yangxiansheng.top/img/20200612183845.png?imagelist)

**打开这个文件路径， 如果该文件夹没有`setting.xml`这个文件，去群文件下载这个文件，然后拷贝到这个路径，最后点击应用即可**

![](https://image.yangxiansheng.top/img/20200612183906.png?imagelist)

## 导入项目

### 导入群文件，并配置

1. 首先打开设置，导入群文件我之前发过的项目文件

![](https://image.yangxiansheng.top/img/20200612184219.png?imagelist)

![](https://image.yangxiansheng.top/img/20200612184233.png?imagelist)

**点击ok即可**

2. 然后打开配置文件

![](https://image.yangxiansheng.top/img/20200612184305.png?imagelist)

修改为你的数据库名 数据库账号密码即可

![](https://image.yangxiansheng.top/img/20200612184330.png?imagelist)

3. 测试项目能否运行，按下`shift`+`F10`即可启动，看到这个界面及代表配置成功

   ![](https://image.yangxiansheng.top/img/20200612184657.png?imagelist)

### 书写接口的入门

#### 导入模型

点击侧边栏这个选项

![1591960122435](C:\Users\努力中的杨先生\AppData\Roaming\Typora\typora-user-images\1591960122435.png)

如果导入项目发现没有这个选项则打开设置

![](https://image.yangxiansheng.top/img/20200612190941.png?imagelist)

添加`JPA`模块，搜索`JPA`即可,最后点击应用

![](https://image.yangxiansheng.top/img/20200612190956.png?imagelist)



然后点击右键 选择`选择数据库schema`

![](https://image.yangxiansheng.top/img/20200612191203.png?imagelist)

**分别为：选择数据库  选择导入模型路径(选择`model`这个包即可)，选择导入的表，勾选上点击确定**

![](https://image.yangxiansheng.top/img/20200612191227.png?imagelist)

**删除这些除属性之外的方法，只保留成员变量即可, 然后添加这样的注解 ，这里注解需要打上标红的这两个注解**

![](https://image.yangxiansheng.top/img/20200612191647.png?imagelist)

#### 书写`controller`层

在`api.v1`包下新建文件，文件命名为`业务对象+Controller`即可，例如用户接口，则命名为`UserController`

```java
// 固定注解
@RestController
// 路由前缀  配置完成接口路由变成 localhost:8080/student
@RequestMapping("/student")
public class StudentController {
    // 导入业务对象
    @Autowired
    private StudentService studentService;

    // 书写一个get请求的接口  通常get请求是获取信息 post请求是私密信息传递 比如get请求:获取学生成绩，post请求:登录，注册，删除，修改
    // 这里的{sno} 代表传入一个变量 可以为1,2,3  比如我查询学号为1的学生详情 接口路由:localhost:8080/student/detail/1
    @GetMapping("/detail/{sno}")
    // @PathVariable Long sno  代表接收路由变量,即接收sno这个变量，如果不需要传入任何参数，就不需要这个注解。
    public StudentUser getdetail(@PathVariable Long sno){
        // 调用业务层方法，传入学号,返回数据库中的学生信息这条记录
        StudentUser student = studentService.getdetail(sno);
        // 如果不存在
        if(student == null){
            throw new NotFoundException(10002);
        }
        // 返回这条记录
        return StudentUser;
    }
    
    
      //post请求 打上@RequestBody这个注解，然后定义一个类接收传过来的对象

   @PostMapping("/register")
   public Map<String, Object> register(@RequestBody StudentRegisterDto studentRegisterDto){
        studentService.register(studentRegisterDto);
        Map<String, Object> data = new HashMap<>();
        data.put("code",200);
        data.put("msg","注册成功");
        return data;
    }
    
}
```

#### 书写`repository`层和`service`层

分别在这些包下新建文件，命名方式和控制层相似

`repository`层

> 这里是定义接口`interface`,接口只需要定义方法接口，想好返回什么类型的数据再定义

```java
public interface StudentRepository extends JpaRepository<StudentUser,Long> {

    StudentUser findOneBySno(Long sno);
    StudentUser findOneById(Long id);

    // 登录判断

    Long countByUsernameAndPassword(String username,String password);
    StudentUser findOneByUsername(String username);

    // sno删除

    void deleteAllBySno(Long sno);
}
```

方法都是有提示的，参考

![](https://image.yangxiansheng.top/img/20200507164608.png?imagelist)

`service`层

> 这里就要去写具体的业务

```java

@Service
public class StudentService {
    // 导入repository层 调用方法操作数据库
    @Autowired
    private StudentRepository studentRepository;

// 学生登录
    public Map<String, Object> login(String username, String password){
        // 查询记录的数量
        Long result = studentRepository.countByUsernameAndPassword(username, password);
        if(result == 1){
            // 返回这条记录
            StudentUser user = studentRepository.findOneByUsername(username);
           // 定义map集合 添加一个对象
            Map<String, Object> data = new HashMap<>();
            data.put("userInfo",user);
            return data;
        }else{
            throw new PasswordException(10004);
        }
    }
    
}
```

#### 启动项目,然后再`postman`中测试

示例：

`get请求`

![](https://image.yangxiansheng.top/img/20200612190550.png?imagelist)

`登录请求`
![](https://image.yangxiansheng.top/img/20200612190600.png?imagelist)

### 总结

首先导入模型，然后新建控制层，定义接口，书写业务层，配置`repository`层方法，调整控制层返回数据，最后在`postman`里测试接口

> 建议: 提前想好需要写哪些接口，比如我的，后端写完了，前端对接起来非常迅速的。

![](https://image.yangxiansheng.top/img/20200608120039.png?imagelist)

### 例子

我这里书写两个典型的接口‘

- 传入学号，获取学生的基本信息
- 登录

#### 获取信息

`model`

```java
@Getter
@Setter
@Builder
@Entity
@AllArgsConstructor
@NoArgsConstructor
public class StudentUser {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Long sno;
    private String name;
    private String username;
    @JsonIgnore
    private String password;
    private Integer gender;
    private String college;
    private String subject;
    private String mobile;
    private String address;
    // 添加默认值

    private String avatar;

    // 连表查询

    @OneToMany
    @JoinColumn(name = "sno")
    private List<StudentClass>courseList;
}
```



`controller`层

```java
 @GetMapping("/detail/{sno}")
// @PathVariable 接收学号变量
    public ResponseVo getdetail(@PathVariable Long sno){
        StudentUser student = studentService.getdetail(sno);
        if(student == null){
            throw new NotFoundException(10002);
        }
        return new ResponseVo(student);
    }
```

`service层`

```java

    // 查询学生详情和成绩
    public StudentUser getdetail(Long sno){
        return studentRepository.findOneBySno(sno);
    }
```

`repository`层

```java

public interface StudentRepository extends JpaRepository<StudentUser,Long> {

    StudentUser findOneBySno(Long sno);

}
```

测试

![](https://image.yangxiansheng.top/img/20200612193849.png?imagelist)

####  登录

`model`

```java
@Getter
@Setter
@Builder
@Entity
@AllArgsConstructor
@NoArgsConstructor
public class StudentUser {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Long sno;
    private String name;
    private String username;
    @JsonIgnore
    private String password;
    private Integer gender;
    private String college;
    private String subject;
    private String mobile;
    private String address;
    // 添加默认值

    private String avatar;

    // 连表查询

    @OneToMany
    @JoinColumn(name = "sno")
    private List<StudentClass>courseList;
}
```



`controller`层

```java
 // 学生登录
    @PostMapping ("/login")
    public ResponseTVo Login(@RequestBody StudentUserDto userDto){
        return new ResponseTVo("登录成功",200,studentService.login(userDto.getUsername(),userDto.getPassword()));
    }

```

`service层`

```java

  // 学生登录
    public Map<String, Object> login(String username, String password){
        Long result = studentRepository.countByUsernameAndPassword(username, password);
        if(result == 1){
            StudentUser user = studentRepository.findOneByUsername(username);
            Map<String, Object> data = new HashMap<>();
            // 暂时不使用jwt token
            data.put("userInfo",user);
            return data;
        }else{
            throw new PasswordException(10004);
        }
    }
```

`repository`层

```java

public interface StudentRepository extends JpaRepository<StudentUser,Long> {

   // 登录判断
    Long countByUsernameAndPassword(String username,String password);
    StudentUser findOneByUsername(String username);


}
```

测试

![](https://image.yangxiansheng.top/img/20200612194107.png?imagelist)

### 更新内容

#### 注册

1. 书写`controller`层

   ```java
      // 注册
       @PostMapping("/register")
       public OptionVo register(@RequestBody StudentDto studentDto){
        
       }
   ```

   > `@RequestBody`的作用就是接受`post`请求传递的对象注解
   >
   > 然后需要通过`StudentDto`这个类去接受传递传递的对象，帮助去取里面的值	

`StudentDto.java`

```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class StudentDto {
    private String name;
    private String password;
    private Integer gender;
}

```

2. 书写`vo`层，因为不需要返回任何数据,所以需要定义一个类

   ```java
   @Getter
   @Setter
   @AllArgsConstructor
   @NoArgsConstructor
   public class OptionVo {
       private String msg;
       private Integer code;
   }
   
   ```
写完发现写不下去了，之后就要编写`service`层去写具体的业务了。
   
3. 书写`service`层

   ```java
    // 注册 定义成void类型 参数是传递过来的studentDto对象
    public void register(StudentDto studentDto){
        // 查询数据库是否已经拥有该用户
        Student student = this.studentRepository.findOneByNameAndPassword(studentDto.getName(),studentDto.getPassword());
        if(student != null){
            // 已存在 抛出异常
            throw new AllExistedException(10002);
        }else{
            // save(params)  params    Student.setName()  Student.setPassowrd Student.setGender
            // 构建模型对象 这一步操作相当于setName 用Builder去构建会变得很简单
            Student student1 = Student.builder().name(studentDto.getName()).password(studentDto.getPassword())
                 .gender(studentDto.getGender()).build();
            // 调用JPA的添加方法 save()
            this.studentRepository.save(student1);
   
        }
 }
   
   ```
   
4. 补全`controller层`

   ```java
   // 注册
       @PostMapping("/register")
       public OptionVo register(@RequestBody StudentDto studentDto){
           this.studentService.register(studentDto);
           return new OptionVo("注册成功",200);
       }
   ```

5. 测试接口

   ![](https://image.yangxiansheng.top/img/20200616233556.png?imagelist)

   

#### 分页获取学生列表

1. 编写`controller`层

   返回格式需要构建一个`pagingVo`对象统一分页格式，

   `PagingVo`

   ```java
   
   @Getter
   @Setter
   @NoArgsConstructor
   public class PaggingVo<T> {
       private Long total;
       private Integer size;
       private Integer page;
       private Integer total_page;
       // 使用到泛型 因为不止student一处使用到这个vo 使用泛型要将类名上也要打上泛型
       private List<T> items;
   
       // 构造方法
       public PaggingVo(Page<T> pageT) {
           /*
           传入service获取到的Page对象 初始化参数
            */
           this.initPageParamters(pageT);
           // items - 查询出的分页记录
           this.items = pageT.getContent();
       }
   
   
       // set分页信息参数 这些参数都是由Page对象内置，这里只是做了一个set操作,赋值
       void initPageParamters(Page<T> pageT) {
           this.total = pageT.getTotalElements();
           this.size = pageT.getSize();
           this.page = pageT.getNumber();
           this.total_page = pageT.getTotalPages();
       }
   
   }
   
   ```

   然后正式编写`controller`层

   > 首先传入`page`,`size`,但是考虑到分页参数不传的情况下必须要设置默认值
   >
   > 所以定义方法时打上注解`@RequestParams(name="page",defaultValue="0")Integer page`


```java
  @GetMapping("/getStudentList")
    public ResponseVo getlist(@RequestParam(name = "page",defaultValue = "0")Integer page,
                              @RequestParam(name = "size",defaultValue = "10")Integer size){
    }
```

2. 继续编写`service`层

   ```java
      // 分页查询学生列表  返回对象是一个分页类型的对象 Page<T>
      public Page<Student> getlist(Integer page,Integer size){
           // jpa创建分页器
           Pageable pageable = PageRequest.of(page,size);
           // 相当于 select * from .. 传入pageable分页器
           return this.studentRepository.findAll(pageable);
       }
   ```

3. 然后补全`controller`的方法

   ```java
    public ResponseVo getlist(@RequestParam(name = "page",defaultValue = "0")Integer page,
                                 @RequestParam(name = "size",defaultValue = "10")Integer size){
   
        // 获取刚刚在service获取的分页对象
           Page page1 = studentService.getlist(page, size);
        // 统一分页格式 一开始定义的接收类
           PaggingVo<Student>paggingVo = new PaggingVo<>(page1);
        // 返回ResponseBo 类格式的对象 传入msg,data(pagingvo),code
           return new ResponseVo("返回成功",paggingVo,200);
       }
   ```

4. 测试接口

   **不传参数**

   ![](https://image.yangxiansheng.top/img/20200616234619.png?imagelist)

​          **传参数**

![](https://image.yangxiansheng.top/img/20200616234635.png?imagelist)

返回以上数据及代表书写成功

