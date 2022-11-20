#easyDb 使用说明

###1. 设计结构
WorkBook: this is use as an database  ,it provide a WorkBook object;
DataTable: this is user as an datatable ,it provide a worksheet object;
SelectAble:  it is  a selector , u can use this to retrieve you data;
EditeAble: it is a editor, if u want to modeify your data,you need it;
DataSourceScanner:it is a  annotation scanner,it's provide static method connectSheet;


##2. 功能

 I  write this tools which can use a Excel file to use as a database base on apache poi,but only support 1 thread ,it means it is only can one user to use;
   so it's useless, just for fun. this project use an excel file to be an data base, and worksheet as a table;  it accomplished the function of create retrieve update and delete(CRUD);
  Because of time constraints, i use rowIndex as primary key of easyDb; 
  this project use a mass of java reflect; 

## 用法
###配置
1. 注册数据源
新建类实现datasourceconfig 接口，重写DataSourcef方法即可；
DataSource()： 返回工作簿路径即可；目前只支持xls格式的工作簿；
tips:
多个工作簿需要多个类，注册；

```java
@Repository//在spring中该类放在dao层中，可以配上该注解
public class Demo  implements DataSourceConfig {
    @Override
    public String DataSource() {
        return "C:/dev/demo.xls";
    }
}
```

2. 建立实体类；
```java

@SheetInfo(sheetIndex = 0 ,beginRowIndex = 1)
public class Company {

    @Column(index = "A")
    private String name;

```
使用@sheetInfo 标注你的entity 类，sheetinfo 有两个属性，sheetIndex(工作表的索引值)和beginRowIndex(数据正式开始的坐标);
使用@Column 标注entity的域，column 注解有index，用index 赋上工作表对应列的列索引（A、a），大小写不敏感；

3. 注册工作表
在刚才的数据源中可以注册该工作簿所包含的工作表
写一个返回值为DataTable 的方法即可
```java
    @Bean(name="company")//使用spring 框架时可以将这个方法注册成bean；
    public DataTable company(){
        return DataSourceScanner.connectSheet(new Company(),this);
    }
```
```java
    /*利用反射获取注册类的 sheetInfo注解和column注解 */
    public static DataTable connectSheet(Object obj , DataSourceConfig data){
        Class<?> clazz =  obj.getClass();
        SheetInfo sheetInfo = clazz.getAnnotation(SheetInfo.class);
        Integer sheetIndex = sheetInfo.sheetIndex();
        if(sheetIndex==null){
            throw  new NullPointerException(clazz.getTypeName()+"需要配置sheetIndex");
        }
        Field[] fields = clazz.getDeclaredFields();
        Map map=new HashMap<Integer,String>();
        for(Field field:fields){
            Column hl = field.getAnnotation(Column.class);
            map.put(transfer(hl.index()),field.getName());
        }
        return new Sheet(sheetInfo.beginRowIndex(),sheetIndex,sheetIndex,data.DataSource(),map,clazz);
    }
```
截至目前excle数据库就配置好了，在配置无误的情况下就现在可以开始开始使用；

###使用
easydb 实现数据的增删该查依赖于两个接口；selectAble 和Editable;
由于时间的关系，还有其他许多查询功能没有实现；
已实现的简单功能有：
单表的数据查询 like 、equals、
单表的增删改；
具体接口如下；
####EditAble;
```java
public interface EditAble<T> {
    /*
     * @Desc 所有的编辑操作最后都需要通过该方法写入数据库中；
     **/
    boolean write();

    /*
     * @Desc 将单个对象放入数据库最后一行
     **/
    boolean putObj(T t);

    /*
     * @Desc 将对象集合放入数据中
     **/
    int putObjBatch(List<T> t);

    /*
     * @Desc 修改制定行的数据
     **/
    boolean postObj(int index, T t);

    /*
     * @Desc 批量修改数据
     **/
    String postObjBatch(DataWrapper<T> wrapper);

    /*
     * @Desc 删除指定行的数据
     **/
    boolean del(int index);

    /*
     * @Desc 批量删除数据
     **/
    int delBatch(int[] batch);
    
```

####SelectAble 
```java
      /*
     * @Desc 获取查询的结果集合，查询完成之后调用
     **/
    List getResultList();
      /*
     * @Desc 获取带行号的结果
     **/
    DataWrapper getResultWrapper();
      /*
     * @Desc 输入属性名和查询条件，可以输入多个条件，只要其中一个满足与条件相等即可查询出结果；
     *  select *from table where fieldName in(condition1,...conditionN;)
     **/
    SelectAble eq(String fieldName, String... conditions);
      /*
     * eq 方法的重载，可以输入对应工作表相应字段的索引（A：0，B：1）
     **/
    SelectAble eq( int fieldIndex,String ...conditions);
          /*
     * @Desc 输入属性名和查询条件，可以输入多个条件，只要其中一个满足与条件相等即可查询出结果；
     *  select *from table where fieldName like (condition1,...conditionN;)
     **/
    SelectAble like( String  fieldName,String ...conditions);
    SelectAble like( int fieldIndex,String ...conditions);
``` 
其中DataWrapper<T> 是数据的包装器，主要用来存储行数和数据的映射；
后期可以在此基础上实现缓存功能等；
```java

public class DataWrapper<T> {
    private T t; 
    private Integer fieldIndex;
    private Map<Integer,T> dataMap;
    public void add(Integer key, T t){
        this.dataMap.put(key,t);
    }
```

##测试
``` java 
  public String testGetWrapper(){
        DataSourceConfig cd=new Demo();
        DataTable db = data.company();//数据注册器内实现
        db.getSelector().like(0,"欣").like("city","市","山").getResultWrapper();
        return "";
    }
```

```所有接口采用了jmeter 进行接口测试，单文件单表单线程测试；


