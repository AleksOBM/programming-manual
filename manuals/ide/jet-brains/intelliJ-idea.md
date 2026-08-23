![IntelliJ IDEA](../../../files/intelliJ-idea.png)

# IntelliJ IDEA

### Убрать предупреждение "Similar log messages"

Чтобы отключить предупреждение проверки IntelliJ IDEA «Похожие сообщения журнала» в Java, поместите комментарий `//noinspection LoggingSimilarMessage` непосредственно над конкретным оператором ведения журнала или отключите проверку глобально в настройках IDE, если вы не хотите ее видеть.

Пример

```java
//noinspection LoggingSimilarMessage
logger.info("Processing user with id: {}", userId);
```
