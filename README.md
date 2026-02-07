# Python OOP: 
---

## 0) Python’da hamma narsa obyekt (va bu OOP’ni osonlashtiradi)

Python’da “oddiy” ko‘ringan narsalar ham obyekt: son, matn, ro‘yxat, funksiya, hatto klassning o‘zi.

### Misol 1: Son ham obyekt

```python
x = 13
print(type(x))          # <class 'int'>
print(x.bit_length())   # 4 (bu int obyektining metodi)
```

Bu shuni anglatadi: `x` faqat qiymat emas, ichida methodlari bor bo‘lgan obyekt.

### Misol 2: String ham obyekt

```python
s = "hello"
print(type(s))          # <class 'str'>
print(s.upper())        # HELLO
print(s.replace("e", "3"))  # h3llo
```

### Misol 3: Funksiya ham obyekt

```python
def add(a, b):
    return a + b

print(type(add))     # <class 'function'>
add.label = "sum"    # funksiyaga ham atribut qo‘shsa bo‘ladi
print(add.label)
```

**Xulosa:** Python’da OOP “qo‘shimcha narsa” emas. Tilning o‘zi obyektlar atrofida qurilgan.

---

## 1) Class va instance: shablon va real nusxa

**Class** - shablon.  
**Instance** - shu shablondan yasalgan konkret obyekt.

### Misol 1: Eng sodda class

```python
class User:
    pass

u = User()
print(u)
print(type(u))
```

### Misol 2: Ikki obyekt ikki xil

```python
class Lamp:
    pass

l1 = Lamp()
l2 = Lamp()
print(l1 is l2)  # False
```

---

## 2) `__init__` va atributlar: obyekt ichiga ma’lumot joylash

`__init__` obyekt yaratilganda ishga tushadi.

### Misol 1: Atribut berib yaratish

```python
class User:
    def __init__(self, username, age):
        self.username = username
        self.age = age

u = User("alim", 25)
print(u.username, u.age)
```

### Misol 2: Default qiymatlar

```python
class User:
    def __init__(self, username, age=18):
        self.username = username
        self.age = age

u1 = User("sara")
u2 = User("bek", 30)
print(u1.username, u1.age)  # sara 18
print(u2.username, u2.age)  # bek 30
```

**Pro tip:** Obyektning holati (state) - atributlar. Xulqi (behavior) - methodlar.

---

## 3) Methodlar: obyektning xulqi (behavior)

### Misol 1: Holatni o‘zgartirish

```python
class Counter:
    def __init__(self):
        self.value = 0

    def inc(self):
        self.value += 1

c = Counter()
c.inc()
c.inc()
print(c.value)  # 2
```

### Misol 2: Natija qaytarish

```python
class Rectangle:
    def __init__(self, w, h):
        self.w = w
        self.h = h

    def area(self):
        return self.w * self.h

r = Rectangle(3, 4)
print(r.area())  # 12
```

**Professional fikr:** Methodlar “ma’lumot + qoidalar”ni bitta joyda ushlab turadi. Shunda kod tarqoq bo‘lib ketmaydi.

---

## 4) Class atribut va instance atribut farqi

### Misol 1: Class atribut umumiy

```python
class User:
    role = "member"

    def __init__(self, username):
        self.username = username

u1 = User("a")
u2 = User("b")
print(u1.role, u2.role)

User.role = "vip"
print(u1.role, u2.role)
```

### Misol 2: Instance atribut alohida

```python
class User:
    def __init__(self, username):
        self.username = username

u1 = User("alim")
u2 = User("sara")
u1.username = "alim007"
print(u1.username)  # alim007
print(u2.username)  # sara
```

---

## 5) Mutable class atribut xatosi (eng ko‘p “kuydiradigan” joy)

### Misol 1: Yomon (hamma obyekt bitta listni bo‘lishadi)

```python
class Bad:
    items = []

b1 = Bad()
b2 = Bad()
b1.items.append(1)
print(b2.items)  # [1]
```

### Misol 2: Yaxshi (har obyektga alohida list)

```python
class Good:
    def __init__(self):
        self.items = []

g1 = Good()
g2 = Good()
g1.items.append(1)
print(g2.items)  # []
```

**Qoidani eslab qol:** `list/dict/set` kabi mutable narsani class atributga qo‘ymaslikka harakat qil.

---

