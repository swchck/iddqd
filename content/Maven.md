# Maven

Зависимости
Зависимости - следующая очень важная часть pom.xml - тут хранится список всех библиотек (зависимостей) которые используюся в проекте. 
Каждая библиотека идентифицируется также как и сам проект - тройкой groupId, artifactId, version (GAV). 
Объявление зависимостей заключено в тэг ```<dependencies>...</dependencies>```

```<dependencies>
                        <dependency>
                            <groupId>junit</groupId>
                            <artifactId>junit</artifactId>
                            <version>4.4</version>
                            <scope>test</scope>
                        </dependency>
                        <dependency>
                            <groupId>org.powermock</groupId>
                            <artifactId>powermock-reflect</artifactId>
                            <version>${version}</version>
                        </dependency>
                        <dependency>
                            <groupId>org.javassist</groupId>
                            <artifactId>javassist</artifactId>
                            <version>3.13.0-GA</version>
                            <scope>compile</scope>
                        </dependency>
                    </dependencies>```
					
Как вы могли заметить, кроме GAV при описании зависимости может присутствовать тэг <scope>. Scope задаёт, для чего библиотека используется.
В данном примере говорится, что библиотека с GAV junit:junit:4.4 нужна только для выполнения тестов.
 
Тэг <build>
Тэг <build> не обязательный, т. к. существуют значения по умолчанию. Этот раздел содержит информацию по самой сборке: где находятся исходные файлы, где ресурсы, какие плагины используются. Например:

          
         ```<build>
         <outputDirectory>target2</outputDirectory>
          <finalName>ROOT</finalName>
         <sourceDirectory>src/java</sourceDirectory>
            <resources>
                <resource>
                    <directory>${basedir}/src/java</directory>
                    <includes>
                    <include>**/*.properties</include>
                    </includes>
                </resource>
            </resources>
                 <plugins>
                     <plugin>
                         <groupId>org.apache.maven.plugins</groupId>
                         <artifactId>maven-pmd-plugin</artifactId>
                         <version>2.4</version>
                     </plugin>
                 </plugins>
             </build>```
                    
   
Давайте рассмотрим этот пример более подробно.

<sourceDirectory>
определяет, откуда maven будет брать файлы исходного кода. По умолчанию это src/main/java, но вы можете определить, где это вам удобно. Директория может быть только одна (без использования специальных плагинов)
<resources>
и вложенные в неё тэги <resource> определяют, одну или несколько директорий, где хранятся файлы ресурсов. Ресурсы в отличие от файлов исходного кода при сборке просто копируются . Директория по умолчанию src/main/resources
<outputDirectory>
определяет, в какую директорию компилятор будет сохранять результаты компиляции - *.class файлы. Значение по умолчанию - target/classes
<finalName>
- имя результирующего jar (war, ear..) файла с соответствующим типу расширением, который создаётся на фазе package. Значение по умолчанию — artifactId-version. 

Репозитории - это место где хранятся артефакты: jar файлы, pom -файлы, javadoc, исходники. Существуют:

Локальный репозиторий по умолчанию он расположен в <home директория>/.m2/repository - персональный для каждого пользователя.
центральный репозиторий который расположен в http://repo1.maven.org/maven2/ и доступен на чтение для всех пользователей в интернете.
Внутренний "Корпоративный" репозиторий- дополнительный репозиторий, один на несколько пользователей.

Локальный репозиторий
Локальный репозиторий по умолчанию расположен в <home директория>/.m2/repository. Здесь лежат артефакты которые были скачаны из центрального репозитория либо добавлены другим способом. Например если вы наберёте команду

Центральный репозиторий
Чтобы самому каждый раз не создавать репозиторий, сообщество для Вас поддерживает центральный репозиторий. Если для сборки вашего проекта не хватает зависимостей, то они по умолчанию автоматически скачиваются с http://repo1.maven.org/maven2. В этом репозитории лежат практически все опенсорсные фреймворки и библиотеки.

Самому в центральный репозиторий положить нельзя. Т.к. этот репозиторий используют все, то перед тем как туда попадают артефакты они проверяются, тем более что если артефакт однажды попал в репозиторий, то по правилам изменить его нельзя.

