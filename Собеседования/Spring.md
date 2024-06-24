---
tags: interview, spring
---
### Общее

Зачем нужен Spring Framework?
1. Для управления зависимостями, Dependency Injection (внедрение зависимостей, инверсия контроля)
2. Есть много компонентов, которые ускоряют написание приложений в разы

##### Компоненты Spring Framework

![[spring_framework_components.jpg|700]]

##### IoC Container and Beans - TODO

Источник: https://docs.spring.io/spring-framework/reference/core/beans/introduction.html

##### Скоупы бинов - TODO

Источник: https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html

**Singleton**
По умолчанию.
Один bean definition на один экземпляр объекта для каждого Spring IoC контейнера.

Контекстом управляется только один общий экземпляр singleton-бина.
На все запросы бинов, удовлетворяющих bean definition singleton-бина, будет возвращён один и тот же экземпляр бина.

Один singleton-бин на один Spring IoC контейнер.

**Prototype**
Один bean definition на любое количество экземпляров объекта.

На каждый запрос prototype-бина создаётся новый экземпляр бина.
Запрос бина - либо внедрение (inject) в другой бин, либо вызов метода `getBean()` у контейнера.

*Правило*: используйте скоуп prototype для stateful бинов (с сохранением состояния) и скоуп singleton для stateless бинов (без сохранения состояния).

В отличие от других скоупов, Spring не управляет полным жизненным циклом prototype-бина. Контейнер создает экземпляр, настраивает и иным образом собирает prototype-объект и передает его клиенту без дальнейшей записи об этом экземпляре.
Методы lifecycle callback инициализации вызываются для всех объектов независимо от скоупа. У prototype настроенные методы lifecycle callback уничтожения не вызываются. Ответственность за очищение объектов скоупа prototype, освобождение дорогостоящие ресурсов в этих объектах лежит на клиентском коде. Для этого можно использовать кастомный bean post-processor, который содержит ссылку на компоненты, которые необходимо очистить.
В некотором отношении роль контейнера Spring в отношении компонента со скоупом prototype - замена оператора Java `new`. Все управление жизненным циклом после этого момента должно выполняться клиентом.

Если в singleton-бине нужно использовать protoype-бин, но каждый раз новый, нужно использовать [Method Injection](https://docs.spring.io/spring-framework/reference/core/beans/dependencies/factory-method-injection.html).

<u>Далее скоупы, допустимые только в ApplicationContext веб-приложения Spring.</u>

**Request**
Один bean definition на жизненный цикл одного HTTP-запроса. Каждый HTTP-запрос имеет свой собственный экземпляр компонента, созданный на основе одного bean definition.

**Session**
Один bean definition на жизненный цикл одной HTTP-сессии.

**Application**
Один bean definition на жизненный цикл `ServletContext`.

**Websocket**
Один bean definition на жизненный цикл одного `WebSocket`.

##### Цикл жизни бина

![[spring_bean_lifecycle.jpg|700]]

##### Аннотации

Различия компонентных аннотаций
`@Controller`, `@Service`, `@Repository`, `@Configuration` - наследуются от `@Component`
* `@Repository` - позволяет транслировать исключения из запросов в обрабатываемые спрингом исключения
* `@Controller` - ...
* `@Service` - ничего не добавляет, не делает, просто помечать класс с бизнес-логикой
* `@Configuration` - используется в основном для создания других бинов

##### BeanPostProcessor - TODO

##### Spring vs Spring Boot

Spring Boot - расширение Spring. Берёт на себя настройку шаблонных конфигураций Spring.
- Стартеры - для упрощения сборки и настройки приложения
- Встроенный сервер - позволяет избежать сложностей при развертывании приложений
- Автоматическая настройка функциональности Spring - всегда, когда возможно

В Spring Boot есть куча настроек по умолчанию, не надо копаться. Spring - это просто фреймворк, Spring Boot - это фреймворк с запускатором.
Spring Boot - надстройка над Spring, которая автоматически конфигурирует некоторые компоненты, например Jackson.
Чтобы создать важные компоненты, например DataSource, нужно лишь прописать несколько свойств в специальный файл, остальное возьмёт на себя Spring Boot.
Также в Spring Boot встроен Tomcat - в Spring нужно отдельно настраивать servlet-контейнер.
В аннотации `@SpringBootApplication` уже встроены другие аннотации:
```java
@Target(ElementType.TYPE)  
@Retention(RetentionPolicy.RUNTIME)  
@Documented  
@Inherited  
@SpringBootConfiguration  
@EnableAutoConfiguration  
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),  
      @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
```
Spring Boot имеет starter-зависимости для лёгкой конфигурации компонентов:
* spring-boot-starter-data-jpa
* spring-boot-starter-security
* spring-boot-starter-test
* spring-boot-starter-web
* spring-boot-starter-thymeleaf

---

### Spring MVC

Всё крутится вокруг `DispatcherServlet`, проходит несколько фаз:
1. Handler Mapping
2. Controller
3. View Resolver
4. View
После чего объект отображается на той стороне

`@Controller` vs `@RestController`
`@RestController` = `@Controller` + `@ResponseBody`

---

### Spring Data

Spring Repository - интерфейсы, которые используют `Entity` для взаимодействия с БД.

Способы создания методов для получения данных:
1. В `Repository` в названии метода указать поля, по которым нужно фильтровать, передать соответствующие параметры в метод
2. Аннотация `@Query`
3. Создать спецификацию - функциональный интерфейс с методом `Predicate toPredicate(Root<T> root, CriteriaQuery<?> query, CriteriaBuilder builder);` - Criteria API

`@Transactional`
Нужна для обеспечения атомарности операций при взаимодействии с БД.
Основные свойства:
* Propagation:
	* REQUIRED - должно выполняться в транзакции, если её нет - создать
	* SUPPORTS - если транзакция есть, используем её, иначе - без транзакции
	* MANDATORY - обязательно наличие активной транзакции, иначе - исключение
	* REQUIRES_NEW - останавливает текущую транзакцию, если она есть, исполняется в отдельной транзакции
	* NOT_SUPPORTED - останавливает текущую транзакцию, если она есть, исполняется не транзакционно
	* NEVER - обязательно отсутствие транзакции, иначе - исключение
	* NESTED - если было исключение, возвращается к savepoint
* Isolation
* readOnly - несколько оптимизирует запросы, но не гарантирует, что нельзя делать операции записи
* rollbackFor - для каких исключений откатывать транзакцию
* noRollbackFor - для каких исключений не откатывать транзакцию

2 метода - 1 с `@Transactional`, 2 без. Из 2 вызывается 1, будет ли он транзакционным? - нет:
Бины проксируются - когда помечаем класс аннотацией, спринг делает прокси класса, чтобы выполнить сквозную логику (AOP) - `@Transactional` выполняет какую-то логику до и после выполнения самого метода
В данной ситуации `@Transactional` не отработает, т.к. будет вызван сам метод, а не метод прокси с доп. логикой.

JpaRepository, CrudRepository

Транзакциями можно управлять программно (`TransactionTemplate`).

---

### Spring Security

Теория

---

### Spring Cloud

Теория

---

[[!Теория для собеседования]]