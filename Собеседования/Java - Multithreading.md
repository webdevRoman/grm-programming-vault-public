---
tags:
  - interview
  - java
---
### Способы создания потока

1. Унаследовать `Thread`, переопределить метод `run`:
```java
class MyThread extends Thread {
	@Override
	public void run() {
		// do something
	}
}
```
2. Передать `Runnable` аргументом в конструктор `Thread`:
```java
Runnable task = () -> {
	// do something
};
Thread thread = new Thread(task);
thread.star();
```

---

### ExecutorService

Интерфейс, который описывает сервис для запуска `Runnable` и `Callable` задач. На вход принимает задачу в виде `Runnable` или `Callable`, возвращает Future, через `который` можно получить результат.

---

### TODO

future - что даёт - обработать результат
основной вызываемый класс - Runnable, Callable

---

[[!Теория для собеседования]]