Для поиска нужной библиотеки очень удобно пользоваться сайтами http://mavenrepository.com/ и http://findjar.com/

Корпоративный репозиторий
Если вы хотите создать свой репозиторий, содержимое которого вы можете полностью контролировать(как локальный), и сделать так, чтобы он был доступен для нескольких человек, вам будет полезен корпоративный репозиторий. Доступ к артефактам можно ограничивать настройками безопасности сервера так, что код ваших проектов не будет доступен извне.

Чтобы добавить репозиторий в список, откуда будут скачиваться зависимости, нужно добавить секцию repositories в pom.xml, например:

                    ```<project>
                      <repositories>
                        <repository>
                          <id>my-company-repo</id>
                          <url>http://my-company-site.ru/repo</url>
                        </repository>
                      </repositories>
                    </project>```

                
Существуют несколько реализаций серверов - репозиториев maven. Наиболее известные это artifactory, continuum, nexus.

Основные фазы сборки проекта
1. compile
Компилирование проекта
2. test
Тестирование с помощью JUnit тестов
3. package
Создание .jar файла или war, ear в зависимости от типа проекта
4. integration-test
Запуск интеграционных тестов
5. install
Копирование .jar (war , ear) в локальный репозиторий
6. deploy
публикация файла в удалённый репозиторий

Мавен изначально создавался , принимая во внимание портируемость. Но довольно часто приложение приходится запускать в разном окружении: например, для разработки используется одна база данных, в рабочем сервере используется другая. при этом могут понадобиться разные настройки, разные зависимости и плагины. Для этих целей в maven используются профайлы.

Давайте определим два профайла: один для разработки, другой для производственного сервера. Для разработки вполне подойдёт база hsqldb, которая хранит все данные в памяти. На производственном сервере же используется база данных postgres, которая сохраняет все данные на диск. В профайлах для каждой конфигурации определены свои проперти database.url и зависимости для разных jdbc драйверов.

Ниже приведён пример объявления таких профайлов.

```<?xml version="1.0" encoding="UTF-8"?>
<project>
  <profiles>
(1)     <profile>
            <id>development</id>
            <properties>
                <database.url>jdbc:hsqldb:mem:testdb</database.url>
            </properties>
            <dependencies>
                <dependency>
                    <groupId>org.hsqldb</groupId>
                    <artifactId>hsqldb</artifactId>
                    <version>2.0.0</version>
                </dependency>
            </dependencies>
        </profile>
(2)     <profile>
            <id>productionServer</id>
            <properties>
                <database.url>jdbc:postgresql://databseserver/database</database.url>
            </properties>
            <dependencies>
                <dependency>
                    <groupId>postgresql</groupId>
                    <artifactId>postgresql</artifactId>
                    <version>9.0-801.jdbc4</version>
                </dependency>
            </dependencies>
        </profile>
    </profiles>
</project>```
               
цифрами 1 и 2 обозначены начала объявления профайлов. каждый профайл имеет идентификатор в данном случае development и productionServer.

Внутри тэга <profile> содержатся все те же объявления что и внутри <project>: properties, dependencies, и др. Вот полный список тегов которые могут содержаться внутри профайлов:
```
 * <repositories>
 * <pluginRepositories>
 * <dependencies>
 * <plugins>
 * <properties>
 * <modules>
 * <reporting>
 * <dependencyManagement>
 * <distributionManagement>
 *  <build> тэг, который может содержать
         o <defaultGoal>
         o <resources>
         o <testResources>
         o <finalName>```

         При сборке проекта в тестах произошла ошибка. Как мне собрать проект без запуска тестов? Для запуска сборки без выполнения тестов добавьте -Dmaven.test.skip=true к командной строке запуска maven:

```mvn install -Dmaven.test.skip=true```

Как запустить только один тест? Для запуска только одного теста добавьте -Dtest=[Имя класса] к командной строке запуска maven. Например

```mvn install -Dtest=ru.apache-maven.utils.ConverterTest```

Компилятор
Компилятор - основной плагин который используется практически во всех проектах. Он доступен по умолчанию, но практически в каждом проекте, его приходится переобъявлять т.к. настройки по умолчанию не очень подходящие. 
Пример использования:

                        <plugin>
                            <groupId>org.apache.maven.plugins</groupId>
                            <artifactId>maven-compiler-plugin</artifactId>
                            <version>2.0.2</version>
                            <configuration>
                                <source>1.6</source>
                                <target>1.6</target>
                                <encoding>UTF-8</encoding>
                            </configuration>
                        </plugin>
