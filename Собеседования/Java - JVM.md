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

GC чистит память от объектов, которые больше не используются.

Виды GC:
* Serial GC - работает в один поток, "замораживает" приложение, когда выполняется
* Parallel GC - использует несколько потоков, но тоже "замораживает" другие потоки
* CMS GC - использует несколько потоков, ..., уже deprecated
* G1 GC - запускается на многопроцессорных машинах... designed for applications running on multi-processor machines with large memory space.
* Z GC - ZGC performs all expensive work concurrently, without stopping the execution of application threads for more than 10 ms, which makes it suitable for applications that require low latency.

---

[[!Теория для собеседования]]