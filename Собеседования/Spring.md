---
tags: interview, spring
---
### Общее

Зачем нужен Spring Framework?
1. Для управления зависимостями, Dependenci Injection (внедрение зависимостей, инверсия контроля)
2. Есть много компонентов, которые ускоряют написание приложений в разы

Компоненты Spring Framework
![[spring_framework_components.jpg|700]]

Скоупы бинов:
* Singleton - по умолчанию
* Prototype - множественные, создаются по запросу (?)
* Привязанные к сессии, чего-либо ещё...

Цикл жизни бинов
![[spring_bean_lifecycle.jpg|700]]

Различия компонентных аннотаций
`@Controller`, `@Service`, `@Repository`, `@Configuration` - наследуются от `@Component`
* `@Repository` - позволяет транслировать исключения из запросов в обрабатываемые спрингом исключения
* `@Controller` - ...
* `@Service` - ничего не добавляет, не делает, просто помечать класс с бизнес-логикой
* `@Configuration` - используется в основном для создания других бинов

разница spring и spring boot - в boot куча настроек по умолчанию есть и не надо копаться. Спринг это просто фреймворк, спринг бут это фреймворк с запускатором
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