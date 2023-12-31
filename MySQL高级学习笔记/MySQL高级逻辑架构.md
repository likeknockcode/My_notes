# MySQL逻辑架构

## 1. MySQL逻辑架构图

[![pC6VSde.png](https://s1.ax1x.com/2023/07/06/pC6VSde.png)](https://imgse.com/i/pC6VSde)

## 2. 逻辑架构各部分详解

### 2.1 连接层（Connection Layer）

​		是MySQL与客户端应用程序之间的接口。它处理客户端请求的认证、权限验证以及与客户端之间的通信。连接层接收到客户端的SQL语句，并将其传递给下一层进行处理。

### 2.2 查询解析器（Query Parser）

​		这个组件负责解析接收到的SQL语句，并将其转换为内部数据结构，以便后续的处理。解析器检查语法错误和语义错误，并生成相应的错误信息。

1、词法分析：检测SQL语句的关键字是否正确

2、语法分析：检测SQL语句是否符合MySQL的语法要求，按照MySQl语法规则，生成解析树

3、语义分析：检查解析树是否合法，比如查看表是否存在，列是否存在

### 2.3 优化器（Optimizer）

​		优化器负责对查询进行优化，选择最优的执行计划。它根据查询的复杂度、表的大小、索引等因素来评估可能的执行计划，并选择成本最低的执行计划。优化器使用统计信息和数据库的元数据来进行决策。

### 2.4 执行器（Executor）

​		执行器负责执行查询的执行计划。它与存储引擎进行交互，获取和修改数据。执行器将优化器选择的执行计划转化为具体的操作，然后通过存储引擎提供的API来操作数据。

### 2.5 存储引擎（Storage Engine）

​		存储引擎负责数据的存储和检索。MySQL支持多种存储引擎，如InnoDB、MyISAM等。每个存储引擎都有自己的存储格式、索引技术和锁机制。存储引擎负责将数据存储到磁盘上，并处理查询的读取和写入操作。

> 整个MySQL的逻辑架构可以简单描述为：客户端发送SQL语句到连接层，连接层将SQL语句传递给查询解析器进行解析，解析器将解析结果传递给优化器进行优化，优化器选择执行计划后将其传递给执行器，执行器与存储引擎交互执行查询操作，并将结果返回给客户端。

## 3. SQL语句执行流程

### 3.1 SQL语句执行流程如图

[![pC6ZRBT.png](https://s1.ax1x.com/2023/07/06/pC6ZRBT.png)](https://imgse.com/i/pC6ZRBT)

### 3.2 SQL执行过程

- **建立连接**：客户端应用程序与数据库建立连接。这涉及到网络通信和身份验证，客户端需要提供正确的用户名和密码来验证身份并获取连接权限。
- **查询发送**：客户端通过SQL接口向数据库发送SQL语句。这可以是查询语句（如SELECT），更新语句（如INSERT、UPDATE、DELETE），还可以包括事务控制语句（如BEGIN、COMMIT、ROLLBACK）等。查询语句先检查查询缓存，如果命中，直接返回结果。缓存可以大大提高系统的性能。
- **查询解析**：数据库服务器接收到SQL语句后，首先进行语法解析和语义检查。这一步骤验证SQL语句的正确性，包括检查语法错误、表和列的存在性、权限验证等。
- **查询优化**：如果SQL语句通过了解析和检查，数据库服务器会使用查询优化器来生成一个高效的执行计划。优化器根据查询的复杂度、表的大小、索引等因素，评估可能的执行计划，并选择成本最低的执行计划。

- **执行计划生成**：优化器选择了最优的执行计划后，会将其转换为实际的操作指令。这个执行计划包括了具体的操作步骤，如表的扫描方式、连接操作、索引使用等。
- **执行操作**：数据库服务器根据执行计划开始执行具体的操作。这可能涉及到数据的读取、写入、更新、删除等。执行操作的过程中，数据库会使用存储引擎来实际操作数据，包括数据的读取和写入。

- **结果返回**：一旦操作完成，数据库服务器将结果返回给客户端应用程序。对于查询语句，结果可能是一组数据行；对于更新语句，结果通常是一个表示执行成功或失败的消息。
- **连接关闭**：在完成所有操作后，客户端应用程序可以选择关闭连接，释放数据库资源。这样可以确保数据库服务器的负载得到合理控制，并且释放占用的资源。

### 3.3 MySQL8的sql执行流程如图

[![pC6e9gI.png](https://s1.ax1x.com/2023/07/06/pC6e9gI.png)](https://imgse.com/i/pC6e9gI)

## 4. MySQL存储引擎

### 4.1 MySQL中存储引擎的查看方式

```sql
SHOW ENGINES; //查看MySQL提供的存储引擎
SHOW VARIABLES LIKE '%default_storage_engine%'; //查看MySQL默认的存储引擎
```

### 4.2 MySQL设置存储引擎

```sql
SET DEFAULT_STORAGE_ENGINE=MyISAM;//设置默认的存储引擎为MyISAM
```

> ① 也可通过修改my.cnf设置。
>
> ② 也可通过sql语句为不同的表设置不同的存储引擎。

### 4.3 常见的存储引擎

#### 4.3.1 InnoDB存储引擎

**特点**

- 默认存储引擎：在MySQL 5.5版本之后，InnoDB成为了MySQL的默认存储引擎。

- 支持事务：InnoDB是一个支持ACID事务的存储引擎，可以提供数据的一致性和可靠性。

- 行级锁定：InnoDB使用行级锁定来实现并发控制，允许多个事务同时读取和写入不同的行，提高了并发性能。

- 外键约束：InnoDB支持外键约束，可以保证数据的完整性和一致性。

- 支持崩溃恢复：InnoDB具有崩溃恢复机制，可以在数据库异常关闭后进行恢复。
- 高并发性能： InnoDB使用多版本并发控制（MVCC，Multi-Version Concurrency Control）机制，可以处理高并发环境下的读写操作。
- 全文搜索和数据也压缩。

> 适用于高并发、事务性的应用场景，提供了可靠性、数据完整性和高性能的支持。

#### 4.3.2 MyISAM存储引擎

**特点**

- 表级锁定：MyISAM使用表级锁定（table-level locking），而不是行级锁定（row-level locking）。这意味着在并发访问下，不同的事务必须按顺序对整个表进行锁定，导致并发性能受限。

- 不支持事务：MyISAM存储引擎不支持事务处理。它不具备事务的ACID属性，无法保证多个操作作为一个逻辑单元的原子性、一致性和隔离性。

- 高速读取：相对于InnoDB等存储引擎，MyISAM在读取操作上具有较高的性能。它使用了简单的索引结构，适用于主要进行读取操作的应用场景，例如只读或读多写少的应用。

- 全文搜索：MyISAM支持全文搜索索引，可以进行高效的全文搜索和匹配操作。它提供了全文搜索函数和关键词搜索功能，对于需要进行文本搜索的应用场景具有一定的优势。

- 较低的存储空间占用：MyISAM在存储数据时占用较少的磁盘空间，这主要得益于其索引结构和数据存储方式。这使得MyISAM在存储大量数据时具有一定的优势。

- 不支持外键约束：MyISAM存储引擎不支持外键约束，无法建立引用完整性关系。这意味着在使用MyISAM时，需要在应用层面进行外键约束的处理。

- 较差的并发性能：由于表级锁定的限制，MyISAM在高并发环境下的并发性能相对较差。当多个事务同时对同一张表进行操作时，可能会出现锁冲突和性能瓶颈。

> MyISAM适用于读取密集型的应用场景，例如报表生成、日志分析等。它具有较高的读取性能和较低的存储空间占用，但不支持事务和行级锁定，对于要求数据一致性和高并发性的应用，建议使用其他支持事务和行级锁定的存储引擎，如InnoDB。

#### 4.3.3 MyISAM和InnoDB的对比

| 对比项         | MyISAM                                                   | InnoDB                                                       |
| -------------- | -------------------------------------------------------- | ------------------------------------------------------------ |
| 外键           | 不支持                                                   | 支持                                                         |
| 事务           | 不支持                                                   | 支持                                                         |
| 行表锁         | 表锁，即使操作一条记录也会锁住整个表，不适合高并发的操作 | 行锁，操作时只锁某一行，不对其它行有影响，适合高并发的操作。和表锁！ |
| 缓存           | 只缓存索引，不缓存真实数据                               | 不仅缓存索引还要缓存真实数据，对内存要求较高，而且内存大小对性能有决定性的影响。支持聚簇索引 |
| 关注点         | 并发查询，节省资源、消耗少、简单业务                     | 并发写、事务、更大更复杂的资源操作                           |
| 默认使用       | N                                                        | Y                                                            |
| 自带系统表使用 | Y                                                        | N                                                            |