## 6) `__repr__` (debug uchun) va “o‘qiladigan” obyekt

### Misol 1: `__repr__` bilan chiroyli ko‘rinish

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def __repr__(self):
        return "Product(name=%r, price=%s)" % (self.name, self.price)

print(Product("Mouse", 150))
```

### Misol 2: Loglarda yordam beradi

```python
class Task:
    def __init__(self, title, done=False):
        self.title = title
        self.done = done

    def __repr__(self):
        return "Task(title=%r, done=%s)" % (self.title, self.done)

t = Task("OOP o‘rganish")
print("Yangi task:", t)
```

---

## 7) Encapsulation: holatni tartib bilan boshqarish (raise’siz)

“Encapsulation” degani: obyekt ichidagi holat bexosdan buzilib ketmasin.
Biz bu yerda `raise` o‘rniga status qaytaramiz.

### Misol 1: BankAccount (True/False)

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.__balance = balance

    def deposit(self, amount):
        if amount <= 0:
            return False
        self.__balance += amount
        return True

    def withdraw(self, amount):
        if amount <= 0:
            return False
        if amount > self.__balance:
            return False
        self.__balance -= amount
        return True

    def get_balance(self):
        return self.__balance

acc = BankAccount("Alim", 100)
print(acc.withdraw(500))  # False
print(acc.withdraw(30))   # True
print(acc.get_balance())  # 70
```

### Misol 2: “Result” obyekt bilan (status + xabar)

```python
class Result:
    def __init__(self, ok, message):
        self.ok = ok
        self.message = message

class Wallet:
    def __init__(self, balance):
        self._balance = balance

    def pay(self, amount):
        if amount <= 0:
            return Result(False, "amount 0 dan katta bo‘lsin")
        if amount > self._balance:
            return Result(False, "pul yetarli emas")
        self._balance -= amount
        return Result(True, "to‘lov muvaffaqiyatli")

w = Wallet(50)
res = w.pay(100)
print(res.ok, res.message)
```

**Professional fikr:** Katta loyihada `True/False`dan ko‘ra `Result(ok, code, message)` kabi struktura qulayroq bo‘lib qoladi. “Nega false?” degan savolga javob bo‘ladi.

---

## 8) Property: tashqarida atributdek, ichida nazorat

Biz `raise` ishlatmaslik uchun setter o‘rniga `set_*` uslubini ishlatamiz.

### Misol 1: `@property` bilan o‘qish

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age

    @property
    def age(self):
        return self._age

p = Person("Sara", 20)
print(p.age)
```

### Misol 2: Nazoratli update

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self._age = age

    @property
    def age(self):
        return self._age

    def set_age(self, value):
        if value < 0:
            return False
        self._age = value
        return True

p = Person("Sara", 20)
print(p.set_age(-1))  # False
print(p.set_age(21))  # True
print(p.age)          # 21
```

---

## 9) Inheritance: meros olish (lekin ehtiyotkor)

Inheritance yaxshi, lekin ortiqcha ishlatish kodni “qotirib” qo‘yadi.

### Misol 1: Override (methodni almashtirish)

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return "..."

class Dog(Animal):
    def speak(self):
        return "vov vov"

print(Dog("Bobik").speak())
```

### Misol 2: Ota class methodidan foydalanish

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def info(self):
        return "Men %s" % self.name

class Cat(Animal):
    pass

c = Cat("Mimi")
print(c.info())
```

---

## 10) `super()` bilan ota class’ni chaqirish

### Misol 1: Developer

```python
class Employee:
    def __init__(self, name):
        self.name = name

class Developer(Employee):
    def __init__(self, name, stack):
        super().__init__(name)
        self.stack = stack

dev = Developer("Alim", ["Python", "FastAPI"])
print(dev.name, dev.stack)
```

### Misol 2: Vehicle -> Car

```python
class Vehicle:
    def __init__(self, brand):
        self.brand = brand

class Car(Vehicle):
    def __init__(self, brand, doors):
        super().__init__(brand)
        self.doors = doors

c = Car("BMW", 4)
print(c.brand, c.doors)
```

---

## 11) Polymorphism: bir xil “interfeys”, turli xulq

Polymorphism’ni eng oson tushuntirish: “bir xil method nomi, turli obyektlar”.

### Misol 1: Payment’lar

