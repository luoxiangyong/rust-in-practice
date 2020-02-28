# PostgreSQL

![PostgreSQL](https://bkimg.cdn.bcebos.com/pic/9e3df8dcd100baa1613b79b74a10b912c8fc2ea2?x-bce-process=image/resize,m_lfit,w_268,limit_1/format,f_jpg)

[PostgreSQL](https://www.postgresql.org/)是一个功能强大的开源对象关系型数据库系统，他使用和扩展了SQL语言，并结合了许多安全存储和扩展最复杂数据工作负载的功能。PostgreSQL的起源可以追溯到1986年，作为加州大学伯克利分校POSTGRES项目的一部分，并且在核心平台上进行了30多年的积极开发。

PostgresSQL凭借其经过验证的架构，可靠性，数据完整性，强大的功能集，可扩展性以及软件背后的开源社区的奉献精神赢得了良好的声誉，以始终如一地提供高性能和创新的解决方案。PostgreSQL在所有主要操作系统都可使用。


## 安装与配置

Ubuntu的默认存储库包含Postgres软件包，因此您可以使用`apt`打包系统安装这些软件包。

```bash
sudo apt install postgresql postgresql-contrib
```

现在安装了该软件，我们可以了解它的工作原理以及它可能与您可能使用的类似数据库管理系统的不同之处。

###  使用PostgreSQL角色和数据库

默认情况下，Postgres使用称为“角色”的概念来处理身份验证和授权。这些在某些方面类似于普通的Unix风格的账户，但是Postgres并没有区分用户和组，而是倾向于更灵活的术语“角色”。

安装后，Postgres被设置为使用*ident身份*验证，这意味着它将Postgres角色与匹配的Unix / Linux系统帐户相关联。如果Postgres中存在一个角色，则具有相同名称的Unix / Linux用户名可以作为该角色登录。

安装过程创建了一个名为**postgres**的用户帐户，它与默认的Postgres角色相关联。为了使用Postgres，您可以登录到该帐户。

有几种方式可以使用此帐户访问Postgres。

### 切换到postgres帐户

输入以下内容切换到服务器上的**postgres**帐户：

```undefined
sudo -i -u postgres
```

您现在可以通过键入以下命令立即访问Postgres提示符：

```undefined
psql
```

这会将您登录到PostgreSQL提示符中，从这里您可以立即与数据库管理系统进行交互。

输入以下命令退出PostgreSQL提示符：

```undefined
\q
```

这会将您带回到`postgres`Linux命令提示符。

### 在不切换帐户的情况下访问Postgres提示

你也可以直接用**postgres**账户运行你想要的命令`sudo`。

例如，在最后一个例子中，您被指示通过首先切换到**postgres**用户然后运行`psql`以打开Postgres提示符来访问Postgres提示符。您可以通过`psql`像**postgres**用户`sudo`一样运行单个命令来完成此操作，如下所示：



```undefined
sudo -u postgres psql
```

这会将你直接登录到Postgres中，而不需要中间的`bash`shell。

同样，您可以键入以下命令退出交互式Postgres会话：

```undefined
\q
```

许多用例需要多个Postgres角色。

### 创建一个新的角色

目前，您只需在数据库中配置**postgres**角色。您可以使用命令从命令行创建新角色`createrole`。该`--interactive`标志会提示您输入新角色的名称，并询问它是否应具有超级用户权限。

如果您以**postgres**帐户登录，则可以通过键入以下内容创建新用户：



```undefined
createuser --interactive
```

相反，如果您宁愿使用`sudo`每个命令而不从普通帐户切换，请键入：

```undefined
sudo -u postgres createuser --interactive
```

该脚本会提示您一些选择，并根据您的响应执行正确的Postgres命令以根据您的规范创建用户。

Output

```csharp
Enter name of role to add: sammyShall the new role be a superuser? (y/n) y
```

通过传递一些额外的标志可以获得更多控制权。通过查看`man`页面查看选项：

```undefined
man createuser
```

你现在安装的Postgres有一个新用户，但你还没有添加任何数据库。

## 快速入门

### 创建一个新的数据库

Postgres身份验证系统默认情况下的另一个假设是，对于用于登录的任何角色，该角色将具有可访问的同名数据库。

这意味着，如果您在上一节中创建的用户名为**sammy**，则该角色将尝试连接到默认情况下也称为“sammy”的数据库。您可以使用该`createdb`命令创建适当的数据库。

如果您以**postgres**帐户登录，则可以键入如下所示的内容：



```undefined
createdb sammy
```

相反，如果您宁愿使用`sudo`每个命令而无需从普通帐户中切换，则可以键入：

```undefined
sudo -u postgres createdb sammy
```

这种灵活性可根据需要为创建数据库提供多种路径。

### 以新的角色打开Postgres提示

要使用`ident`基于身份验证的身份登录，您需要一名与Postgres角色和数据库名称相同的Linux用户。

如果您没有可用的匹配Linux用户，则可以使用该`adduser`命令创建一个。您必须从您的非**root用户**帐户拥有`sudo`权限（意思是，未以**postgres**用户身份登录）执行此操作：

```undefined
sudo adduser sammy
```

一旦这个新帐户可用，您可以通过键入以下命令切换并连接到数据库：

```undefined
sudo -i -u sammy
psql
```

或者，你可以这样做内联：

```undefined
sudo -u sammy psql
```

假设所有组件都已正确配置，此命令将自动登录。

如果你想让你的用户连接到不同的数据库，你可以通过像这样指定数据库来实现：

```undefined
psql -d postgres</span
```

登录后，您可以输入以下内容来检查当前的连接信息：

```undefined
\conninfo
```

Output

```csharp
You are connected to database "sammy" as user "sammy" via socket in "/var/run/postgresql" at port "5432".
```

如果您连接到非默认数据库或非默认用户，这很有用。

### 创建和删除表格

现在您已经知道如何连接到PostgreSQL数据库系统，您可以学习一些基本的Postgres管理任务。

首先，创建一个表来存储一些数据。作为一个例子，描述一些游乐场设备的表格。

该命令的基本语法如下所示：



```cpp
CREATE TABLE table_name (    column_name1 col_type (field_length) column_constraints,    column_name2 col_type (field_length),    column_name3 col_type (field_length));
```

正如你所看到的，这些命令为表提供了一个名称，然后定义列以及列类型和字段数据的最大长度。您也可以选择为每列添加表约束。

您可以[在](https://digitalocean.com/community/articles/how-to-create-remove-manage-tables-in-postgresql-on-a-cloud-server)这里了解更多关于[如何在Postgres中创建和管理表格的](https://digitalocean.com/community/articles/how-to-create-remove-manage-tables-in-postgresql-on-a-cloud-server)信息。

为了演示目的，请创建一个如下所示的简单表格：

```php
CREATE TABLE playground (    equip_id serial PRIMARY KEY,    type varchar (50) NOT NULL,    color varchar (25) NOT NULL,    location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),    install_date date);
```

这些命令将创建一个库存游乐场设备的表格。这从一个设备ID开始`serial`。此数据类型是一个自动递增整数。你也给了这个列的约束，`primary key`这意味着这些值必须是唯一的而不是空的。

对于两列（`equip_id`和`install_date`），这些命令不指定字段长度。这是因为某些列类型不需要设置长度，因为该类型隐含了长度。

接下来的两个命令的设备创建列`type`和`color`分别，其中每一个可以不为空。这些命令之后创建一个`location`列并创建一个约束条件，要求该值为八个可能的值之一。最后一个命令创建一个日期列，记录您安装设备的日期。

您可以输入以下内容来查看新表：

```undefined
\d
```

Output

```ruby
                  List of relations Schema |          Name           |   Type   | Owner --------+-------------------------+----------+------- public | playground              | table    | sammy public | playground_equip_id_seq | sequence | sammy(2 rows)
```

你的游乐场桌子在这里，但也有一些叫做`playground_equip_id_seq`这种类型的东西`sequence`。这是`serial`您为`equip_id`专栏提供的类型的表示形式。这会跟踪序列中的下一个数字，并为此类型的列自动创建。

如果你想只看到没有序列的表格，你可以输入：

```undefined
\dt
```

Output

```ruby
          List of relations Schema |    Name    | Type  | Owner --------+------------+-------+------- public | playground | table | sammy(1 row)
```

### 在表中添加，查询和删除数据

现在您已经有了一个表格，您可以在其中插入一些数据。

例如，通过调用要添加的表来添加幻灯片和摆动，命名列，然后为每列提供数据，如下所示：

```bash
INSERT INTO playground (type, color, location, install_date) VALUES ('slide', 'blue', 'south', '2017-04-28');
INSERT INTO playground (type, color, location, install_date) VALUES ('swing', 'yellow', 'northwest', '2018-08-16');
```

输入数据时应小心，以避免几次常见的挂断。例如，不要将列名换成引号，但输入的列值需要引号。

另外要记住的是，您不输入`equip_id`列的值。这是因为无论何时创建表中的新行时都会自动生成这些数据。

通过输入以下内容检索您添加的信息：

```undefined
SELECT * FROM playground;
```

Output

```ruby
 equip_id | type  | color  | location  | install_date ----------+-------+--------+-----------+--------------        1 | slide | blue   | south     | 2017-04-28        2 | swing | yellow | northwest | 2018-08-16(2 rows)
```

在这里，你可以看到你的`equip_id`成功填写以及你所有其他数据的组织都是正确的。

如果操场上的幻灯片发生断裂并且您必须将其删除，则还可以键入以下内容以从表格中删除该行：

```bash
DELETE FROM playground WHERE type = 'slide';
```

再次查询表格：

```undefined
SELECT * FROM playground;
```

Output

```ruby
 equip_id | type  | color  | location  | install_date ----------+-------+--------+-----------+--------------        2 | swing | yellow | northwest | 2018-08-16(1 row)
```

您注意到您的幻灯片不再是表格的一部分。

### 从表中添加和删除列

创建表格后，可以对其进行修改以相对简单地添加或删除列。通过输入以下内容添加一列以显示每件设备的上次维护访问：

```undefined
ALTER TABLE playground ADD last_maint date;
```

如果再次查看表格信息，则会看到已添加新列（但未输入任何数据）：

```undefined
SELECT * FROM playground;
```

Output

```ruby
 equip_id | type  | color  | location  | install_date | last_maint ----------+-------+--------+-----------+--------------+------------        2 | swing | yellow | northwest | 2018-08-16   | (1 row)
```

删除列同样简单。如果您发现工作人员使用单独的工具来跟踪维护历史记录，则可以通过键入以下内容来删除该列：

```undefined
ALTER TABLE playground DROP last_maint;
```

这将删除该`last_maint`列及其中找到的任何值，但保留所有其他数据不变。

### 更新表中的数据

到目前为止，您已经学会了如何向表中添加记录以及如何删除它们，但本教程尚未介绍如何修改现有条目。

您可以通过查询您想要的记录并将列设置为您希望使用的值来更新现有条目的值。您可以查询“摆动”记录（这将匹配表格中的*每个*摆动）并将其颜色更改为“红色”。如果你给挥杆设置一个油漆工作，这可能是有用的：



```bash
UPDATE playground SET color = 'red' WHERE type = 'swing';
```

您可以通过再次查询数据来验证操作是否成功：

```undefined
SELECT * FROM playground;
```

Output

```ruby
 equip_id | type  | color | location  | install_date ----------+-------+-------+-----------+--------------        2 | swing | red   | northwest | 2010-08-16(1 row)
```

正如你所看到的，你的幻灯片现在被注册为红色。

