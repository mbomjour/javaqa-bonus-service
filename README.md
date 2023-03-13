# Система сборки Maven и тесты на JUnit5
Нашей целью будет переделать проект с [приложением про бонус с покупки](https://github.com/mbomjour/javaqa-mobile-bonus/blob/main/src/Main.java) на Maven и хорошо его протестировать.

1. Создайте проект на базе Maven.
2. Добавьте в проект JUnit Jupiter & Surefire Plugin.
3. Создайте сервисный класс со следующим исходным кодом:
```java
public class BonusService {
  public long calculate(long amount, boolean registered) {
    int percent = registered ? 3 : 1;
    long bonus = amount * percent / 100;
    long limit = 500;
    if (bonus > limit) {
      bonus = limit;
    }
    return bonus;
  }
}
```
4. Создайте тестовый класс со следующим исходным кодом:
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.Assertions;

public class BonusServiceTest {

  @Test
  void shouldCalculateForRegisteredAndUnderLimit() {
    BonusService service = new BonusService();

    // подготавливаем данные:
    long amount = 1000;
    boolean registered = true;
    long expected = 30;

    // вызываем целевой метод:
    long actual = service.calculate(amount, registered);

    // производим проверку (сравниваем ожидаемый и фактический):
    Assertions.assertEquals(expected, actual);
  }

  @Test
  void shouldCalculateForRegisteredAndOverLimit() {
    BonusService service = new BonusService();

    // подготавливаем данные:
    long amount = 1_000_000;
    boolean registered = true;
    long expected = 500;

    // вызываем целевой метод:
    long actual = service.calculate(amount, registered);

    // производим проверку (сравниваем ожидаемый и фактический):
    Assertions.assertEquals(expected, actual);
  }
}
```
5. Запустите тесты через `mvn clean test`, убедитесь, что они запускаются и проходят.
6. Проведите поверхностный тест-дизайн сервисного класса, допишите как минимум два недостающих и прямо напрашивающихся теста.
7. Убедитесь, что тесты запускаются и проходят.

## Дополнительное задание
Давайте подключим плагин, который бы автоматически проверял наличие в вашем коде неправильно названных локальных переменных и обрушал бы сборку, если таковые были бы обнаружены.

Сперва создадим подобный дефект. Перейдите в класс `BonusService` и переименуйте переменную в нём с `percent` на `Percent`. Убедитесь, что код компилится, тесты проходят.

Запустите `mvn clean test`. Сборка должна упасть, тк плагин должен найти эту ошибку именования. 

Сделайте коммит с комментарием `Rename percent to Percent` и запушьте изменения на гитхаб.
Создайте баг-репорт на основе Github Issues. Добавьте в баг-репорт ещё один раздел для логов сборки. 

После создания баг-репорта, переключитесь на IDEA, исправьте дефект, убедитесь что сборка проходит успешно, сделайте коммит с сообщением `Fix #1` и пуш, где `1` это номер баг-репорта, который будет закрыт автоматически при пуше этого коммита в основную ветку.
