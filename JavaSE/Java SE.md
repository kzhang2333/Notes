# 封装 Encapsulation

- 该露的露，该藏的藏
  - 程序设计要追求**“高内聚，低耦合”**， 



# interface and implements

- 接口中也可以定义fields，所有的量都是常量， public static final
  - int AGE = 99;
  - 不一定要这样写

- 接口中所有的定义的方法都是抽象的，public abstract
  - 命名规范：UserService
  - 只定义方法
    - void add();
    - void delete();
    - void update();
    - void query();
- 实现类
  - 命名规范： UserServiceImpl
  - 实现类必须重写（@Override）接口内的方法
- 使用接口可以实现多继承，Java中父子类不可以多继承
- 接口的作用
  1. 约束
  2. 定义方法， 让不同的人实现
  3. 所有的方法都是public abstract
  4. 所有的fields都是public static final
  5. 接口不能被实例化，接口中没有constructor
  6. implements关键字可以实现多个接口
  7. 必须重写接口中的方法

