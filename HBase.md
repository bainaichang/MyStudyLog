一行叫 rowkey,一行对应着多个列族(Map)

```xml
table => List<rowkey(下面那个)>
rowkey => List<HashMap>
ColumnFamily:ColumnName => HashMap
```

一个表有多个行,一行有多个列族 (如果不好理解的话结合下面的示例进行辅助理解)

![image-20250124123526375](C:\Users\白乃常\AppData\Roaming\Typora\typora-user-images\image-20250124123526375.png)

创建表

```ruby
create "表名","列族名1","列族名2";
# decribe 'table_name' 查看表信息
```

删除表

```ruby
# 先禁用表,后删除
disable "name"
drop "name"
```

PUT操作

```ruby
put '表名','ROWKEY','列族名1:列名','值1'
# "rowkey"类似于主键
```

GET操作

```ruby
get "表名","rowkey"
# 获取这个主键对应的所有值
```

查询显示中文

```ruby
get "tablename","rowkey",{FORMATTER => 'toString'}
```

更新数据操作

```ruby
# 用put来进行更新,语法一致,时间戳会更新
put '表名','ROWKEY','列族名1:列名','New值1'
```

DELETE删除操作

```ruby
# 删除特定数据
delete 'tablename','rowkey','列族名1:列名'
```

delete all操作

```ruby
# 用于删除整行数据
deleteall 'tablename','rowkey'
```

但是实际操作中一般都是假删除,给行添加一个列为状态,标志是否被删除

注意,delete删除操作是删除最新版本的数据,会把上一个版本的数据显示出来

delete all是删除所有





批量插入数据,读取文件的指令进行插入

```shell
hbase shell /data/hbase_data.txt
```

COUNT操作 --查看表中有多少条记录

```ruby
count "tablename"
```

SCAN操作 --查询表

```ruby
# 显示3条数据
scan 'tablename',{LIMIT => 3}
# 指定列名查询
scan 'tbName',{COLUMNS => ['列族名1:列名1','列族名1:列名2']}
# 指定rowkey
scan 'tablename',{ROWPREFIXFILTER => 'rowkey'}
```

过滤器

```ruby
scan 'tbName',{Filter => "过滤器名(操作符,比较器类型:值)"}
```

incr计数器

```ruby
# 用put创建的列是不实现累加的
incr 'tbName','rowkey','列族:列名',累加值(默认为 1)
```

alter操作

```ruby
# 改变表和列族结构
#新增列
alter 'tbName','new 列名'
#删除列
alter 'tbName','delete' => '列名'
```

Maven依赖

```xml
<dependencies>
    <!-- https://mvnrepository.com/artifact/org.apache.hbase/hbase-server -->
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-server</artifactId>
        <version>2.0.2</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.apache.hbase/hbase-client -->
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-client</artifactId>
        <version>2.0.2</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
    <dependency>
        <groupId>org.apache.zookeeper</groupId>
        <artifactId>zookeeper</artifactId>
        <version>3.4.6</version>
        <type>pom</type>
    </dependency>
</dependencies>

```

Java创建表 API

```java
public class Main {
    public static Configuration conf = HBaseConfiguration.create();
    public static Connection conn;
    public static Admin admin;
    @Before
    public void Init(){
        conf.set("hbase.zookeeper.quorum", "localhost");
        conf.set("hbase.zookeeper.property.clientPort", "2181");
        try {
            conn = ConnectionFactory.createConnection(conf);
            admin = conn.getAdmin();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
    @After
    public void end(){
        try {
            conn.close();
            admin.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    @Test
    public void createTable() throws Exception {
        TableName tableName = TableName.valueOf("order_info_01");
        if (admin.tableExists(tableName)) {
            System.out.println("表已存在!");
            return;
        }
        // 表描述器建造对象
        TableDescriptorBuilder tableDescriptorBuilder = TableDescriptorBuilder.newBuilder(tableName);
        // 列族描述器建造对象
        ColumnFamilyDescriptorBuilder columnFamilyDescriptorBuilder = ColumnFamilyDescriptorBuilder.newBuilder(Bytes.toBytes("c1"));
        // 真正的表描述器
        ColumnFamilyDescriptor columnFamilyDescriptor = columnFamilyDescriptorBuilder.build();
        // 将列族和表联系起来
        tableDescriptorBuilder.setColumnFamily(columnFamilyDescriptor);
        // 真正的表描述器
        TableDescriptor tableDescriptor = tableDescriptorBuilder.build();
        // 开始创建表
        admin.createTable(tableDescriptor);
    }
}
```

插入数据操作

```java
@Test
public void insertData() throws Exception {
    Table table = conn.getTable(TableName.valueOf("order_info_01"));
    String rowkey = "0001";
    // 列族名
    String columnFamily = "c1";
    // 列名
    String columnName = "name";
    // 根据rowkey添加
    Put put = new Put(Bytes.toBytes(rowkey));
    // 列族,列名,值
    put.addColumn(Bytes.toBytes(columnFamily),Bytes.toBytes(columnName),Bytes.toBytes("小王"));
    table.put(put);
    // 注意,Htable是一个非线程安全的对象
    table.close();
}
```

根据rowkey查询数据

```java
@Test
public void getData() throws Exception {
    Table table = conn.getTable(TableName.valueOf("order_info_01"));
    // 根据rowkey拿到列族
    Get get = new Get(Bytes.toBytes("0003"));
    Result result = table.get(get);
    List<Cell> cells = result.listCells();
    StringBuilder sb = new StringBuilder();
    String rowkey = Bytes.toString(result.getRow());
    sb.append(rowkey).append(": { ");
    for (Cell cell : cells) {
        // 获得列族的名字
        String s = Bytes.toString(cell.getFamilyArray(), cell.getFamilyOffset(), cell.getFamilyLength());
        // 获得列名
        String c = Bytes.toString(cell.getQualifierArray(), cell.getQualifierOffset(), cell.getQualifierLength());
        // 获得值
        String v = Bytes.toString(cell.getValueArray(), cell.getValueOffset(), cell.getValueLength());
        sb.append(s).append(":").append(c).append("->").append(v);
    }
    sb.append(" }");
    System.out.println(sb.toString());
    table.close();
}
```

scan全表查询(需要配合过滤器)

```java
@Test
public void scanTest() throws Exception{
    Table table = conn.getTable(TableName.valueOf("order_info_01"));
    Scan scan = new Scan();
    // FilterList 多个过滤条件
    ValueFilter valueFilter = new ValueFilter(CompareOperator.EQUAL,new SubstringComparator("Bot"));
    scan.setFilter(valueFilter);
    ResultScanner resultScanner = table.getScanner(scan);
    for (Result result : resultScanner) {
        for (Cell cell : result.listCells()) {
            // 列族名
            String s = Bytes.toString(cell.getFamilyArray(), cell.getFamilyOffset(), cell.getFamilyLength());
            // 获得列名
            String c = Bytes.toString(cell.getQualifierArray(), cell.getQualifierOffset(), cell.getQualifierLength());
            // 获得值
            String v = Bytes.toString(cell.getValueArray(), cell.getValueOffset(), cell.getValueLength());
            System.out.println(Bytes.toString(result.getRow()) + "{" + s + ":" + c + "->" + v + "}");
        }
    }
    table.close();
}
```

命名空间: 用于批量存放表

Hbase有两个默认命名空间 default(你创建的表),hbase(系统表)

```ruby
create_namespace 'name'
list_namespace
drop_namespace 'name'
describe_namespace 'name'
create 'namespace:table', '列族1'
```

LSM-Tree

