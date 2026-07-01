# 🐍 Python Special Methods (Dunder Methods)

A hands-on reference and example repo covering Python's **special methods** (also called **magic methods** or **dunder methods** — short for "double underscore"). These methods let your custom classes hook into built-in Python syntax and behavior: `+`, `len()`, `print()`, `[]`, `in`, `()`, `for`, `if`, and more.

---

## 📑 Table of Contents

1. [Constructor](#1-constructor)
2. [String Representation](#2-string-representation)
3. [Length Method](#3-length-method)
4. [Equality Comparison](#4-equality-comparison)
5. [Operator Overloading](#5-operator-overloading)
6. [Item Access](#6-item-access)
7. [Item Assignment](#7-item-assignment)
8. [Membership Testing](#8-membership-testing)
9. [Callable Object](#9-callable-object)
10. [Iteration Support](#10-iteration-support)
11. [Boolean Evaluation](#11-boolean-evaluation)
12. [Destruction Method](#12-destruction-method)

---

## 1. Constructor

`__init__` runs automatically when a new object is created. It initializes the object's attributes.

```python
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author

b = Book("Dune", "Frank Herbert")
```

> Note: `__new__` is the *actual* object creator (called before `__init__`), but `__init__` is the one you'll override 99% of the time.

---

## 2. String Representation

`__str__` defines the "pretty" output for `print()` / `str()`. `__repr__` defines the unambiguous, developer-facing output (used in the REPL, debuggers, and as a fallback if `__str__` isn't defined).

```python
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author

    def __str__(self):
        return f"{self.title} by {self.author}"

    def __repr__(self):
        return f"Book(title={self.title!r}, author={self.author!r})"

b = Book("Dune", "Frank Herbert")
print(b)        # Dune by Frank Herbert
b               # Book(title='Dune', author='Frank Herbert')
```

---

## 3. Length Method

`__len__` lets your object work with the built-in `len()` function.

```python
class Playlist:
    def __init__(self, songs):
        self.songs = songs

    def __len__(self):
        return len(self.songs)

p = Playlist(["Song A", "Song B", "Song C"])
len(p)  # 3
```

---

## 4. Equality Comparison

`__eq__` defines what `==` means for your objects (by default, Python compares identity, not values).

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

Point(1, 2) == Point(1, 2)  # True
```

Related comparison dunders: `__ne__`, `__lt__`, `__le__`, `__gt__`, `__ge__` (or use `functools.total_ordering` to derive them all from `__eq__` + one other).

---

## 5. Operator Overloading

Dunders like `__add__`, `__sub__`, `__mul__`, and `__truediv__` let you customize how arithmetic operators behave on your objects.

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

Vector(1, 2) + Vector(3, 4)  # Vector(4, 6)
```

---

## 6. Item Access

`__getitem__` allows indexing and slicing with `obj[key]`.

```python
class Deck:
    def __init__(self, cards):
        self.cards = cards

    def __getitem__(self, index):
        return self.cards[index]

d = Deck(["A♠", "K♦", "Q♣"])
d[0]  # 'A♠'
```

---

## 7. Item Assignment

`__setitem__` allows assignment via `obj[key] = value`.

```python
class Deck:
    def __init__(self, cards):
        self.cards = cards

    def __setitem__(self, index, value):
        self.cards[index] = value

d = Deck(["A♠", "K♦", "Q♣"])
d[0] = "10♥"
```

---

## 8. Membership Testing

`__contains__` powers the `in` keyword.

```python
class Deck:
    def __init__(self, cards):
        self.cards = cards

    def __contains__(self, card):
        return card in self.cards

d = Deck(["A♠", "K♦", "Q♣"])
"K♦" in d  # True
```

---

## 9. Callable Object

`__call__` lets an instance be invoked like a function.

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, value):
        return value * self.factor

double = Multiplier(2)
double(5)  # 10
```

---

## 10. Iteration Support

`__iter__` (returns an iterator) and `__next__` (returns the next value, raises `StopIteration` when done) allow an object to be used in a `for` loop.

```python
class Countdown:
    def __init__(self, start):
        self.current = start

    def __iter__(self):
        return self

    def __next__(self):
        if self.current <= 0:
            raise StopIteration
        self.current -= 1
        return self.current + 1

for n in Countdown(3):
    print(n)  # 3, 2, 1
```

---

## 11. Boolean Evaluation

`__bool__` defines how an object behaves in truthy/falsy contexts (`if obj:`, `while obj:`). If undefined, Python falls back to `__len__` (nonzero = truthy), then defaults to `True`.

```python
class Cart:
    def __init__(self, items):
        self.items = items

    def __bool__(self):
        return len(self.items) > 0

cart = Cart([])
if not cart:
    print("Cart is empty")
```

---

## 12. Destruction Method

`__del__` runs when an object is about to be garbage collected. Useful for cleanup (closing files, releasing resources), but its timing isn't guaranteed — prefer context managers (`__enter__` / `__exit__`) for deterministic cleanup.

```python
class Connection:
    def __init__(self, name):
        self.name = name
        print(f"Opening connection: {self.name}")

    def __del__(self):
        print(f"Closing connection: {self.name}")

c = Connection("db1")
del c  # Closing connection: db1
```

---

## 📚 Quick Reference Table

| # | Method(s) | Trigger |
|---|-----------|---------|
| 1 | `__init__` | Object creation |
| 2 | `__str__`, `__repr__` | `print()`, `str()`, REPL |
| 3 | `__len__` | `len(obj)` |
| 4 | `__eq__` | `obj == other` |
| 5 | `__add__`, `__sub__`, etc. | `+`, `-`, `*`, `/`, ... |
| 6 | `__getitem__` | `obj[key]` |
| 7 | `__setitem__` | `obj[key] = value` |
| 8 | `__contains__` | `value in obj` |
| 9 | `__call__` | `obj(args)` |
| 10 | `__iter__`, `__next__` | `for x in obj` |
| 11 | `__bool__` | `if obj:` |
| 12 | `__del__` | garbage collection |

---

## 🤝 Contributing

Contributions, corrections, and additional examples are welcome — open an issue or submit a PR.

## 📄 License

MIT
