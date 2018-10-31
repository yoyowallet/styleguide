# Python

## py.test

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