```python
class PayPal:
    def pay(self, amount):
        return "PayPal orqali %s to‘landi" % amount

class Click:
    def pay(self, amount):
        return "Click orqali %s to‘landi" % amount

def checkout(payment, amount):
    print(payment.pay(amount))

checkout(PayPal(), 1000)
checkout(Click(), 1000)
```

### Misol 2: Renderer’lar

```python
class HtmlRenderer:
    def render(self, text):
        return "<p>%s</p>" % text

class MarkdownRenderer:
    def render(self, text):
        return "**%s**" % text

def show(renderer, text):
    print(renderer.render(text))

show(HtmlRenderer(), "salom")
show(MarkdownRenderer(), "salom")
```

**Pro tip:** Polymorphism’ni ko‘p ishlatsang, `if payment_type == ...` kabi shartlar kamayadi.

---

## 12) Abstraction: “shartnoma” bilan ishlash (ABC)

Bu professional arxitekturada eng ko‘p foyda beradigan joylardan biri.
Yuqori darajadagi kod konkret implementatsiyaga emas, shartnomaga tayanadi.

### Misol 1: Storage shartnomasi

```python
from abc import ABC, abstractmethod

class Storage(ABC):
    @abstractmethod
    def save(self, key, value):
        pass

    @abstractmethod
    def load(self, key):
        pass
```

### Misol 2: Implementatsiyalar + ishlatish

```python
class MemoryStorage(Storage):
    def __init__(self):
        self._data = {}

    def save(self, key, value):
        self._data[key] = value

    def load(self, key):
        return self._data.get(key)


class FileStorage(Storage):
    def __init__(self, path):
        self.path = path

    def save(self, key, value):
        f = open(self.path, "a", encoding="utf-8")
        f.write("%s=%s\n" % (key, value))
        f.close()

    def load(self, key):
        try:
            f = open(self.path, "r", encoding="utf-8")
        except FileNotFoundError:
            return None

        value = None
        for line in f:
            if "=" not in line:
                continue
            k, v = line.rstrip("\n").split("=", 1)
            if k == key:
                value = v
                break
        f.close()
        return value


def app_flow(storage):
    storage.save("token", "abc123")
    print(storage.load("token"))

app_flow(MemoryStorage())
# app_flow(FileStorage("data.txt"))
```

**Professional fikr:** shartnoma borligi sabab, keyin `RedisStorage` yoki `PostgresStorage` qo‘shsang ham `app_flow` o‘zgarmaydi.

---

## 13) Composition: professional loyihada ko‘pincha inheritance’dan afzal

Inheritance bilan classlar “bir-biriga yopishib” qoladi. Composition esa modullarni almashtirishni oson qiladi.

### Misol 1: Logger’ni ichiga berish

```python
class Logger:
    def log(self, msg):
        print(msg)

class Service:
    def __init__(self, logger):
        self.logger = logger

    def run(self):
        self.logger.log("service run")

Service(Logger()).run()
```

### Misol 2: Fake logger bilan testga oson

```python
class FakeLogger:
    def __init__(self):
        self.messages = []

    def log(self, msg):
        self.messages.append(msg)

fake = FakeLogger()

class Service:
    def __init__(self, logger):
        self.logger = logger

    def run(self):
        self.logger.log("run")

svc = Service(fake)
svc.run()
print(fake.messages)  # ['run']
```

---

## 14) `@dataclass`: data class’lar uchun qulay (annotationssiz ham ishlaydi)

Dataclass aslida type hint bilan chiroyliroq, lekin hintsiz ham foydali.
Bu yerda “ko‘p boilerplate”dan qutulish g‘oyasini ko‘rsataman.

### Misol 1: Dataclass’ga o‘xshash qo‘lda yozilgan data class

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):
        return "Point(x=%s, y=%s)" % (self.x, self.y)

print(Point(1, 2))
```

### Misol 2: `dataclass` ishlatish (hintsiz)

```python
from dataclasses import dataclass, field

@dataclass
class User:
    username: str
    tags: list = field(default_factory=list)

