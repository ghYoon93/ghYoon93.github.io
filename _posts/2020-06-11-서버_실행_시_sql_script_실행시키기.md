---
title: 서버_실행_시_sql_script_실행시키기_[SPRING]
categories: [TIL]
tags: [spring, h2]
---

### DataSourceConfig.java

```java
Resource tblBoard = new ClassPathResource("sql/tbl_board.sql");
DatabasePopulator populator = new ResourceDatabasePopulator(tblBoard);
DatabasePopulatorUtils.execute(populator, dataSource);
```

