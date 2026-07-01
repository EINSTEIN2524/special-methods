#  Python Special Methods (Dunder Methods)

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

