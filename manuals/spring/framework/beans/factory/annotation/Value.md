# @Value

Пример
```java
@Value("${app.timeout}") 
private int timeout;
```

Пример с дефолтным значением
```java
@Value("${app.host:localhost}")
private String host;
```

Использование Spring Expression Language (SpEL)
```java
// Получение системного свойства Java
@Value("#{systemProperties['user.home']}")
private String userHome;
```

Аннотацию можно размещать прямо в параметрах конструктора, а также автоматически преобразовывать строки через запятую в массивы или коллекции
```java
private final List<String> adminEmails;

public SecurityConfig(@Value("${app.admins}") List<String> adminEmails) { 
	this.adminEmails = adminEmails; 
}
```

Так не получится.
Дело в том, что аннотация **`@RequiredArgsConstructor`** (из Lombok) автоматически генерирует конструктор только для полей, помеченных как `final`. 
Обычно добавление `final` не решает проблему.
```java
@Service 
@RequiredArgsConstructor 
public class MailSenderService {

	@Value("${smtp.timeout-millis}") 
	private int smtpTimeoutMillis;
	 
	private final MailConfigService mailConfigService;
```

Решение
```java
@Service
public class MailSenderService {

	private final int smtpTimeoutMillis;
	private final MailConfigService mailConfigService;

	// Пишем конструктор вручную, чтобы Spring корректно связал параметры
	public MailSenderService(
			@Value("${vsd.smtp.timeout-millis}") 
			int smtpTimeoutMillis,
			MailConfigService mailConfigService) {
		this.smtpTimeoutMillis = smtpTimeoutMillis;
		this.mailConfigService = mailConfigService;
	}
}
```
