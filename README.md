# db-migration
<p align="center">
    <a target="_blank" href="https://search.maven.org/search?q=g:%22com.github.mengweijin%22%20AND%20a:%22db-migration%22">
        <img src="https://img.shields.io/maven-central/v/com.github.mengweijin/db-migration?label=db-migration&color=blue" />
    </a>
	<a target="_blank" href="https://github.com/mengweijin/db-migration/blob/master/LICENSE">
		<img src="https://img.shields.io/badge/license-Apache2.0-blue.svg" />
	</a>
	<a target="_blank" href="https://www.oracle.com/technetwork/java/javase/downloads/index.html">
		<img src="https://img.shields.io/badge/JDK-8+-green.svg" />
	</a>
	<a target="_blank" href="https://gitee.com/mengweijin/db-migration/stargazers">
		<img src="https://gitee.com/mengweijin/db-migration/badge/star.svg?theme=dark" alt='gitee star'/>
	</a>
	<a target="_blank" href='https://github.com/mengweijin/db-migration'>
		<img src="https://img.shields.io/github/stars/mengweijin/db-migration.svg?style=social" alt="github star"/>
	</a>
</p>

## 介绍
Flyway、Liquibase 扩展支持达梦（DM）、南大通用（GBase 8s）、OpenGauss 等国产数据库。部分数据库直接支持 Flowable 工作流。

### 已支持

- **达梦（DM 8）**：支持 Flyway 和 Liquibase，支持 flowable 工作流。
- **南大通用（GBase 8s）**：支持 Flyway 和 Liquibase。
- **OpenGauss**：支持 Flyway，Liquibase 可直接使用 postgres 驱动得到支持。
- **人大金仓（Kingbase）**：可直接使用 postgres 驱动得到支持，无需依赖 db-migration 项目。

### 版本说明

- ❌❌：不支持；
- 🈯✅：flyway 或 liquibase **需要**指定特定版本才支持；
- ❄️✅：flyway 或 liquibase **不需要**指定版本就支持（不指定版本，则默认使用的 spring boot 默认版本）；

| db-migration 版本 | spring boot 版本 |   flyway 版本 | liquibase 版本 |
|:----------------|:---------------|------------:|-------------:|
| 2.0.8           | 2.0.x.RELEASE  |   7.15.0 ❌❌ |    4.27.0 ❌❌ |
| 2.0.8           | 2.1.x.RELEASE  |   7.15.0 ❌❌ |   4.27.0 🈯✅ | 
| 2.0.8           | 2.2.x.RELEASE  |   7.15.0 ❌❌ |   4.27.0 🈯✅ | 
| 2.0.8           | 2.3.x.RELEASE  |   7.15.0 ❌❌ |   4.27.0 🈯✅ | 
| 2.0.8           | 2.4.x          |  7.15.0 🈯✅ |   4.27.0 🈯✅ |  
| 2.0.8           | 2.5.x          |  7.15.0 🈯✅ |   4.27.0 🈯✅ |  
| 2.0.8           | 2.6.x          |   8.0.4 ❄️✅ |   4.27.0 🈯✅ | 
| 2.0.8           | 2.7.x          |  8.5.11 ❄️✅ |   4.27.0 🈯✅ | 
| 2.0.8           | 3.0.x          |   9.5.1 ❄️✅ |   4.27.0 🈯✅ | 
| 2.0.8           | 3.1.x          |  9.16.3 ❄️✅ |   4.27.0 🈯✅ | 
| 2.0.8           | 3.2.x          |  9.22.3 ❄️✅ |   4.27.0 🈯✅ | 
| 2.0.8           | 3.3.x          | 10.10.0 ❄️✅ |   4.27.0 ❄️✅ |
| 2.0.8           | 3.4.x          | 10.10.0 🈯✅ |   4.27.0 🈯✅ |

## 参考文档

- [【达梦 DM DBMS】 使用 Flyway](./doc/dm_use_flyway.md)
- [【达梦 DM DBMS】 使用 Liquibase 和 Flowable 工作流](./doc/dm_use_liquibase_flowable.md)
- [【南大通用 GBase 8s】 使用 Flyway](./doc/gbase8s_use_flyway.md)
- [【南大通用 GBase 8s】 使用 Liquibase](./doc/gbase8s_use_liquibase.md)
- [【华为 OpenGauss】 使用 Flyway](./doc/opengauss_use_flyway.md)
- [【华为 OpenGauss】 使用 Liquibase](./doc/opengauss_use_liquibase.md)
- [【人大金仓 Kingbase】 使用 Flyway 的示例工程](https://gitee.com/mengweijin/db-migration/tree/master/demo-kingbase/kingbase-flyway)
- [【人大金仓 Kingbase】 使用 Liquibase 的示例工程](https://gitee.com/mengweijin/db-migration/tree/master/demo-kingbase/kingbase-liquibase)

### Flyway 对 PL/SQL 的支持

Oracle 的存储过程、触发器、函数等 PL/SQL 代码块需以 `/` 符号结尾，告知 SQL 引擎执行该代码块。

因此，在 Flyway 中执行 Oracle 存储过程脚本时，必须在 PL/SQL 块的末尾添加 `/` 符号，以明确表示代码块的结束。
这是 Oracle 数据库对 PL/SQL 语法解析的要求，Flyway 在执行脚本时同样需要遵循这一规则。

Flyway 默认使用普通 SQL 解析器执行脚本，而 Oracle 的 PL/SQL 块需要明确的分隔符。添加 / 符号能帮助 Flyway 识别代码块边界。

例如在创建存储过程或触发器时：

```sql
CREATE OR REPLACE PROCEDURE test_proc AS
BEGIN
    -- 存储过程逻辑。此处略。
END;
/
   
CREATE TRIGGER test_trig AFTER INSERT ON test_user
BEGIN
    UPDATE test_user SET name = CONCAT(name, 'triggered');
END;
/
```

与普通 SQL 脚本的区别：

普通的 DDL（如建表）或 DML（如插入数据）脚本无需 / 符号，因为它们不是多行代码块。 例如：

```sql
CREATE TABLE users (id NUMBER PRIMARY KEY);
INSERT INTO users VALUES (1);
```

但涉及 PL/SQL 结构的脚本（如存储过程、函数、包）必须添加 /。

## 其他文档

* [Oracle 清理 Flowable 7.0.1 所有表脚本](./doc/use_oracle_flowable_drop_script.md)
* [MySQL、Oracle、PostgreSQL 等数据库使用Flyway 的温馨提示](./doc/z_flyway_supported_database_notes.md)

完整的基础使用示例参考代码仓库中，各自的 demo 工程。

## 捉急请联系我👇联系前请先提 issue!
|     QQ      |       邮箱        |
|:-----------:|:---------------:|
| 1002284406  | mwjwork@qq.com  |

## ⭐Star db-migration on GitHub

[![Stargazers over time](https://starchart.cc/mengweijin/db-migration.svg)](https://starchart.cc/mengweijin/db-migration)
