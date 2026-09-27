![maven.png](../files/maven.png)

# Maven

```bash
----------Жизненный цикл сборки: фазы------------
# Проверить корректность проекта и наличие всей необходимой информации
mvn validate

# Скомпилировать исходный код проекта
mvn compile

# Запустить unit-тесты (не требуют упаковки или развёртывания)
mvn test

# Упаковать скомпилированный код в распространяемый формат (JAR, WAR)
mvn package

# Выполнить интеграционные тесты (развернуть пакет в окружении)
mvn integration-test

# Проверить, что пакет валиден и соответствует критериям качества
mvn verify

# Установить пакет в локальный репозиторий (для использования другими проектами)
mvn install

# Скопировать финальный пакет в удалённый репозиторий (для команды)
mvn deploy

----------Жизненный цикл сборки: очистка------------
# Удалить файлы, созданные предыдущей сборкой (папку target)
mvn clean

# Выполнить очистку с последующей установкой
mvn clean install

----------Жизненный цикл сборки: сайт------------
# Сгенерировать документацию проекта (сайт)
mvn site

----------Команды: сборка и упаковка------------
# Собрать проект без запуска тестов
mvn -DskipTests package

# Собрать проект с полным пропуском компиляции тестов
mvn -Dmaven.test.skip=true package

# Собрать проект с параллельным выполнением (4 потока)
mvn -T 4 clean install

# Собрать проект из другого каталога
mvn -f dir/pom.xml package

# Собрать проект в offline-режиме (без обращения к удалённым репозиториям)
mvn -o package

----------Команды: диагностика------------
# Показать дерево зависимостей проекта
mvn dependency:tree

# Проанализировать неиспользуемые и необъявленные зависимости
mvn dependency:analyze

# Показать активные профили для текущего проекта
mvn help:active-profiles

# Показать эффективный settings.xml (объединённый global + user)
mvn help:effective-settings

----------Команды: создание проекта------------
# Создать новый проект по архетипу (JAR)
mvn archetype:generate -DgroupId=com.example -DartifactId=my-app

# Создать новый web-проект по архетипу (WAR)
mvn archetype:generate -DgroupId=com.example -DartifactId=my-webapp -DarchetypeArtifactId=maven-archetype-webapp

----------Команды: вывод и отладка------------
# Показать версию Maven
mvn -v

# Показать версию Maven и продолжить сборку
mvn -V package

# Тихий режим (только ошибки)
mvn -q package

# Режим отладки (все сообщения)
mvn -X package

# Показать справку по командам
mvn -help

----------POM: обязательные координаты------------
# Уникальный идентификатор организации или группы проекта
<groupId>com.example</groupId>

# Уникальный идентификатор артефакта в группе
<artifactId>my-app</artifactId>

# Версия артефакта
<version>1.0.0-SNAPSHOT</version>

# Формат упаковки (jar, war, pom)
<packaging>jar</packaging>

----------POM: свойства------------
# Кодировка исходных файлов
<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

# Версия Java для компиляции
<maven.compiler.source>17</maven.compiler.source>
<maven.compiler.target>17</maven.compiler.target>

# Пользовательское свойство (используется как ${my.version})
<my.version>1.0</my.version>

----------POM: зависимости------------
# Зависимость, доступная на всех classpath и передаваемая дальше
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-lang3</artifactId>
  <version>3.13.0</version>
</dependency>

# Зависимость, предоставляемая контейнером (например, Servlet API)
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>4.0.1</version>
  <scope>provided</scope>
</dependency>

# Зависимость, не требуемая при компиляции, но нужная при запуске
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-j</artifactId>
  <scope>runtime</scope>
</dependency>

# Зависимость, используемая только в тестах
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.13.2</version>
  <scope>test</scope>
</dependency>

----------POM: области видимости зависимостей------------
# Область по умолчанию — доступна везде, передаётся зависимым проектам
compile

# Область для API, предоставляемых JDK или контейнером — не передаётся
provided

# Область для зависимостей, нужных только во время выполнения
runtime

# Область для тестовых зависимостей — не передаётся
test

# Область для локальных JAR-файлов, указанных явно
system

# Область для импорта dependencyManagement из другого POM
import

----------POM: плагины------------
# Плагин компилятора (настройка версии Java)
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-compiler-plugin</artifactId>
  <version>3.11.0</version>
  <configuration>
    <source>17</source>
    <target>17</target>
  </configuration>
</plugin>

# Плагин Spring Boot (сборка исполняемого JAR)
<plugin>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>

----------POM: сборка------------
# Имя финального артефакта (без версии)
<finalName>${project.artifactId}-${project.version}</finalName>

# Управление версиями плагинов (наследуется дочерними проектами)
<build>
  <pluginManagement>
    <!-- версии плагинов здесь -->
  </pluginManagement>
</build>

----------settings.xml: расположение------------
# Глобальные настройки (для всех пользователей)
${maven.home}/conf/settings.xml

# Пользовательские настройки (переопределяют глобальные)
${user.home}/.m2/settings.xml

----------settings.xml: профили------------
# Активировать профиль по умолчанию
<activeByDefault>false</activeByDefault>

# Активировать профиль при определённой версии JDK
<jdk>17</jdk>

# Активировать профиль при определённой ОС
<os>
  <family>windows</family>
</os>

# Активировать профиль при наличии свойства
<property>
  <name>env</name>
  <value>production</value>
</property>

# Активировать профиль при наличии/отсутствии файла
<file>
  <exists>${basedir}/src/main/resources/prod.properties</exists>
</file>

# Принудительно активировать профиль (по id)
<activeProfile>production</activeProfile>

----------settings.xml: репозитории------------
# Удалённый репозиторий для зависимостей
<repository>
  <id>central</id>
  <url>https://repo.maven.apache.org/maven2</url>
</repository>

# Удалённый репозиторий для плагинов
<pluginRepository>
  <id>central</id>
  <url>https://repo.maven.apache.org/maven2</url>
</pluginRepository>

# Политика обновления (always, daily, interval:X, never)
<updatePolicy>daily</updatePolicy>

# Политика проверки контрольных сумм (ignore, fail, warn)
<checksumPolicy>warn</checksumPolicy>

----------settings.xml: свойства------------
# Свойство с префиксом env (переменная окружения)
${env.PATH}

# Свойство с префиксом project (значение из POM)
${project.version}

# Свойство с префиксом settings (значение из settings.xml)
${settings.localRepository}

# Системное свойство Java
${java.home}

# Пользовательское свойство без префикса
${my.custom.property}

----------settings.xml: активация профиля из CLI------------
# Активировать профиль по id при запуске
mvn -P production package

# Активировать несколько профилей
mvn -P production,adobe-public package
```

### Указать имя артефакта

```xml
<project>
	<!-- . . . -->
	<build>
		<!-- Укажите желаемое имя без расширения файла (.jar или .war) -->
		<finalName>my-custom-artifact-name</finalName>
	</build>
</project>
```

### Указать манифест

Создает тонкий jar

```xml
 <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.5.0</version>
    <configuration>
        <archive>
            <manifest>
                <addClasspath>true</addClasspath>
                <!-- Укажите ваш класс с методом main -->
                <mainClass>com.yourpackage.YourMainClass</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
```

### Сборка fat-jar

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>3.4.1</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                <mainClass>com.yourpackage.MainClass</mainClass>
                            </transformer>
                        </transformers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```
