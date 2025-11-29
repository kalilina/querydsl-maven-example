# QueryDSL Maven Example
Демонстрационный проект с примером подключения **QueryDSL** к Maven-проекту.

Официальный querydsl почти не обновляется, поэтому будет использован форк от OpenFeign - https://github.com/OpenFeign/querydsl

В данном примере будет использован **QueryDSL 6.12** для работы с **Hibernate 6**. Если используете **Hibernate 7** - выбирайте версии QueryDSL **7.0** и выше. Если для вас уже вышел новый Hibernate тогда перейдите по ссылке выше и гляньте релизы. Там должны появится новые версии.

## Dependency
Сначала добавьте зависимости в секцию `dependencies` файла `pom.xml`:
```xml
<dependencies>
<!-- другие зависимости проекта -->

    <dependency>
      <groupId>io.github.openfeign.querydsl</groupId>
      <artifactId>querydsl-jpa</artifactId>
      <version>6.12</version>
    </dependency>
    <dependency>
      <groupId>io.github.openfeign.querydsl</groupId>
      <artifactId>querydsl-apt</artifactId>
      <version>6.12</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>
```
Источник - https://mvnrepository.com/artifact/io.github.openfeign.querydsl

## Annotation Processing
Для генерации Q-классов потребуется подключить `maven-compiler-plugin`. В параметре `annotationProcessorPaths` указывается `querydsl-apt`. За саму генерацию классов отвечает параметр `classifier`.

Пример:
```xml
<build>
  <!-- другие настройки build -->

    <plugins>
      <!-- другие плагины -->
      
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.14.1</version>
        <configuration>
          <source>23</source>
          <target>23</target>
          <annotationProcessorPaths>
            <!-- другие пути, если нужны (например: lombok) -->

            <path>
              <groupId>io.github.openfeign.querydsl</groupId>
              <artifactId>querydsl-apt</artifactId>
              <version>6.12</version>
              <classifier>jpa</classifier>
            </path>
          </annotationProcessorPaths>
        </configuration>
      </plugin>
    </plugins>
  </build>
```

Версию `maven-compiler-plugin` укажите в соответствии с вашим проектом. Параметры `source` и `target` ставьте в соответствии своей версии Java, или в начале `pom` файла укажите весию Java для всего проекта:
```xml
<properties>
  <maven.compiler.source>23</maven.compiler.source>
  <maven.compiler.target>23</maven.compiler.target>
</properties>
```

## Build
Для сборки проекта выполните: 
```bash
mvn clean compile
```  

В процессе сборки Q-классы будут сгенерированы автоматически в директорию:  

`{project-dir}/target/generated-sources/annotations/`

## Example
Полный рабочий пример конфигурации доступен в файле [pom.xml](./pom.xml). В примере используется Hibernate 6 и QueryDSL 6.12.

## post scriptum
Если что Q-классы генерятся в target/annotations. Оно так-то работает, но хотелось бы изменить на target/java. Провозился больше часа и в итоге забил. Если вы новичок тогда тоже забейте. Если в курсе как это сделать через pom - можете добавить pull request.
