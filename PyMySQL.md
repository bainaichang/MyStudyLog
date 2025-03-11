连接mysql

```py
conn = pymysql.connect(host="localhost", user="root", password="147qaz147", database="pytest")
cursor = conn.cursor()

```

使用多行插入

```py
import pymysql

conn = pymysql.connect(host="localhost", user="root", password="147qaz147", database="pytest")
cursor = conn.cursor()
# sql = 'select * from purchase_order'
sql1 = '''
INSERT INTO purchase_order (supplier_name, total_amount) VALUES
(%s, %s);
'''
sql2 = '''select * from purchase_order'''
try:
    data = [('供应商A',500.00),('供应商B',300.00),('供应商C',100.00)]
    cursor.executemany(sql1,data)
    conn.commit()
    cursor.execute(sql2)
    res = cursor.fetchall()
    for row in res:
        print(row)
except:
    print("error!")
    conn.rollback()
conn.close()

```