В этом примере в кофигурации используется версия java 1.6 (source - версия языка на котором написана программа; target - версия java машины которая будет этот код запускать) и указано что кодировка исходного кода программы UTF-8. По умолчанию версии java - 1.3 а кодировка - та которая у операционной системы по умолчанию.
Вообще у плагина есть две цели compiler:compile и compiler:testCompile

compiler:compile - компилирует основную ветку исходников и по умолчанию связана с фазой compile
compiler:testCompile - компилирует тесты и по умолчанию связана с фазой test-compile.
Кроме приведёных настроек для компилятора можно задать следующие параметры:
verbose
true или false
fork
запусть компиляцию в отдельной jvm
executable
путь к javac
compilerVersion
meminitial
maxmem
debug
compilerArgument
задать аргуметы в одной коммандной строке-verbose -bootclasspath ${java.home}\lib\rt.jar
compilerArguments
задать аргуметы в коммандной строке пораздельно в тегах verbose, bootclasspath и др.
compilerId
позволяет задать язык программирования исходного кода, например csharp

maven-surefire-plugin - плагин для запуска тестов
maven-surefire-plugin - плагин который запускает тесты и генерирует отчёты по результатам их выполнения. По умолчанию отчёты сохраняются в ${basedir}/target/surefire-reports и находятся в двух форматах - txt и xml. maven-surefire-plugin содержит единственную цель surefire:test тесты можно писать используя как JUnit так и TestNG.

по умолчанию запускаются все тесты с такими именами * "**/Test*.java" - включает все java файлы которые начинаются с "Test" и расположены в поддиректориях. * "**/*Test.java" - включает все java файлы которые заканчиваются на "Test" и расположены в поддиректориях. * "**/*TestCase.java" - включает все java файлы которые заканчиваются на "TestCase" и расположены в поддиректориях.

Чтобы вручную добавлять или удалять классы тестов можно посмотреть здесь http://maven.apache.org/plugins/maven-surefire-plugin/examples/inclusion-exclusion.html.

Запустить отдельный тест можно так: mvn -Dtest=TestCircle test имейте в виду что если вы хотите отладить тест в среде разработки то в конфигурации плагина нужно выставить

<forkMode>never</forkMode>
либо запускать тесты с remotedebug
mvn -Dmaven.surefire.debug test
Пропустить выполнение тестов можно

                    <configuration>
                      <skipTests>true</skipTests>
                    </configuration>
или
mvn install -DskipTests
чтобы пропустить ещё и компиляцию тестов вызовите maven так:
mvn install -Dmaven.test.skip=true

Наследование
Наследование позволяет избавиться от дублирования описаний.

Давайте рассмотрим как использовать наследование при описании maven проектов. Предположим, что у нас несколько проектов и в каждом мы используем библиотеку log4j для логгирования приложения.

Сначала создадим проект-предок. Весь проект-предок будет состоять из одного файла pom.xml


                <project xmlns="http://maven.apache.org/POM/4.0.0"
                   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                   http://maven.apache.org/xsd/maven-4.0.0.xsd">
                          <modelVersion>4.0.0</modelVersion>
                          <groupId>ru.apache-maven</groupId>
                          <artifactId>parent</artifactId>
                          <version>1.0-SNAPSHOT</version>
                        <packaging>pom</packaging>
                  </project>
                
                
как вы заметили тэг packaging содержит значение pom. Теперь давайте внутри что-нибудь объявим. Например добавим
                <dependencies>
                <dependency>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                    <version>1.2.16</version>
                </dependency
                
                
и установим в локальный репозиторий:
                cd  parent
                mvn install.
                
Если посмотреть список зависимостей, то он будет такой:
                $ mvn dependency:tree
                     \- log4j:log4j:jar:1.2.16:compile
                
