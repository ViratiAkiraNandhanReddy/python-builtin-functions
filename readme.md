![BANNER IMAGE](assets/images/banner.png)

## `abs(x)`

**Signature:** `abs(x)`<br>
**Returns:** absolute value of a number (int, float, complex magnitude)

**Examples**

```py
# Integers and floats
abs(-5)           # -> 5
abs(3.14)         # -> 3.14
abs(-0)           # -> 0

# Complex numbers (returns magnitude)
abs(3+4j)         # -> 5.0  (sqrt(3^2+4^2))
abs(-1j)          # -> 1.0

# Practical pattern: distance between two points
def distance(a, b):
    return abs(b - a)

distance(10, 25)  # -> 15
```

**Detailed Explanation:**
- For integers/floats: returns the non-negative numeric value.
- For complex numbers: returns the **magnitude** (always a float), computed as √(real² + imag²).
- Works with any numeric type that defines `__abs__()`.
- The result has the same type as input (except complex → float).

**Use-cases:**
- Distance/error calculations in algorithms
- Normalizing values (e.g., confidence scores)
- Computing magnitude of force/velocity vectors
- Handling signed differences

**Tips & Pitfalls:**
- `abs()` on complex returns float, not complex.
- For arrays/NumPy: use `numpy.abs()` for vectorized efficiency.
- Cannot use on incompatible types (e.g., strings); raises `TypeError`.
- `abs(None)` raises `TypeError`; check types first in dynamic code.

---

## `all(iterable)`

**Signature:** `all(iterable)`<br>
**Returns:** `True` if every element in `iterable` is truthy (or iterable empty → `True`)

**Examples**

```py
# Basic truthy checks
all([True, 1, "non-empty"])  # -> True
all([True, 0, "x"])          # -> False (0 is falsy)
all([])                      # -> True (empty → True)
all([1, 2, 3, 4])            # -> True

# With generator (efficient, short-circuits)
all(x > 0 for x in [1, 2, -1, 4])  # -> False (stops at -1)

# Practical pattern: validate all users are active
users = [{"name": "Alice", "active": True}, {"name": "Bob", "active": True}]
all(u["active"] for u in users)  # -> True

# Pattern: check all values within range
values = [10, 20, 15, 18]
all(5 <= v <= 25 for v in values)  # -> True
```

**Detailed Explanation:**
- Returns `True` if **all** elements evaluate to `True` in a boolean context.
- Empty iterables return `True` by convention (vacuous truth).
- **Short-circuits**: stops immediately upon encountering a falsy value; remaining elements are not evaluated.
- Works with any iterable (list, tuple, generator, set, dict keys, etc.).

**Use-cases:**
- Validating ALL conditions in a batch (e.g., all users have emails)
- Pre-flight checks before processing
- Assertions in test code
- Ensuring all items meet a filter criteria

**Tips & Pitfalls:**
- Generator expressions + `all()` = lazy & efficient ; avoids materializing full list.
- Empty list returns `True`; use explicit length checks if that's not desired: `len(items) > 0 and all(...)`
- `all()` does not short-circuit on truthy values - it continues until a falsy one is found or list exhausted.
- For readability, use `all()` over nested `if` statements.

---

## `any(iterable)`

**Signature:** `any(iterable)`<br>
**Returns:** `True` if at least one element in `iterable` is truthy (or `False` if all falsy/empty)

**Examples**

```py
# Basic checks
any([0, "", None, False])      # -> False (all falsy)
any([0, "", "ok", None])       # -> True ("ok" is truthy)
any([])                        # -> False (empty)
any([False, False, True])      # -> True

# With generator (short-circuits on first truthy)
any(x > 10 for x in [1, 2, 15, 20])  # -> True (stops at 15)

# Practical pattern: check if ANY error occurred
errors = [{"code": 200}, {"code": 404}, {"code": 200}]
any(e["code"] >= 400 for e in errors)  # -> True

# Pattern: fast membership check in performance-sensitive code
blacklist = ["admin", "root", "system"]
if any(username == b for b in blacklist):  # Better: use `in` for strings
    reject_login()
```

**Detailed Explanation:**
- Returns `True` if **at least one** element is truthy.
- Returns `False` for empty iterables (no truthy elements to find).
- **Short-circuits**: stops at the first truthy value; remaining elements unprocessed.
- Complements `all()`; they are logical opposites for most inputs.

**Use-cases:**
- Checking if ANY validation passed (e.g., at least one login method works)
- Fast failure detection (e.g., any errors in a batch)
- Optional condition flags
- Defensive checks before operations

**Tips & Pitfalls:**
- For string membership, use `in` operator directly: `if "admin" in blacklist:` (faster than `any()`)
- `any([])` returns `False`, not `True`; empty = no truthy elements.
- Short-circuits: once a truthy value is found, no further iteration.
- Opposite of `all()`; `not all(x)` ≈ `any(not x for x in ...)`

---

## `ascii(obj)`

**Signature:** `ascii(obj)`<br>
**Returns:** string similar to `repr(obj)` but escapes non-ASCII characters with `\x`, `\u`, or `\U`.

**Examples**

```py
# Unicode escape patterns
ascii("café")          # -> "'caf\\xe9'"  (é → \xe9)
ascii("hello world")   # -> "'hello world'"  (all ASCII → unchanged)
ascii("你好")         # -> "'\\u4f60\\u597d'"  (Chinese chars)
ascii("emoji 🎉")     # -> "'emoji \\U0001f389'"  (high Unicode)

# With objects
ascii([1, 2, "café"])  # -> "[1, 2, 'caf\\xe9']"

# Practical pattern: safe logging for all character sets
def safe_log(message):
    return ascii(message)  # Safe for ASCII-only logs

print(safe_log("User: José"))  # -> "'User: Jos\\xe9'"
```

**Detailed Explanation:**
- Similar to `repr()` but **escapes all non-ASCII characters**.
- ASCII printable characters (0x20–0x7E) remain unchanged.
- Non-ASCII encoded as `\xhh` (8-bit), `\uhhhh` (16-bit), or `\Uhhhhhhhh` (32-bit).
- Result is always a valid Python string literal (safe to print/log).

**Use-cases:**
- Logging systems that only support ASCII output
- Debugging code with international text without corruption
- Network protocols requiring ASCII-safe representations
- Safe display of untrusted input

**Tips & Pitfalls:**
- Output is **safe for ASCII terminals/logs** but less human-readable.
- `repr()` vs. `ascii()`: `repr()` keeps Unicode readable; `ascii()` escapes it.
- Useful when you don't control the output encoding (old systems, telnet, etc.).
- `ascii()` slower than `repr()` for ASCII-only strings (due to escape processing).
