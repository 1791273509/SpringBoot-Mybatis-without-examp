十二、SpringBoot整合Mybatis不带example的

2019年3月5日, 星期二
20:45

区别于之前的带example的

1、启动类ControllerTest.java
packagespringbootmybatis.demo;

importorg.springframework.web.bind.annotation.RequestMapping;
importorg.springframework.web.bind.annotation.RestController;

@RestController
publicclassControllerTest{
@RequestMapping("/hello")
publicStringget(){
return"hello";
}
}

2、UserController.java
packagecom.cwh.springbootMybatis.controller;

importjava.util.List;

importorg.springframework.beans.factory.annotation.Autowired;
importorg.springframework.web.bind.annotation.RequestMapping;
importorg.springframework.web.bind.annotation.RestController;

importcom.cwh.springbootMybatis.entity.User;
importcom.cwh.springbootMybatis.service.UserService;

@RestController
@RequestMapping("/user")
publicclassUserController{
@Autowired
privateUserServiceuserService;

@RequestMapping("/getUserInfo")
publicList<User>getUserInfo(){
System.out.println("getUserInfo");
List<User>user=userService.getUserInfo();
System.out.println(user.toString());
returnuser;
}

@RequestMapping("/addUserInfo")
publicStringaddUserInfo(){
System.out.println("addUserInfo");
Useruser=newUser();
user.setId(3L);
user.setName("cwh");
userService.insert(user);
return"success:"+user.toString();
}
}

3、User.java
packagecom.cwh.springbootMybatis.entity;

importjava.io.Serializable;

publicclassUserimplementsSerializable{
privateLongid;
privateStringname;

publicLonggetId(){
returnid;
}

publicvoidsetId(Longid){
this.id=id;
}

publicStringgetName(){
returnname;
}

publicvoidsetName(Stringname){
this.name=name;
}

@Override
publicbooleanequals(Objecto){
if(this==o)returntrue;
if(o==null||getClass()!=o.getClass())returnfalse;

Useruser=(User)o;

if(id!=null?!id.equals(user.id):user.id!=null)returnfalse;

returntrue;
}

@Override
publicinthashCode(){
returnid!=null?id.hashCode():0;
}
}
4、UserMapper.java
packagecom.cwh.springbootMybatis.mapper;

importjava.util.List;

importorg.apache.ibatis.annotations.Mapper;

importcom.cwh.springbootMybatis.entity.User;

@Mapper
publicinterfaceUserMapper{

publicList<User>findUserInfo();
publicintaddUserInfo(Useruser);
publicintdelUserInfo(intid);
}

5、UserService.java
packagecom.cwh.springbootMybatis.service;

importjava.util.List;

importcom.cwh.springbootMybatis.entity.User;

publicinterfaceUserService{
publicList<User>getUserInfo();

publicvoidinsert(Useruser);
}

6、UserServiceImpl.java
packagecom.cwh.springbootMybatis.service.impl;

importjava.util.List;

importorg.springframework.beans.factory.annotation.Autowired;
importorg.springframework.stereotype.Service;
importorg.springframework.transaction.annotation.Transactional;

importcom.cwh.springbootMybatis.entity.User;
importcom.cwh.springbootMybatis.mapper.UserMapper;
importcom.cwh.springbootMybatis.service.UserService;

@Service
publicclassUserServiceImplimplementsUserService{

@Autowired
privateUserMapperuserMapper;

publicList<User>getUserInfo(){

returnuserMapper.findUserInfo();
}

//@Transactional开启事务
@Transactional
publicvoidinsert(Useruser){
userMapper.addUserInfo(user);
inti=1/0;
userMapper.addUserInfo(user);

}
}

7、application.properties
注意这里连接你自己的数据库，我这里是crud
用户名和密码也需要注意
spring.datasource.url=jdbc\:mysql\://127.0.0.1\:3306/crud?useUnicode\=true&characterEncoding\=gbk&zeroDateTimeBehavior\=convertToNull
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
#初始化大小，最小，最大
spring.datasource.initialSize=5
spring.datasource.minIdle=5
spring.datasource.maxActive=20
#配置获取连接等待超时的时间
spring.datasource.maxWait=60000

mybatis.mapper-locations=classpath*:mapper/*Mapper.xml  #你自己的mapper类的包位置
mybatis.type-aliases-package=com.cwh.springbootMybatis.entity  #实体类的位置




整个项目的文件结构


