# Maven

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
