---
layout: post
categories : Spring
tags : Spring Sqlite DB
title:  "[Spring] Spring Boot에 Sqlite3 연동하기"
---

회사에서 RDBMS를 사용한 프로젝트를 SQLite로 아키텍처를 바꾸어야하는 일이 있어, 

Spring boot에 SQLite를 연동하게 되었는데 그 기록을 남겨두려 한다. 

# 1. 설정 방법
## 1-1. Dependency 추가 
```java
// https://mvnrepository.com/artifact/org.xerial/sqlite-jdbc
implementation 'org.xerial:sqlite-jdbc:3.42.0.0'
```

## 1-2. Dialect 추가
SQLite로 아키텍처를 바꾸기로 했을 당시, 가장 장점이라고 생각했던 부분은 JPA를 사용한 많은 서비스들을 수정하지 않아도 된다는 것이였다.
Hibernate은 Dialect를 이용해 여러 DB들의 문법 차이를 자동으로 변환해주지만, SQLite에 대한 Dialect는 따로 추가해주어야한다. 

```java
import org.hibernate.dialect.Dialect;
import org.hibernate.dialect.function.SQLFunctionTemplate;
import org.hibernate.dialect.function.StandardSQLFunction;
import org.hibernate.dialect.function.VarArgsSQLFunction;
import org.hibernate.type.StringType;
import java.sql.Types;

public class SQLiteDialect extends Dialect {
    public SQLiteDialect() {
        registerColumnType(Types.BIT, "integer");
        registerColumnType(Types.TINYINT, "tinyint");
        registerColumnType(Types.SMALLINT, "smallint");
        registerColumnType(Types.INTEGER, "integer");
        registerColumnType(Types.BIGINT, "bigint");
        registerColumnType(Types.FLOAT, "float");
        registerColumnType(Types.REAL, "real");
        registerColumnType(Types.DOUBLE, "double");
        registerColumnType(Types.NUMERIC, "numeric");
        registerColumnType(Types.DECIMAL, "decimal");
        registerColumnType(Types.CHAR, "char");
        registerColumnType(Types.VARCHAR, "varchar");
        registerColumnType(Types.LONGVARCHAR, "longvarchar");
        registerColumnType(Types.DATE, "date");
        registerColumnType(Types.TIME, "time");
        registerColumnType(Types.TIMESTAMP, "timestamp");
        registerColumnType(Types.BINARY, "blob");
        registerColumnType(Types.VARBINARY, "blob");
        registerColumnType(Types.LONGVARBINARY, "blob");
        // registerColumnType(Types.NULL, "null");
        registerColumnType(Types.BLOB, "blob");
        registerColumnType(Types.CLOB, "clob");
        registerColumnType(Types.BOOLEAN, "integer");

        registerFunction("concat", new VarArgsSQLFunction(StringType.INSTANCE, "", "||", ""));
        registerFunction("mod", new SQLFunctionTemplate(StringType.INSTANCE, "?1 % ?2"));
        registerFunction("substr", new StandardSQLFunction("substr", StringType.INSTANCE));
        registerFunction("substring", new StandardSQLFunction("substr", StringType.INSTANCE));
    }

    public boolean supportsIdentityColumns() {
        return true;
    }

    public boolean hasDataTypeInIdentityColumn() {
        return false; // As specify in NHibernate dialect
    }

    public String getIdentityColumnString() {
        return "integer";
    }

    public String getIdentitySelectString() {
        return "select last_insert_rowid()";
    }

    @Override
    @Deprecated
    public boolean supportsLimit() {
        return true;
    }

    @Override
    @Deprecated
    protected String getLimitString(String query, boolean hasOffset) {
        return query + (hasOffset ? " limit ? offset ?" : " limit ?");
    }

    public boolean supportsTemporaryTables() {
        return true;
    }

    public String getCreateTemporaryTableString() {
        return "create temporary table if not exists";
    }

    public boolean dropTemporaryTableAfterUse() {
        return false;
    }

    @Override
    public boolean supportsCurrentTimestampSelection() {
        return true;
    }

    @Override
    public boolean isCurrentTimestampSelectStringCallable() {
        return false;
    }

    @Override
    public String getCurrentTimestampSelectString() {
        return "select current_timestamp";
    }

    @Override
    public boolean supportsUnionAll() {
        return true;
    }

    @Override
    public boolean hasAlterTable() {
        return false; // As specify in NHibernate dialect
    }

    @Override
    public boolean dropConstraints() {
        return false;
    }

    @Override
    public String getAddColumnString() {
        return "add column";
    }

    @Override
    public String getForUpdateString() {
        return "";
    }

    @Override
    public boolean supportsOuterJoinForUpdate() {
        return false;
    }

    @Override
    public String getDropForeignKeyString() {
        throw new UnsupportedOperationException("No drop foreign key syntax supported by SQLiteDialect");
    }

    @Override
    public String getAddForeignKeyConstraintString(String constraintName, String[] foreignKey, String referencedTable,
                                                   String[] primaryKey, boolean referencesPrimaryKey) {
        throw new UnsupportedOperationException("No add foreign key syntax supported by SQLiteDialect");
    }

    @Override
    public String getAddPrimaryKeyConstraintString(String constraintName) {
        throw new UnsupportedOperationException("No add primary key syntax supported by SQLiteDialect");
    }

    @Override
    public boolean supportsIfExistsBeforeTableName() {
        return true;
    }

    @Override
    public boolean supportsCascadeDelete() {
        return false;
    }
}

```

