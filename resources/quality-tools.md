# Контроль качества кода (formatter, linter, тесты)

На курсе перед сдачей практики нужно:

1. отформатировать код;
2. прогнать статический анализ / проверку стиля;
3. покрыть ключевую логику базовыми unit-тестами.

Ниже — практическая настройка. Достаточно выбрать **один** путь и придерживаться его весь семестр.

**Рекомендуемый стек:**

| Инструмент | Роль | Вариант на курсе |
| ---------- | ---- | ---------------- |
| Formatter | единый стиль отступов и переносов | Google Java Format (IDE или Spotless) |
| Linter | стиль и типичные ошибки | Checkstyle (`google_checks.xml`) |
| Tests | проверка логики | JUnit 5 |

Для практик **15–16** и РГР удобнее сразу **Maven**. Для ранних практик можно начать с IDE, а к Maven перейти позже.

---

## 1. Форматирование кода (formatter)

### Вариант A. IntelliJ IDEA (с практики 1)

1. `Settings → Editor → Code Style → Java → Scheme → Set from… → Google Style`  
   (или загрузите [GoogleStyle.xml](https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml)).
2. Включите форматирование при сохранении:  
   `Settings → Tools → Actions on Save → Reformat code`.
3. Перед коммитом: выделите проект → `Code → Reformat Code` (`Ctrl+Alt+L`).

В VS Code / Cursor: расширение **Language Support for Java**, в настройках `java.format.settings.url` укажите URL Google Style XML; форматирование — `Shift+Alt+F`.

### Вариант B. Maven + Spotless (рекомендуется с Maven-проекта)

В `pom.xml` (фрагмент):

```xml
<build>
  <plugins>
    <plugin>
      <groupId>com.diffplug.spotless</groupId>
      <artifactId>spotless-maven-plugin</artifactId>
      <version>2.44.5</version>
      <configuration>
        <java>
          <googleJavaFormat/>
        </java>
      </configuration>
    </plugin>
  </plugins>
</build>
```

Команды:

```bash
mvn spotless:apply   # привести код к стилю
mvn spotless:check   # только проверка (удобно в CI)
```

---

## 2. Статический анализ и стиль (linter)

### Вариант A. Inspections в IntelliJ

1. `Settings → Editor → Inspections` — оставьте включёнными предупреждения по Java (unused, NPE-риски, empty blocks и т. п.).
2. `Code → Inspect Code…` по модулю/пакету перед сдачей.
3. Исправьте **Error** и по возможности **Warning** в своём коде (не в сгенерированных файлах).

### Вариант B. Maven + Checkstyle

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-checkstyle-plugin</artifactId>
  <version>3.6.0</version>
  <configuration>
    <configLocation>google_checks.xml</configLocation>
    <consoleOutput>true</consoleOutput>
    <failsOnError>true</failsOnError>
  </configuration>
</plugin>
```

Команда:

```bash
mvn checkstyle:check
```

Правила `google_checks.xml` входят в плагин — отдельный файл не обязателен.  
Если правило мешает учебной задаче — ослабьте его осознанно в конфиге, а не отключайте проверку целиком.

---

## 3. Unit-тесты (JUnit 5)

Тестируйте **ключевую логику** (методы расчёта, проверки условий, работу коллекций), а не `main` с `Scanner`.

### Maven: зависимости

```xml
<dependencies>
  <dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.12.2</version>
    <scope>test</scope>
  </dependency>
</dependencies>

<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-surefire-plugin</artifactId>
      <version>3.5.3</version>
    </plugin>
  </plugins>
</build>
```

### Структура

```text
src/main/java/.../Task34.java
src/test/java/.../Task34Test.java
```

### Пример теста

```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.api.Test;

class Task34Test {
  @Test
  void perimeterOfSquare() {
    assertEquals(20.0, Task34.perimeter(5.0), 1e-9);
  }
}
```

Запуск:

```bash
mvn test
```

В IntelliJ: зелёная стрелка у класса/метода теста или `Ctrl+Shift+F10`.

**Минимум на практику:** 2–5 осмысленных тестов на основные методы (нормальный случай + граничный/ошибочный, если есть проверки входных данных).

---

## 4. Рекомендуемый ритуал перед сдачей

```bash
mvn spotless:apply
mvn checkstyle:check
mvn test
git add -A
git commit -m "practice-03: format, checkstyle, tests"
git push
```

Без Maven:

1. `Reformat Code` по проекту;
2. `Inspect Code` — исправить замечания;
3. прогнать тесты в IDE;
4. закоммитить и запушить на GitHub.

В отчёте достаточно кратко указать: «код отформатирован (Google Style / Spotless), Checkstyle без ошибок, `mvn test` — OK» (или скрин/лог в приложении, если просит преподаватель).

---

## 5. Минимальный `pom.xml` «под ключ»

Если проект ещё без сборщика — можно перейти на Maven с таким каркасом (JDK 25; для 21 замените `release`):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>edu.dvgups.java</groupId>
  <artifactId>practices</artifactId>
  <version>1.0-SNAPSHOT</version>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.release>25</maven.compiler.release>
    <junit.version>5.12.2</junit.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter</artifactId>
      <version>${junit.version}</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.14.0</version>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.5.3</version>
      </plugin>
      <plugin>
        <groupId>com.diffplug.spotless</groupId>
        <artifactId>spotless-maven-plugin</artifactId>
        <version>2.44.5</version>
        <configuration>
          <java>
            <googleJavaFormat/>
          </java>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-checkstyle-plugin</artifactId>
        <version>3.6.0</version>
        <configuration>
          <configLocation>google_checks.xml</configLocation>
          <consoleOutput>true</consoleOutput>
          <failsOnError>true</failsOnError>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

Команды одной строкой:

```bash
mvn spotless:apply checkstyle:check test
```
