## Замыкания
- Функция с расширенной областью видимости, может обращаться к переменным, не являющимся для нее локальной
```python
from typing import Callable

def make_counter() -> Callable[[], int]:
	counter = 0
	def count() -> int:
		nonlocal counter
		counter += 1
		return counter
	return count
```
- Функция **count()** является замыканием
- Функция **make_counter()** является фабрикой замыканий
### Можем запустить

```python
c1 = make_counter()
c2 = make_counter()
for i in range(3):
	c1()
for i in range(5):
	c2()
print(f"counter1: {c1()}")
print(f"counter2: {c2()}")
```

- Выходные данные

```output
counter1: 4
counter2: 6
```

### Где данные лежат

```python 
print(f"local vars: {c1.__code__.co_varnames}")
print(f"free vars: {c1.__code__.co_freevars}")
```

```output
local vars: ()
free vars: ('counter', )
```


---