## 1-3. DB 설정 추가
application.yml파일 내에 SQLite에 대한 설정으로 수정 및 추가해주어야한다.

```yml
spring:
  jpa:
    properties:
      database-platform: test.config.SQLiteDialect # 추가한 Dialect 파일 경로 맞추기
      hibernate:
        dialect: test.config.SQLiteDialect # 추가한 Dialect 파일 경로 맞추기
  datasource:
    driver-class-name: org.sqlite.JDBC
    url: jdbc:sqlite:testdb.db # 연결할 DB파일 (app이 실행될때 생성됨)
```

# 2. 개인적인 생각
File DB여서 갖는 장점도 분명히 있겠지만, Write를 동시에 하지 못한다는 점은 꽤 크리티컬한 이슈가 될 수 있는 것 같다. 

여러 대안들을 사용해보았지만 한계를 느끼고 결국 다른 아키텍처로 또다시 변경하게 된 이 시점에서, 

그래도 장기 기억에 남겨두고자 글을 쓰는 이 시점에서는 솔직히 SQLite는 잘 모르겠다. 


혹시 누군가 SQLite를 사용하기 위해 이 글을 참고하게 된다면 

연동하려는 프로젝트가 Write를 빈번하게 처리해야하는 프로젝트는 아닌지

DB Locking 이슈에 대해 충분히 인지하고 있는지 다시 한번 검토해보길 바라는 마음이다.


## 3. 시도해보았던 것들 중 일부. 
```yml
datasource:
    type: com.zaxxer.hikari.HikariDataSource
    driver-class-name: org.sqlite.JDBC
    url: jdbc:sqlite:testdb.db
    username: test
    password: test1234
    hikari:
      connection-init-sql: PRAGMA journal_mode=WAL; PRAGMA busy_timeout=100000; PRAGMA synchronous=OFF;
      maximum-pool-size: 100
      minimum-idle: 10
      connection-timeout: 30000
      idle-timeout: 30000
      leak-detection-threshold: 100000 
      registerMbeans: true  
```

- **maximum-pool-size**: 풀이 한 번에 유지할 수 있는 최대 연결 수
- **minimum-idle**: HikariCP가 풀에서 유지하려고 하는 최소 대기 연결 수
- **connection-timeout**: 클라이언트가 풀에서 연결을 기다리는 최대 밀리초. 이 시간이 초과되었는데 연결이 불가하면, SQLException 발생
- **idle-timeout**: 대기 상태의 연결은 이 타임아웃을 초과하면 취소됨
- **leak-detection-threshold** : 연결이 반환되지 않았을 가능성이 있는지 확인하는 데 걸리는 시간(밀리초)
- **registerMbeans** : MBeans는 Java Management Extensions (JMX)를 통해 애플리케이션의 동작을 모니터링하고 관리할 수 있는 자바 객체.
HikariCP는 JMX MBeans를 등록하여 풀의 상태와 동작을 모니터링 할 수 있고, JMX를 사용하면, 연결 풀의 현재 상태, 활성 연결 수, 대기 연결 수 등과 같은 다양한 메트릭스와 통계를 볼 수 있음.

