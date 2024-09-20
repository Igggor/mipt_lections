- *Декоратор* - функция, которая принимает на вход другую функцию и дополняет ее функционал
## Пример Декоратора

```python
def my_decorator(func):
	def wrapper(*args, **kwargs):
		print("start function")
		result = func(*args, **kwargs)
		print("function finished")
		return result
	return wrapper

@my_decorator
def do_sth():
	print("sth_done")

do_sth()
```

- В выводе получаем

```output
start function
do_sth
function finished
```

## Синтаксический сахар
- Альтернативное использование декоратора. Не самое правильное

```python
def my_decorator(func):
	def wrapper(*args, **kwargs):
		print("start function")
		result = func(*args, **kwargs)
		print("function finished")
		return result
	return wrapper

#@my_decorator - тут декоратор закомментрирован.
#При вызове функции просто так, не будет выполнен дополнительный функционал
def do_sth():
	print("sth_done")

a = my_decorator(do_sth)
a()
```

- В выводе получаем тот же результат

```output
start function
do_sth
function finished
```

## Подмена
- При вызове декорированной функции будет вызываться функция-декоратор. В [[#^e5d033|сигнатуре]] получаем имя функции-декоратора

```python
def my_decorator(func):
	def wrapper(*args, **kwargs):
		print("start function")
		result = func(*args, **kwargs)
		print("function finished")
		return result
	return wrapper

@my_decorator
def do_sth():
	print("sth_done")

do_sth.__name__
```

- В выводе получаем

```output
'wrapper'
```

- Что показывает, что функция *do_sth* запускалась из под функции *wrapper*

---

## Wraps
- В библиотеке **functools** лежит декоратор **wraps**, который позволяет избежать подмены сигнатуры

```python
from functools import wraps

def my_decorator(func):
	@wraps(func)
	def wrapper(*args, **kwargs):
		print("start function")
		result = func(*args, **kwargs)
		print("function finished")
		return result
	return wrapper

@my_decorator
def do_sth():
	print("sth_done")

do_sth.__name__
```

- В выводе получаем

```output
'do_something'
```
- **wrap** сохраняет сигнатуру функции
- ***Сигнатура*** - это то, как называется функция, данные, об ее переменных ^e5d033

## Момент выполнения декораторов

- Декораторы выполняются в момент запуска файла. 

```python
def decorate(func):
	print('func decorate')
	return func

@decorate
def do_sth() -> None:
	print("sth done")

@decorate
def do_another_sth() -> None:
	print('do another sth')
```

- В выводе получаем

```output
run decorate
run decorate
```


---
## Композиция Декораторов

- Декораторы можно составлять в композиции декораторов. То есть передавать в декоратор другой декоратор.

```python
def outer(func):
	def wrapper(*args, **kwargs):
		print("outher")
		result = func(*args, **kwargs)
		return result
	return wrapper

def inner(func):
	def wrapper(*args, **kwargs):
		print("inner")
		result = func(*args, **kwargs)
		return result
	return wrapper

@my_decorator
def do_sth():
	print("sth_done")

do_sth()
```

- Проведем *композицию декораторов*

```python
@outer
@inner
def do_sth() -> None:
	print("sth done")

do_sth()
```

- В выводе получаем

```output
outer
inner
sth done
```

### Либо же можем сделать

```python
def do_sth():
	print('sth done')
do = outer(inner(do_sth))
do()
```

- В результате в консоли увидим то же самое
```output
outer
inner
sth done
```

----

## Параметризованный декоратор
- В декораторе мы можем запрашивать переменные

```python
from functools import wraps
from typing import Callable

def print_greeting(greeting: str) -> Callable:
	def _print_greeting(func):
		@wraps(func)
		def _wrapper(*args, **kwargs):
			print(f"{greeting}
			{func.__name__}")
			result = func(...)
			return result
		return _wrapper
	return _print_greeting

@print_greeting("hello World!!!")
def do_sth():
	print("sth done")

do_sth()

```

- В выводе получим

```output
Hello, world!!! do_sth
sth done
```

