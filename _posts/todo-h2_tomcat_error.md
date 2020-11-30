11월 30, 2020 11:46:30 오후 org.apache.catalina.core.StandardContext reload
INFO: 이름이 []인 컨텍스트를 다시 로드하는 작업이 시작되었습니다.
11월 30, 2020 11:46:30 오후 org.apache.catalina.core.ApplicationContext log
INFO: Destroying Spring FrameworkServlet 'dispatcher'
11월 30, 2020 11:46:30 오후 org.apache.catalina.core.ApplicationContext log
INFO: Closing Spring root WebApplicationContext
INFO : com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Shutdown initiated...
INFO : com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Shutdown completed.

11월 30, 2020 11:46:30 오후 org.apache.catalina.loader.WebappClassLoaderBase clearReferencesJdbc
WARNING: 웹 애플리케이션 [ROOT]이(가) JDBC 드라이버 [net.sf.log4jdbc.sql.jdbcapi.DriverSpy]을(를) 등록했지만, 웹 애플리케이션이 중지될 때, 해당 JDBC 드라이버의 등록을 제거하지 못했습니다. 메모리 누수를 방지하기 위하여, 등록을 강제로 제거했습니다.

11월 30, 2020 11:46:30 오후 org.apache.catalina.loader.WebappClassLoaderBase clearReferencesJdbc
WARNING: 웹 애플리케이션 [ROOT]이(가) JDBC 드라이버 [oracle.jdbc.OracleDriver]을(를) 등록했지만, 웹 애플리케이션이 중지될 때, 해당 JDBC 드라이버의 등록을 제거하지 못했습니다. 메모리 누수를 방지하기 위하여, 등록을 강제로 제거했습니다.
11월 30, 2020 11:46:30 오후 org.apache.catalina.loader.WebappClassLoaderBase clearReferencesJdbc

WARNING: 웹 애플리케이션 [ROOT]이(가) JDBC 드라이버 [org.h2.Driver]을(를) 등록했지만, 웹 애플리케이션이 중지될 때, 해당 JDBC 드라이버의 등록을 제거하지 못했습니다. 메모리 누수를 방지하기 위하여, 등록을 강제로 제거했습니다.

11월 30, 2020 11:46:30 오후 org.apache.catalina.loader.WebappClassLoaderBase clearReferencesThreads
WARNING: 웹 애플리케이션 [ROOT]이(가) [MVStore background writer nio:C:/Users/grand/dogiver.mv.db](이)라는 이름의 쓰레드를 시작시킨 것으로 보이지만, 해당 쓰레드를 중지시키지 못했습니다. 이는 메모리 누수를 유발할 가능성이 큽니다. 해당 쓰레드의 스택 트레이스:
 java.base@14.0.2/java.lang.Object.wait(Native Method)
 org.h2.mvstore.MVStore$BackgroundWriterThread.run(MVStore.java:2708)
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by org.apache.catalina.loader.WebappClassLoaderBase (file:/D:/Program%20Files/Apache%20Software%20Foundation/Tomcat%209.0/lib/catalina.jar) to field java.io.ObjectStreamClass$Caches.localDescs
WARNING: Please consider reporting this to the maintainers of org.apache.catalina.loader.WebappClassLoaderBase
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release

11월 30, 2020 11:46:36 오후 org.apache.catalina.core.ApplicationContext log
INFO: 1 Spring WebApplicationInitializers detected on classpath

11월 30, 2020 11:46:36 오후 org.apache.jasper.servlet.TldScanner scanJars
INFO: 적어도 하나의 JAR가 TLD들을 찾기 위해 스캔되었으나 아무 것도 찾지 못했습니다. 스캔했으나 TLD가 없는 JAR들의 전체 목록을 보시려면, 로그 레벨을 디버그 레벨로 설정하십시오. 스캔 과정에서 불필요한 JAR들을 건너뛰면, 시스템 시작 시간과 JSP 컴파일 시간을 단축시킬 수 있습니다.

11월 30, 2020 11:46:36 오후 org.apache.catalina.core.ApplicationContext log
INFO: Initializing Spring root WebApplicationContext