# Python

## Syntax

- Use double underscores `__` for unused variables. Single underscore `_` is reserved for internationalisation functions.

```python
user, __ = User.objects.get_or_create(email='hello@yoyowallet.com')
```

## Testing

- When using `@pytest.mark.parametrize`, use deterministic arguments.

```python
# No
@pytest.mark.parametrize('number', set([1, 2, 3]))
def test_positive_number(number):
    assert number > 0

# Yes
@pytest.mark.parametrize('number', [1, 2, 3])
def test_positive_number(number):
    assert number > 0
```

- Avoid running expensive operations in the global scope as it will increase the test collection time.

```python
# No
value = expensive_operation()

def test_value():
    assert value

# Yes
def calculate_value():
    return expensive_operation()

def test_value():
    value = calculate_value()
    assert value

# Yes, using py.test fixture
@pytest.fixture
def value():
    return expensive_operation()

def test_value(value):
    assert value
```