Теперь можно создать один или несколько проектов-потомков:

                <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
                <modelVersion>4.0.0</modelVersion>

                <!--<groupId>ru.apache_maven</groupId>-->
                <artifactId>testproj1</artifactId>
                <!--<version>1.0-SNAPSHOT</version>-->
                <parent>
                    <groupId>ru.apache_maven</groupId>
                    <artifactId>parent</artifactId>
                    <version>1.0-SNAPSHOT</version>
                </parent>
                <packaging>jar</packaging>

                <dependencies>
                </dependencies>
            </project>
            
                
Как видно в примере, мы объявили первое на что стоит обратить внимание — это то что groupId и version закомментированы. Они теперь необязательны и берутся по умолчанию такие же как и у parent проекта, хотя можно задать значения и отличные от тех которые по умолчанию. Второе и самое важное, если посмотреть список зависимойстей
                    $ mvn dependency:tree
                    \- log4j:log4j:jar:1.2.16:compile
                
то можно увидеть log4j, хотя мы в pom.xml проекта мы log4j не объявляли. Он унаследовался от проекта-предка.
Кроме зависимостей в проекте-предке часто объявляют плагины:

maven-compiler-plugin нужен в каждом проекте!
properties
репозитории

Свойства
Для того чтобы можно было легче писать и настраивать проект в pom.xml можно использовать свойства. Можно рассматривать свойства просто как переменные.

Есть:

Переменные объявленные внутри pom.xml
Предопределённые переменные.
Переменные объявленные во внешнем файле
Переменные объявленные внутри pom.xml
Давайте начнём с самого простого объявим свойства и сами будем их использовать. Свойства можно объявить и использовать так:

                    
    <properties>                             
        <temp.directory>/tmp</temp.directory>
    </properties>
    <build>
    <outputDirectory>${temp.directory}<outputDirectory>

                    
                
Свойства помогают избавиться от дублирования информации В примере

                    
        <properties>
            <jetty.version>6.1.25</jetty.version>
        </properties>
        <dependency>
            <groupId>org.mortbay.jetty</groupId>
            <artifactId>jetty</artifactId>
            <version>${jetty.version}</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.mortbay.jetty</groupId>
            <artifactId>jetty-util</artifactId>
            <version>${jetty.version}</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.mortbay.jetty</groupId>
            <artifactId>jetty-management</artifactId>
            <version>${jetty.version}</version>
            <scope>provided</scope>
        </dependency>
                            
                
использование свойства jetty.version позволяет избавиться от дублирования и уменьшет вероятность ошибок при апгрейде.
Предопределёные переменные
Предопределённые переменные можно разделить на несколько видов.
Встроенные свойства

${basedir}
директория где лежит pom.xml
${version}
тоже самое что и ${project.version} или ${pom.version}
Свойства проекта На все свойства в pom.xml, можно сослаться с помощью префиксов project. или pom. Ниже приведён пример некоторых часто используемых элементов.

${project.build.directory}
ваша "target" директория,или тоже самое
${pom.project.build.directory}
${project.build.outputDirectory}
путь к директории куда компилятор складывает файлы по умолчанию "target/classes"
${project.name}
или ${pom.name} имя Вашего проекта
${project.version}
или ${pom.version} версия Вашего проекта.
Настройки пользователя Можно получить доступ к свойствам settings.xml с помощью префикса settings. ,например:

${settings.localRepository} путь к локальному репозиторию пользователя.
Переменные окружения Для доступа к переменным окружения используйте префикс env. Примеры:

${env.M2_HOME}
путь .
${java.home}
specifies the path to the current JRE_HOME environment use with relative paths to get for example: <jvm>${java.home}../bin/java.exe</jvm>
Системные свойства System.properties Доступ к системным свойствам возможен напрямую. Для просмотра переменных можно воспользоваться maven-echo-plugin.

Переменные объявленные во внешнем файле
Для того чтобы загрузить переменные из внешнего файла удобнее всего использовать maven-properties-plugin Давайте рассмотрим его работу.
                
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>maven-properties-plugin</artifactId>
                <version>1.0-SNAPSHOT</version>
                <executions>
                    <execution>
                        <phase>initialize</phase>
                        <goals>
                            <goal>read-project-properties</goal>
                        </goals>
                        <configuration>
                            <files>
                                <file>src/config/app.properties</file>
                            </files>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
                            
            
В данном примере объявляется плагин, он отрабатывает на стадии initialize и загружает свойства из src/config/app.properties.