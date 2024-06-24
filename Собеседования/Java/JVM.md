---
tags:
  - interview
  - java
---
### Устройство памяти - TODO

1. Heap (куча) - динамически распределяемая область памяти, создаваемая при старте JVM. Используется Java Runtime для выделения памяти под объекты и JRE классы.
2. Stack (стековая память) - работает по схеме LIFO. Когда вызывается метод, в памяти стека создаётся новый блок, который содержит примитивы и ссылки на другие объекты в методе.

потоки - почему используют ресурсы (сигналы, системный процесс, а не пользовательский ?)

---

### Garbage Collectors - TODO

https://www.baeldung.com/java-gc-cyclic-references - todo

GC чистит память от объектов, которые больше не используются.

Виды GC:
* Serial GC - работает в один поток, "замораживает" приложение, когда выполняется
* Parallel GC - использует несколько потоков, но тоже "замораживает" другие потоки. Стоит по умолчанию в Java 8.
* CMS GC - использует несколько потоков, ..., уже deprecated
* G1 GC - запускается на многопроцессорных машинах... designed for applications running on multi-processor machines with large memory space. Стоит по умолчанию в Java 9.
* Z GC - ZGC performs all expensive work concurrently (параллельно), without stopping the execution of application threads for more than 10 ms, which makes it suitable for applications that require low latency (задержку). Появился в Java 11. Готов к промышленному применению, начиная с JDK 15.

---

### Побитовые операторы - TODO

Источник: https://javarush.com/groups/posts/operatory-java-logicheskie-arifmeticheskie-pobitovye#Логические-операции-в-Java

---

[[!Теория для собеседования]]
[[!Java]]