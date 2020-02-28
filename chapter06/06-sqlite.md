# Sqlite

![sqlite](https://sqlite.org/images/sqlite370_banner.gif)

[SQLite](https://sqlite.org/index.html)是一种开源，零配置，独立的，独立的，事务关系数据库引擎，旨在嵌入到应用程序中，具备如下特性：

- SQLite不需要一个单独的服务器进程或系统操作（服务器）。
- SQLite 不需要配置，这意味着它不需要安装或管理。
- 一个完整的SQLite数据库可存储在跨平台的磁盘文件中。
- SQLite是非常小，重量轻，小于400KB完全配置或小于250KB的省略可选功能。
- SQLite是自配置的，独立的，这意味着它不需要任何外部的依赖。
- SQLite的交易是完全符合ACID，允许多个进程或线程安全访问。
- SQLite支持大多数（SQL2）符合SQL92标准的查询语言功能。
- SQLite是在ANSI-C编写的，并提供了简单和易于使用的API。
- SQLite可在UNIX（在Linux，Mac OS-X，Android，IOS）和Windows（Win32中，WINCE，WinRT的）中运行。



## 安装与配置

SQLite以其零配置而闻名，所以不需要复杂的设置或管理。可以使用如下命令安装：

```bash 
$ sudo apt-get install sqlite3
```

使用如下命令开始使用：

```bash
$ sqlite3 
SQLite version 3.22.0 2018-01-22 18:45:57
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite> 
```

现在就可以在这里执行SQLite查询。 但是在这里，数据是暂时的，一旦你关闭了电脑，就将失去操作过的所有数据记录。因为使用这种方法不能创建，附加或分离数据库。



## 快速入门

SQLite命令不区分大小写。但是，有一些区分大小写的命令。例如：`GLOB`和`glob`在SQLite语句中有不同的含义。注释以两个连续的“ `-` ”字符，也可使用“`/*`”字符开始，并延伸至下一个“`*/`”字符对所包括的内容视为注释。



所有的SQLite语句都是以关键字(如：`SELECT`，`INSERT`，`UPDATE`，`DELETE`，`ALTER`，`DROP`等)开始的。所有语句都以分号(`;`)结尾。

**SQLite ANALYZE语句的语法：**

```sql
ANALYZE;  
-- or  
ANALYZE database_name;  
-- or  
ANALYZE database_name.table_name;
```

**AND/OR子句的语法：**

```sql
SELECT column1, column2....columnN  
FROM   table_name  
WHERE  CONDITION-1 {AND|OR} CONDITION-2;
```

**ALTER TABLE语句的语法**

```sql
ALTER TABLE table_name ADD COLUMN column_def...;
SQL
```

**ALTER TABLE语句(Rename)语句的语法**

```sql
ALTER TABLE table_name RENAME TO new_table_name;
```

** ATTACH DATABASE语句的语法：**

```sql
ATTACH DATABASE 'DatabaseName' As 'Alias-Name';
```

**BEGIN TRANSACTION语句的语法：**

```sql
BEGIN;  
-- or  
BEGIN EXCLUSIVE TRANSACTION;
```

**BETWEEN语句的语法：**

```sql
SELECT column1, column2....columnN  
FROM   table_name  
WHERE  column_name BETWEEN val-1 AND val-2;  
SQLite COMMIT Statement:  
COMMIT;
```

**CREATE INDEX语句的语法：**

```sql
CREATE INDEX index_name  
ON table_name ( column_name COLLATE NOCASE );
```

**CREATE UNIQUE INDEX语句的语法：**

```sql
CREATE UNIQUE INDEX index_name  
ON table_name ( column1, column2,...columnN);
```

**CREATE TABLE语句的语法：**

```sql
CREATE TABLE table_name(  
   column1 datatype,  
   column2 datatype,  
   column3 datatype,  
   .....  
   columnN datatype,  
   PRIMARY KEY( one or more columns ));
```

**CREATE TRIGGER语句的语法：**

```sql
CREATE TRIGGER database_name.trigger_name   
BEFORE INSERT ON table_name FOR EACH ROW  
BEGIN   
   stmt1;   
   stmt2;  
   ....  
END;
```

**CREATE VIEW语句的语法：**

```sql
CREATE VIEW database_name.view_name  AS  
SELECT statement....;
```

**CREATE VIRTUAL TABLE语句的语法：**

```sql
CREATE VIRTUAL TABLE database_name.table_name USING weblog( access.log );  
-- or  
CREATE VIRTUAL TABLE database_name.table_name USING fts3( );
```

**COMMIT TRANSACTION语句的语法：**

```sql
COMMIT;
```

**COUNT语句的语法：**

```sql
SELECT COUNT(column_name)  
FROM   table_name  
WHERE  CONDITION;
```

**DELETE语句的语法：**

```sql
DELETE FROM table_name  
WHERE  {CONDITION};
```

**DETACH DATABASE语句的语法：**

```sql
DETACH DATABASE 'Alias-Name';
```

**DISTINCT语句的语法：**

```sql
SELECT DISTINCT column1, column2....columnN  
FROM   table_name;
```

**DROP INDEX语句的语法：**

```sql
DROP INDEX database_name.index_name;
```

**DROP TABLE语句的语法：**

```sql
DROP TABLE database_name.table_name;
```

**DROP VIEW语句的语法：**

```sql
DROP INDEX database_name.view_name;
```

**DROP TRIGGER 语句的语法：**

```sql
DROP INDEX database_name.trigger_name;
```

**EXISTS语句的语法：**

```sql
SELECT column1, column2....columnN  
FROM   table_name  
WHERE  column_name EXISTS (SELECT * FROM   table_name );
```

**EXPLAIN语句的语法：**

```sql
EXPLAIN INSERT statement...;  
-- or   
EXPLAIN QUERY PLAN SELECT statement...;
```

**GLOB语句的语法：**

```sql
SELECT column1, column2....columnN  
FROM   table_name  
WHERE  column_name GLOB { PATTERN };
```

**GROUP BY语句的语法：**

```sql
SELECT SUM(column_name)  
FROM   table_name  
WHERE  CONDITION  
GROUP BY column_name;
```

**HAVING语句的语法：**

```sql
SELECT SUM(column_name)  
FROM   table_name  
WHERE  CONDITION  
GROUP BY column_name  
HAVING (arithematic function condition);
```

**INSERT INTO语句的语法：**

```sql
INSERT INTO table_name( column1, column2....columnN)  
VALUES ( value1, value2....valueN);
```

**IN语句的语法：**

```sql
SELECT column1, column2....columnN  
FROM   table_name  
WHERE  column_name IN (val-1, val-2,...val-N);
```

**Like语句的语法：**

```sql
SELECT column1, column2....columnN  
FROM   table_name  
WHERE  column_name LIKE { PATTERN };
```

**NOT IN语句的语法：**

```sql
SELECT column1, column2....columnN  
FROM   table_name  
WHERE  column_name NOT IN (val-1, val-2,...val-N);
```

**ORDER BY语句的语法：**

```sql
SELECT column1, column2....columnN  
FROM   table_name  
WHERE  CONDITION  
ORDER BY column_name {ASC|DESC};
```

**PRAGMA语句的语法：**

```sql
PRAGMA pragma_name;
```

有关`pragma`的几个示例：

```sql
PRAGMA page_size;  
PRAGMA cache_size = 1024;  
PRAGMA table_info(table_name);
```

**RELEASE SAVEPOINT语句的语法：**

```sql
RELEASE savepoint_name;
```

**REINDEX语句的语法：**

```sql
REINDEX collation_name;  
REINDEX database_name.index_name;  
REINDEX database_name.table_name;
```

**ROLLBACK语句的语法：**

```sql
ROLLBACK;  
-- or  
ROLLBACK TO SAVEPOINT savepoint_name;
```

**SAVEPOINT语句的语法：**

```sql
SAVEPOINT savepoint_name;
```

**SELECT语句的语法：**

```sql
SELECT column1, column2....columnN  
FROM   table_name;
```

**UPDATE语句的语法：**

```sql
UPDATE table_name  
SET column1 = value1, column2 = value2....columnN=valueN  
[ WHERE  CONDITION ];
```

**VACUUM语句的语法：**

```sql
VACUUM;  
SQLite WHERE Clause:  
SELECT column1, column2....columnN  
FROM   table_name  
WHERE  CONDITION;
```

如果你学习过关系型数据的话，上面的语法都是很熟悉的，很简单，以后我们使用的PostgreSQL的基本SQL语法大部分也差不多。


好啦，基于以上的介绍，结合sqlite3的帮助，我们来简单实验下吧。

### 创建数据库

数据库名的后缀你可以直接指定，甚至没有后缀都可以：

```bash
$ mkdir chapter06
$ cd chapter06
$ sqlite3 sqlite-demo.db
SQLite version 3.22.0 2018-01-22 18:45:57
sqlite> create table test(id interger, name string);
sqlite> .exit
$ ls
sqlite-demo.db
```

我们以`sqlite-demo.db`为名字，创建了一个数据库，同时在这个库中创建了一个有两个字段的表`test`。数据库文件后缀无所谓，随便起都可以。简单介绍就这样吧！

## 开始编码

### 创建工程并准备数据

```bash
$ cargo new --bin gprl-sqlite3-demo
$ cd gprl-sqlite3-demo
$ sqlite3 gprl-sqlite3-demo.db
SQLite version 3.22.0 2018-01-22 18:45:57
Enter ".help" for usage hints.
sqlite> create table my_demo_apps (id integer, name text, version text, authors text);
sqlite> insert into my_demo_apps (id, name, version, authors) values (1,"gprl-sqlite3-demo","0.1.0", "Luo Xiangyong <luoxiangyong@topgridcloud.com>");
sqlite> insert into my_demo_apps (id, name, version, authors) values (2,"gprl-redis-demo","0.1.0", "Luo Xiangyong <luoxiangyong@topgridcloud.com>");
sqlite> insert into my_demo_apps (id, name, version, authors) values (3,"gprl-lib-demo","0.1.3", "Luo Xiangyong <luoxiangyong@topgridcloud.com>");
sqlite> select * from my_demo_apps;
1|gprl-sqlite3-demo|0.1.0|Luo Xiangyong <luoxiangyong@topgridcloud.com>
2|gprl-redis-demo|0.1.0|Luo Xiangyong <luoxiangyong@topgridcloud.com>
3|gprl-lib-demo|0.1.3|Luo Xiangyong <luoxiangyong@topgridcloud.com>
sqlite> .exit
```

我们创建了一个数据库，在其中建立了一个叫`my_demo_apps`表，同时往里面添加了三条数据。

crates.io上关于sqlite使用的比较多的crate有`sqlite`,`rusqlite`,`diesel`等。

#### 使用sqlite crate

下面开始编码测试下吧：

```toml
[package]
name = "gprl-sqlite3-demo"
version = "0.1.0"
authors = ["Luo Xiangyong <luoxiangyong@topgridcloud.com>"]
edition = "2018"

[dependencies]
sqlite = "0.25.0"
```

```rust
extern crate sqlite;

fn main() {
    let connection = sqlite::open("gprl-sqlite3-demo.db").unwrap();
    
    println!("修改前的数据为：");
    connection.iterate("SELECT * FROM my_demo_apps", |pairs| {
        for &(column, value) in pairs.iter() {
            print!("{} = {} | ", column, value.unwrap());
        }
        println!("");
        true
    }).unwrap();
    
    connection.execute("update my_demo_apps set authors="\祥勇\"　where id=1;").unwrap();
    
    println!("修改后的数据为：");
    connection.iterate("SELECT * FROM my_demo_apps", |pairs| {
        for &(column, value) in pairs.iter() {
            print!("{} = {} | ", column, value.unwrap());
        }
        println!("");
        true
    }).unwrap();
}
```

运行结果为：

```bash
$ cargo run
修改前的数据为：
id = 1 | name = gprl-sqlite3-demo | version = 0.1.0 | authors = 罗 | 
id = 2 | name = gprl-redis-demo | version = 0.1.0 | authors = Luo Xiangyong <luoxiangyong@topgridcloud.com> | 
id = 3 | name = gprl-lib-demo | version = 0.1.3 | authors = Luo Xiangyong <luoxiangyong@topgridcloud.com> | 
修改后的数据为：
id = 1 | name = gprl-sqlite3-demo | version = 0.1.0 | authors = 祥勇 | 
id = 2 | name = gprl-redis-demo | version = 0.1.0 | authors = Luo Xiangyong <luoxiangyong@topgridcloud.com> | 
id = 3 | name = gprl-lib-demo | version = 0.1.3 | authors = Luo Xiangyong <luoxiangyong@topgridcloud.com> | 
```

代码比较简单，直观，不做多解释。

#### 使用rusqlite crate



```rust
use rusqlite::{params, Connection, Result};
use time::Timespec;

#[derive(Debug)]
struct Person {
    id: i32,
    name: String,
    time_created: Timespec,
    data: Option<Vec<u8>>,
}

fn main() -> Result<()> {
    let conn = Connection::open_in_memory()?;

    conn.execute(
        "CREATE TABLE person (
                  id              INTEGER PRIMARY KEY,
                  name            TEXT NOT NULL,
                  time_created    TEXT NOT NULL,
                  data            BLOB
                  )",
        params![],
    )?;
    let me = Person {
        id: 0,
        name: "Steven".to_string(),
        time_created: time::get_time(),
        data: None,
    };
    conn.execute(
        "INSERT INTO person (name, time_created, data)
                  VALUES (?1, ?2, ?3)",
        params![me.name, me.time_created, me.data],
    )?;

    let mut stmt = conn.prepare("SELECT id, name, time_created, data FROM person")?;
    let person_iter = stmt.query_map(params![], |row| {
        Ok(Person {
            id: row.get(0)?,
            name: row.get(1)?,
            time_created: row.get(2)?,
            data: row.get(3)?,
        })
    })?;

    for person in person_iter {
        println!("Found person {:?}", person.unwrap());
    }
    Ok(())
}
```

