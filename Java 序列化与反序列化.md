[TOC]
## 常见疑问
> 1.序列化、反序列化是干什么的？
> 2.常见序列化方法有哪些？
> Java序列化要注意什么？

## 一、什么是序列化
### 1.1 序列化简介
       在业务开发中经常有这样的场景，我们需要把某个配置保存成一串文本，使用的时候再从某个地方或DB读取出来解析，这里的文本转换和解析是如何实现的呢？
       又或者在网络传输中，TCP协议传输的是字节流，服务端如何把内存对象转换成字节流发送出去、客户端又如何把字节流转换成对象呢？
       一般这类的操作被称为序列化（Serialization）和反序列化（Deserialization）。把对象转换为字节流的过程称为对象的序列化；把字节流恢复成对象的过程称为对象的反序列化。
### 1.2 序列化协议
    广义上的序列化不仅仅可以是对象转成字节流，还可以是其他的格式。不同的格式使用不同的序列化协议。
    协议一般是指交流双方共同遵守的一种事先约定。有了协议序列化就有了实现的基础。序列化协议约定了格式本身的语法和对象与格式的映射规则。
    常见的序列化协议：Protobuf、Thrift、Hessian、XML、JSON等。
### 1.3 序列化作用

* 把对象的字节序列永久的保存到硬盘上，通常存放在一个文件中
* 在网络上传输对象的字节序列

## 二、常见序列化方法有哪些

| 序列化方法 | 描述 |
| --- | --- |
| Java原生序列化 | 利用InputStream和OutputStream进行转化 |
| JSON序列化 | 常用的jackson包，一般用com.fasterxml.jackson.databind.ObjectMapper进行转化 |
| FastJson序列化 | 阿里制造，一般用com.alibaba.fastjson.JSON |
| Gson序列化 | google制造，一般用com.google.gson.Gson |
| ProtoBuff | google制造，二进制 |
| Thrift序列化 | thrift框架使用，二进制 |
| Hessian | pigeon常用，轻量，二进制|

## 三、Java序列化要注意什么
### 3.1 Java序列化实现
常见写法：实现Serializabel【推荐】
```
public class Person implements Serializable {

    /**
     * 序列化ID
     */
    private static final long serialVersionUID = 1L;

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
其他写法：实现Externalizable
```
public class Person implements Externalizable {

    /**
     * 序列化ID
     */
    private static final long serialVersionUID = 1L;

    private String name;

    private String sex;

    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
          out.writeObject(name);
          out.writeObject(sex);
          out.writeInt(age);
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
          this.name = (String)in.readObject();
          this.sex = (String)in.readObject();
          this.age = in.readInt();
    }
}
```
### 3.2 父类的序列化
    问题：当子类实现了Serializable，但是父类没有，序列化时会拿到父类的状态吗？
    解答：由于父类没有序列化，将导致反序列化时，虽然创建出了父类，但是父类的状态没有被保存，全部都是初始值。
### 3.3 静态变量序列化
    问题：序列化对象中，存在静态变量时，静态变量的状态会保存吗？
    解答：序列化保存的是对象的状态，静态变量属于类的状态，因此序列化并不保存静态变量。
### 3.4 Transient关键字
    Transient关键字的作用是控制变量的序列化，用Serializable时，凡是用Transient声明的变量，状态都不会被序列化（手动指定的除外）。由此可知，反序列化时，该被Transient声明的字段，将会是初始值。
    