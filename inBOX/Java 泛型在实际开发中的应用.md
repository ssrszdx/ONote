# Java 泛型在实际开发中的应用

# [Java 泛型在实际开发中的应用](https://www.cnblogs.com/ldh-better/p/7127308.html)

**目录**

* [一：泛型出现的背景](https://www.cnblogs.com/ldh-better/p/7127308.html#_label0)
* [二： 泛型的语法使用](https://www.cnblogs.com/ldh-better/p/7127308.html#_label1)

  * [1：使用具体的泛型类型： 尖括号内带有具体的类型。可以限定这个Map的key和value只能是字符串](https://www.cnblogs.com/ldh-better/p/7127308.html#_label1_0)
  * [ 2：方法声明的时候 ： public   T getValue(){...}](https://www.cnblogs.com/ldh-better/p/7127308.html#_label1_1)
  * [3 :类或者接口使用泛型  interface Collection {..}](https://www.cnblogs.com/ldh-better/p/7127308.html#_label1_2)
  * [4:  声明带边界的泛型   class userDao](https://www.cnblogs.com/ldh-better/p/7127308.html#_label1_3)
  * [5：用于通配符  ](https://www.cnblogs.com/ldh-better/p/7127308.html#_label1_4)
* [三： 泛型可以用到那些地方](https://www.cnblogs.com/ldh-better/p/7127308.html#_label2)
* [四： Java中泛型独特之处](https://www.cnblogs.com/ldh-better/p/7127308.html#_label3)

  * [Java会在编辑期把泛型擦除掉](https://www.cnblogs.com/ldh-better/p/7127308.html#_label3_0)
  * [擦除的原理以及边界](https://www.cnblogs.com/ldh-better/p/7127308.html#_label3_1)
  * [泛型擦除肯可能导致的问题](https://www.cnblogs.com/ldh-better/p/7127308.html#_label3_2)
* [五： 泛型中特殊使用](https://www.cnblogs.com/ldh-better/p/7127308.html#_label4)

  * [程序如果运行时需要类型信息](https://www.cnblogs.com/ldh-better/p/7127308.html#_label4_0)
  * [异常中使用泛型](https://www.cnblogs.com/ldh-better/p/7127308.html#_label4_1)
  * [数组与泛型](https://www.cnblogs.com/ldh-better/p/7127308.html#_label4_2)
  * [泛型的一些其他细节：　　](https://www.cnblogs.com/ldh-better/p/7127308.html#_label4_3)
* [六：简单概括](https://www.cnblogs.com/ldh-better/p/7127308.html#_label5)

**正文**

　　java泛型是对[Java](http://www.2cto.com/kf/ware/Java/)语言的类型[系统](http://www.2cto.com/os/)的一种扩展，泛型的本质就是将所操作的数据类型参数化。下面我会由浅入深地介绍Java的泛型。

[回到顶部](https://www.cnblogs.com/ldh-better/p/7127308.html#_labelTop)

## 一：泛型出现的背景

在java代码里，你会经常发现类似下边的代码：

![复制代码](0.8038229421682838-20220901151245-skq8qvj.png)[&quot;复制代码&quot;]("复制代码")

```
public class Test {
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add("hah");
        //list.add(new Test());
       // list.add(1);
        for (Object object : list) {
            String s1 = (String)object;
            //.....如果是你你该如何拿出list的值，如果list中放着上边的不同类型的东西。无解
        }
    }
}
```

![复制代码](0.011524050625770952-20220901151245-jkxklh2.png)[&quot;复制代码&quot;]("复制代码")

　　编码的时候，不加泛型是可以的，但是 你从容器中拿出来的时候必须强制类型转换，第一是多敲很多代码，第二极容易发生类型转换错误，这个运行时异常 比如你把上边

注释的代码放开，程序在获取容器的地方就会报运行时异常 ClassCasrException

Java语言的设计者引入了泛型，暂时先不追究它内在是怎么实现的。只需要知道，如果我们像下边这么写，我们就不需要强制类型转换。我们也不需要担心运行是异常了。

```
List<String> newList = new ArrayList<String>();
newList.add("hhe");
newList.add("123");
String s1 = newList.get(0);//不需要强制类型转换，因为我加了泛型，我就认为它里边一定都是String
```

[回到顶部](https://www.cnblogs.com/ldh-better/p/7127308.html#_labelTop)

## 二： 泛型的语法使用

### 1：使用具体的泛型类型： 尖括号内带有具体的类型。可以限定这个Map的key和value只能是字符串

```
Map<String, String> map = new HashMap<String, String>();
map.put("key","value");
String value = map.get("key")
```

从面向对象的角度看，使用对象的时候，泛型内传入的具体的类型。声明的时候采用尖括号内加占位符的形式，比如这是HashMap的源码

![复制代码](0.9905018391181126-20220901151245-ll5a0jd.png)[&quot;复制代码&quot;]("复制代码")

```
public class HashMap<K,V>
    extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable{
    ...
   public HashMap(Map<? extends K, ? extends V> m) {
        this(Math.max((int) (m.size() / DEFAULT_LOAD_FACTOR) + 1,
                      DEFAULT_INITIAL_CAPACITY),     DEFAULT_LOAD_FACTOR);
        putAllForCreate(m);
    }  
    ...
      public V remove(Object key) {
        Entry<K,V> e = removeEntryForKey(key);
        return (e == null ? null : e.value);
    }   
}
```

![复制代码](0.021113605545701505-20220901151245-1gde0kd.png)[&quot;复制代码&quot;]("复制代码")

### 2：方法声明的时候 ： public  <T> T getValue(){...}

　　在上边的代码中，我们可以看到在类上如何定义泛型，也看到了类上定义的占位符在类的普通方法上可以直接使用。但是如果想在静态方法上定义泛型，这需要单独的处理  。下面我们单独对方法上如何定义

和使用泛型进行介绍（注意：方法上是否定义泛型和类上是否定义没有必然的联系）

比如Web项目中，泛型是修饰类型的，在方法上，一般就是返回值和参数列表

* 　　返回值类型：可以定义为List<String>等形式，但是实际开发中，一般都是不知道具体类型，定义形式如下  <T> List<T> test(T t){...} ，前边的<T>可以理解为泛型的声明，你只有声明了T,你才可以在              方法中用到T,这一具体的类型， List<T>是具体的返回值类型。
* 　　方法传参： 可以用占位符限定的容器 比如 List<T>，或者直接是占位符 T

![复制代码](0.4621674909702955-20220901151245-lmbpk4i.png)[&quot;复制代码&quot;]("复制代码")

```
public class BaseServiceImpl implements BaseService {
    protected <T> List<T> calcPage(String hql, PageContext pageContext,
            Object... params) {
        int total = getDataTotalNum(hql, params);
        pageContext.setTotal(total);
        List<T> list = (List<T>) getPageDataByHQL(hql, pageContext.getRows(),
                pageContext.getPage(), pageContext.getTotal(), params);
        return list;
    }
    @Override
    @Sync
    public void deleteBatchVO(final List<?> dataList) throws ServiceException {
        baseDAO.deleteBatchVO(dataList);
    }
    @Override
    public List<?> getPageDataByHQL(final String hql,
            final Map<String, Object> filter) throws ServiceException {
        return baseDAO.getPageDataByHQL(hql, filter);
    }
}
```

![复制代码](0.22856811509307104-20220901151245-71ysmpc.png)[&quot;复制代码&quot;]("复制代码")

简单的例子：

```
public <T> T TestG(T t){
        return t;
    }
```

 方法定义的时候，泛型是这样设计，在使用的时候，代码如下：

```
List<TaclUserinfo> list = calcPage(hqluser1.toString(), pageContext,
                taclRole.getRoleid(), taclRole.getTenantId());
//返回值类型 是<T>List<T>的，j接收的时候，我直接用List<具体类>
```

### 3 :类或者接口使用泛型  interface Collection<V> {..}

　　上边的HashMap代码中，也看到了在类上使用泛型的具体例子。在真正的项目上，一些基础的公共类经常定义泛型，如下：

![复制代码](0.11174345958408471-20220901151245-qimtlrg.png)[&quot;复制代码&quot;]("复制代码")

```
 1 public interface GenericDao<T, ID extends Serializable> {
 2 
 3     public abstract void saveOrUpdate(T t) throws DataAccessException;
 4 
 5     public abstract T get(ID id) throws DataAccessException;
 6 
 7     public abstract List<T> query(String queryString) throws DataAccessException;
 8 
 9     public abstract Serializable save(T t) throws DataAccessException;
10 
11     public abstract void saveOrUpdateAll(Collection<T> entities) throws DataAccessException;
12 
13     public abstract List<T> loadAll() throws DataAccessException;
14 
15     public abstract void merge(T t) throws DataAccessException;
16 
17 }  
```

![复制代码](0.6408028621200573-20220901151245-s1tn0jq.png)[&quot;复制代码&quot;]("复制代码")

接口的实现类： 传入参数为T，实现类中也可以继续用T,返回为T也可以用T;实现的时候 可以用 extends限定泛型的边界。

![复制代码](0.7439131617517721-20220901151245-n9ie1lf.png)[&quot;复制代码&quot;]("复制代码")

```
public abstract class GenericDaoImpl<T extends BaseEntity, ID extends Serializable> extends
        HibernateDaoSupport implements GenericDao<T, ID> {
  
    public void merge(T t) throws DataAccessException {
        TenantInterceptor.setTenantInfoToEntity(t);
        getHibernateTemplate().merge(t);
    }
  
         public T get(ID id) throws DataAccessException {
        // getHibernateTemplate().setCacheQueries(true);
        T load = (T) getHibernateTemplate().get(getEntityClass(), id);
        return load;
    }  
}
```

![复制代码](0.28961084381384883-20220901151245-24lfduo.png)[&quot;复制代码&quot;]("复制代码")

具体使用的时候：

```
public class UserDao extends GenericDaoImpl<User, Serializable> {
    ...//比如get() merge()这些方法不需要在单独编写，直接调用
}  
```

### 4:  声明带边界的泛型   class userDao<T extends BaseEntity>

　　Java中泛型在运行期是不可见的，会被擦除为它的上级类型。如果你是无界的泛型参数类型，就会被替换为Object.

![复制代码](0.33977976648943914-20220901151245-mcx3179.png)[&quot;复制代码&quot;]("复制代码")

```
public class RedColored<T extends Color> {
    public T t;
    public void color(){
        t.getColor();//T的边界是Color,所以可以调用getColor()，否则会编译报错
    }
}

abstract class Color{
    abstract void getColor();
}
```

![复制代码](0.18528224022229212-20220901151245-mccypnb.png)[&quot;复制代码&quot;]("复制代码")

 　　类似这样的定义形式：GenericDaoImpl<T extends BaseEntity, ID extends Serializable> ,java重载了extends，标注T的边界就是BaseEntity。如果需求是继承基类，那么边界定义在子类上

类似

```
class Colored2<T extends Color> extends RedColor<T>
```

### 5：用于通配符  <?>

 　　参考于（ [Java 通配符解惑](http://www.linuxidc.com/Linux/2013-10/90928.htm)  ）泛型类型的子类型的不相关性。比如 现在List<Cat>并不是List<Anilmal>是`两种不同的类型`;且`无继承`关系 。那么，我们像想要传入的参数既可能是List<Cat>

也有可能是List<Annimal>

![复制代码](0.0883659272062324-20220901151245-la9nny4.png)[&quot;复制代码&quot;]("复制代码")

```
public class AnimalTrainer {
    public void act(List<? extends Animal> list) {
//备注：如果用 List<Animal> 作为形参列表，是无法传入List<Cat>for (Animal animal : list) {
            animal.eat();
        }
    }
}
```

![复制代码](0.3603331845114273-20220901151245-hb6kkri.png)[&quot;复制代码&quot;]("复制代码")

　　act(List<？ extends Animal> list),当中“？”就是通配符，而“？ extends Animal”则表示通配符“？”的上界为Animal，换句话说就是，“？ extends Animal”可以代表Animal或其子类，可代表不了Animal的父类（如Object），因为通配符的上界是Animal。

#### 所以，泛型内是不存在父子关系，但是利用通配符可以产生类似的效果：

假设给定的泛型类型为G，(如List<E>中的List),两个具体的泛型参数X、Y，当中Y是X的子类（如上的Animal和Cat））

* G<? extends Y> 是 G<? extends X>的子类型（如List<? extends Cat> 是 List<? extends Animal>的子类型）。
* G<X> 是 G<? extends X>的子类型（如List<Animal> 是 List<? extends Animal>的子类型）
* G<?> 与 G<? extends Object>等同，如List<?> 与List<? extends Objext>等同

[回到顶部](https://www.cnblogs.com/ldh-better/p/7127308.html#_labelTop)

## 三： 泛型可以用到那些地方

　　　　泛型可以用到容器，方法，接口，内部类，抽象类

[回到顶部](https://www.cnblogs.com/ldh-better/p/7127308.html#_labelTop)

## 四： Java中泛型独特之处

　　　　泛型是Java1.5之后才引入的，为了兼容。Java采用了C++完全不同的实现思想。Java中的泛型更多的看起来像是编译期用的，比如我定义一个使用泛型的demo

我在查看它的class文件时，发现class文件并没有任何泛型信息。

### Java会在编辑期把泛型擦除掉

　　在JAVA的虚拟机中并不存在泛型，泛型只是为了完善java体系，增加程序员编程的便捷性以及安全性而创建的一种机制，在JAVA虚拟机中对应泛型的都是确定的类型，在编写泛型代码后，java虚拟中会把这些泛型参数类型都擦除，用相应的确定类型来代替，代替的这一动作叫做类型擦除，而用于替代的类型称为原始类型，在类型擦除过程中，一般使用第一个限定的类型来替换，若无限定，则使用Object.

### 擦除的原理以及边界

　　关键在于从泛型类型中清除类型参数的相关信息，并且再必要的时候添加类型检查和类型转换的方法。

　　可以参考[Java泛型－类型擦除](http://blog.csdn.net/caihaijiang/article/details/6403349)。 运行期编译期会去掉泛型信息，转换为左边界，在调用的地方添加类型转换。

### 泛型擦除肯可能导致的问题

#### 用泛型不可以区分方法签名

![复制代码](0.9163779797802771-20220901151245-y1ps3hi.png)[&quot;复制代码&quot;]("复制代码")

```
public void test(List<String> ls){
                System.out.println("Sting");
            }
            public void test(List<Integer> li){
                System.out.println("Integer");
            }
//这回报错，编译期无法区分这两个方法
```

![复制代码](0.9416953399031348-20220901151245-5a6jt5j.png)[&quot;复制代码&quot;]("复制代码")

**泛型类的静态变量是共享**

![复制代码](0.16604732966787017-20220901151245-v4abkfc.png)[&quot;复制代码&quot;]("复制代码")

```
public class StaticTest{
    public static void main(String[] args){
        GT<Integer> gti = new GT<Integer>();
        gti.var=1;
        GT<String> gts = new GT<String>();
        gts.var=2;
        System.out.println(gti.var);
    }
}
class GT<T>{
    public static int var=0;
    public void nothing(T x){}
}
```

![复制代码](0.35198202505342546-20220901151245-cpy99fq.png)[&quot;复制代码&quot;]("复制代码")

[回到顶部](https://www.cnblogs.com/ldh-better/p/7127308.html#_labelTop)

## 五： 泛型中特殊使用

　　　java中的泛型不只是上述说的内容，还有一些特殊的地方，如果这些地方也用泛型该怎么设计。比如说“动态类型”，“潜在类型”，“异常”

### 程序如果运行时需要类型信息

　　就在调用的地方传入类型信息

### 异常中使用泛型

　　不能抛出也不能捕获泛型类的对象。事实上，泛型类扩展Throwable都不合法，因为泛型信息会被擦除，相当于catch两个相同的异常，是不可以的

### 数组与泛型

　　不能声明参数化类型的数组， 数组可以记住自己的元素类型，不能建立一个泛型数组。（当然 你如果用反射还是可以创建的，用Array.newInstance。这里说不能建是不能用普通方法）

### 泛型的一些其他细节：　　

　　1.基本类型无法作为类型参数即ArrayList<int>这样的代码是不允许的，如果为我们想要使用必须使用基本类型对应的包装器类型ArrayList<Integer>

　　2.在泛型代码内部，无法获得任何有关泛型参数类型的信息换句话说，如果传入的类型参数为T，即你在泛型代码内部你不知道T有什么方法，属性，关于T的一切信息都丢失了（类型信息，博文后续）。

　　3.注，在能够使用泛型方法的时候，尽量避免使整个类泛化。

[回到顶部](https://www.cnblogs.com/ldh-better/p/7127308.html#_labelTop)

## 六：简单概括

　　虚拟机中没有泛型，只有普通类和普通方法

　　所有泛型类的类型参数在编译时都会被擦除

　　创建泛型对象时请指明类型，让编译器尽早的做参数检查

　　要忽略编译器的警告信息，那意味着潜在的ClassCastException等着你。

作者：[leader_Hoo](http://www.cnblogs.com/ldh-better/)  
出处：[http://www.cnblogs.com/ldh-better/](http://www.cnblogs.com/ldh-better/)  
本文版权归作者和博客园共有，欢迎转载，唯一要求的就是请注明转载，此外，如果博客有帮助，希望可以帮忙点击推荐和分享，谢谢

来源： [https://www.cnblogs.com/ldh-better/p/7127308.html](https://www.cnblogs.com/ldh-better/p/7127308.html)
