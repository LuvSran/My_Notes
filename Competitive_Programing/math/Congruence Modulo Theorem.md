同余式（Congruence Modulo Theorem）
---

### ​**1. 定义**

a≡b(mod m) 表示 m∣(a−b)，即存在整数 k，使得 a−b=km。

---

### ​**2. 基本性质**

#### （1）​**自反性**

a≡a(modm)。

#### （2）​**对称性**

如果 a≡b(mod m)，则 b≡a(mod m)。

#### （3）​**传递性**

如果 a≡b(mod m) 且 b≡c(mod m)，则 a≡c(mod m)。

---

### ​**3. 运算规则**

#### （1）​**加法**

如果 a≡b(mod m) 且 c≡d(mod m)，则：

a+c≡b+d(mod m)

#### （2）​**减法**

如果 a≡b(mod m) 且 c≡d(mod m)，则：

a−c≡b−d(mod m)

#### （3）​**乘法**

如果 a≡b(mod m) 且 c≡d(mod m)，则：

a⋅c≡b⋅d(mod m)

#### （4）​**幂运算**

如果 a≡b(mod m)，则对于任意正整数 k：

$a^k≡b^k(mod m)$

#### （5）​**除法（特殊情况）​**

如果 a⋅c≡b⋅c(mod m)，且 gcd(c,m)=1，则可以两边同时除以 c：

a≡b(mod m)

---

### ​**注意**

- 同余式要求模数 m>1。
- 除法规则仅在 gcd(c,m)=1 时成立。