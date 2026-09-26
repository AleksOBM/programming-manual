![lombok.png](../files/lombok.png)

# Lombok

Настройки
```yml
----------Общие настройки------------
# Добавить аннотации nullability (NonNull/Nullable) в сгенерированный код
# (требует Lombok 1.18.38+; другие варианты: javax, jetbrains, eclipse)
lombok.addNullAnnotations = jspecify

# Добавить @Generated над сгенерированным классом
lombok.addLombokGeneratedAnnotation = true

# Добавлять @SuppressWarnings в методы
lombok.addSuppressWarnings = false

# Не искать настройки за пределами проекта
config.stopBubbling = true

----------Аксессоры------------
# Капитализация аксессоров по правилам JavaBeans
lombok.accessors.capitalization = beanspec

# Цепочка вызовов в сеттерах (возвращают this)
lombok.accessors.chain = true

# Fluent-аксессоры (без get/set, возвращают this)
lombok.accessors.fluent = true

# Добавлять префикс j к аксессорам
lombok.accessors.prefix += j

----------Билдеры------------
# Использовать префикс для методов билдера (меняет имена на withXxx())
lombok.builder.setterPrefix = with

# Добавить метод toBuilder() в сгенерированный билдер (для создания копий с изменениями)
lombok.builder.toBuilder = true

----------Конструкторы------------
# Хранить имена параметров конструктора
lombok.anyConstructor.addConstructorProperties = true

# Приватный конструктор без аргументов
lombok.noArgsConstructor.extraPrivate = true

----------Копируемые аннотации------------
# Копировать @JsonValue на сгенерированные методы
lombok.copyableAnnotations += com.fasterxml.jackson.annotation.JsonValue

# Копировать @Lazy на сгенерированные поля/параметры
lombok.copyableAnnotations += org.springframework.context.annotation.Lazy

# Копировать @Qualifier на сгенерированные поля/параметры
lombok.copyableAnnotations += org.springframework.beans.factory.annotation.Qualifier

# Копировать @Value на сгенерированные поля/параметры
lombok.copyableAnnotations += org.springframework.beans.factory.annotation.Value

# Копировать пользовательскую аннотацию
lombok.copyableAnnotations += anno.MyAnnotation

----------Контроль использования------------
# Запретить использование @SneakyThrows
lombok.sneakyThrows.flagUsage = ERROR

# Запретить или предупредить об использовании экспериментальных аннотаций
# (действует на все экспериментальные фичи сразу; нельзя точечно разрешить одну)
lombok.experimental.flagUsage = WARNING

----------EqualsAndHashCode------------
# Экуалс и хэшкод вызывают родителя
lombok.equalsAndHashCode.callSuper = call

# Экуалс и хэшкод не используют геттеры
lombok.equalsAndHashCode.doNotUseGetters = true

----------Поля по умолчанию (final, private)------------
# Делать поля final
lombok.accessors.makeFinal = true
lombok.fieldDefaults.defaultFinal = true

# Делать поля приватными
lombok.fieldDefaults.defaultPrivate = true

----------Логгеры------------
# Кастомная фабрика логгера
lombok.log.custom.declaration = com.example.MyLoggerFactory.createMyLog(TYPE)

# Имя поля логгера — logger
lombok.log.fieldName = logger

# Нестатические логгеры
lombok.log.fieldIsStatic = false

# Добавлять @SuppressFBWarnings для логгеров
lombok.extern.findbugs.addSuppressFBWarnings = true

----------NonNull------------
# @NonNull кидает NullPointerException (по умолчанию в JDK)
lombok.nonNull.exceptionType = jdk

----------ToString------------
# Тустринг вызывает родителя
lombok.toString.callSuper = call

# Тустринг не использует геттеры
lombok.toString.doNotUseGetters = true

# Тустринг без имён полей
lombok.toString.includeFieldNames = false

# В тустринг попадают только поля с @ToString.Include
lombok.toString.onlyExplicitlyIncluded = true

```