u1 = User("a")
u2 = User("b")
u1.tags.append("python")
print(u1.tags)  # ['python']
print(u2.tags)  # []
```

Eslatma: Bu misolda `dataclass` talab qiladigan `username: str` bor. Agar sen “umuman type yozilmasin” desang, dataclass’ni ishlatmasdan qo‘lda yozish eng toza yo‘l bo‘ladi. Pastda real loyiha misolida qo‘lda yozilgan variantni ham beraman.

---

## 15) Real mini arxitektura: Entity + Repository + Service (professional uslub)

Bu bo‘lim OOP’ni “real” ko‘rsatadi. Odatda backend loyihalarda shunaqa qatlamlarga ajralib ketadi:
- Entity (model): ma’lumot
- Repository: saqlash/qidirish
- Service: biznes qoidalar

Biz `raise` qilmaymiz, status qaytaramiz.

### 15.1 Entity (Task) - misol 1

```python
class Task:
    def __init__(self, task_id, title, done=False):
        self.id = task_id
        self.title = title
        self.done = done

    def __repr__(self):
        return "Task(id=%s, title=%r, done=%s)" % (self.id, self.title, self.done)
```

### 15.2 Repository - misol 2 (InMemory)

```python
class InMemoryTaskRepo:
    def __init__(self):
        self._tasks = []

    def add(self, task):
        self._tasks.append(task)

    def list_all(self):
        return list(self._tasks)

    def find_by_id(self, task_id):
        for t in self._tasks:
            if t.id == task_id:
                return t
        return None
```

### 15.3 Service - misol 3 (biznes qoidalar)

```python
class TaskService:
    def __init__(self, repo):
        self.repo = repo
        self._next_id = 1

    def create(self, title):
        title = (title or "").strip()
        if not title:
            return None

        task = Task(self._next_id, title)
        self._next_id += 1
        self.repo.add(task)
        return task

    def mark_done(self, task_id):
        task = self.repo.find_by_id(task_id)
        if task is None:
            return False
        task.done = True
        return True

    def summary(self):
        done = 0
        total = 0
        for t in self.repo.list_all():
            total += 1
            if t.done:
                done += 1
        return (done, total)
```

### 15.4 Ishlatish - misol 4

```python
repo = InMemoryTaskRepo()
svc = TaskService(repo)

print(svc.create("OOP o‘rganish"))
print(svc.create("Unit test yozish"))
print(svc.create("   "))  # None (bo‘sh title)

print(repo.list_all())

print("done 1:", svc.mark_done(1))    # True
print("done 999:", svc.mark_done(999))  # False

print("summary:", svc.summary())  # (done, total)
```

**Nega bu professional:**
- UI (CLI yoki API) service’ni chaqiradi
- service repo bilan ishlaydi
- repo saqlashni biladi
- entity ma’lumotni ushlab turadi
Bu “hamma narsa bir joyda” bo‘lib ketishdan qutqaradi.

---

## 16) Kichik “professional” qoidalar (amaliy)

### 1) Katta `if/elif`larni polymorphism bilan kamaytir
Yomon:

```python
def pay(method, amount):
    if method == "paypal":
        return "paypal pay"
    if method == "click":
        return "click pay"
    return "unknown"
```

Yaxshi:

```python
class PayPal:
    def pay(self, amount):
        return "paypal pay %s" % amount

class Click:
    def pay(self, amount):
        return "click pay %s" % amount

def checkout(gateway, amount):
    return gateway.pay(amount)

print(checkout(PayPal(), 10))
print(checkout(Click(), 10))
```

### 2) Katta inheritance o‘rniga composition
Yomon odat: “hamma narsani meros qilib ketish”.
Yaxshi odat: kerakli narsani ichiga berib ishlatish (`logger`, `storage`, `client`).

### 3) Test yozishni oson qiladigan dizayn qil
Repo yoki tashqi servislarni “ichiga beriladigan” qilib yozsang, fake/mock berish oson bo‘ladi.

---

## 17) Mini test (professional odat) - 2 usul

### Misol 1: `assert` bilan tez tekshirish

```python
repo = InMemoryTaskRepo()
svc = TaskService(repo)

t = svc.create("hello")
assert t is not None
assert t.id == 1

ok = svc.mark_done(1)
assert ok is True

ok = svc.mark_done(999)
assert ok is False

done, total = svc.summary()
assert done == 1
assert total == 1

print("testlar ok")
```

### Misol 2: “qo‘lda” konsolda ko‘rish

```python
repo = InMemoryTaskRepo()
svc = TaskService(repo)

svc.create("a")
svc.create("b")
svc.mark_done(2)

print(repo.list_all())
print(svc.summary())
```

---
