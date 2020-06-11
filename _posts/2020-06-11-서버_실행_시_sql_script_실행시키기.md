---
title: [SPRING] 서버 실행 시 sql script 실행시키기
categories: TIL
tags: [spring,h2]
---



```java
Resource tblBoard = new ClassPathResource("sql/tbl_board.sql");
DatabasePopulator populator = new ResourceDatabasePopulator(tblBoard);
DatabasePopulatorUtils.execute(populator, dataSource);
```

