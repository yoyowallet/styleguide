# Python

## Naming conventions

The [Google Cloud Platform design guides](https://cloud.google.com/apis/design/naming_convention) is a good reference.

-   Use double underscores `__` for unused variables. Single
    underscore `_` is reserved for internationalisation functions.

```python
user, __ = User.objects.get_or_create(email='hello@yoyowallet.com')
```

## Field names

-   Field names **should** avoid prepositions (e.g. for, during, at),
    for example:
    -   `reason_for_error` should instead be `error_reason`
    -   `cpu_usage_at_time_of_failure` should instead
        be `failure_time_cpu_usage`
-   <span style="color: rgb(33,33,33);">Field
    names </span>**should**<span style="color: rgb(33,33,33);"> also
    avoid using postpositive adjectives (modifiers placed after the
    noun), for example:</span>
    -   `stamps_collected` should instead be `collected_stamps`
    -   `objects_imported` should instead be `imported_objects`

## Environment Variables and Django Settings

-   Booleans **should** end with `_ENABLED` (e.g. `SWAGGER_ENABLED`,
    `DJANGO_DEBUG_ENABLED`).
-   Django settings variable name **should** be the same as the
    environment variable name.
    -   If not possible, the environment variable name **must** follow
        the next rule (e.g. `DEBUG` for Django).
-   Variables **should** be grouped using the same prefix (e.g.
    `DJANGO_`).

### Example: Django settings

We
use <a href="https://github.com/joke2k/django-environ" class="external-link">https://github.com/joke2k/django-environ</a> in
this example.

```python
# Boolean
# Django settings named the same as that of the environment variable
SWAGGER_ENABLED = env.bool('SWAGGER_ENABLED')

# Some Django settings cannot be renamed
# Make sure an appropriate prefix is used
DEBUG = env.bool('DJANGO_DEBUG_ENABLED')
```

## Dates

-   `DateTimeField`s **should** end with `_at` (e.g. `created_at`,
    `updated_at`).
-   `DateField`s **should** end with `_date` (e.g. `campaign_date`,
    `birth_date`).
-   Date ranges **should** end with `_from` and `_to` (e.g.
    `available_from`, `available_to`).

## Django

### CharField

-   `CharField.max_length` **should** be set to `255` unless a length
    limit is required. 
    -   [Note that this will be restricted to `255` if `unique=True`](https://docs.djangoproject.com/en/2.1/ref/databases/#character-fields)

### CharField choices

-   The values stored in the database **should** be uppercase.
-   The ordering of `CharField.choices` **should** be deterministic.

```python
# Choices defined as a dictionary
CHOICES = {
    'YES': 'Yes',
    'NO': 'No',
    'MAYBE': 'Maybe',
}

# No
option = models.CharField(max_length=255, choices=CHOICES.items())

# Yes
option = models.CharField(max_length=255, choices=sorted(CHOICES.items())

# Better
# Control your ordering
CHOICES = (
    ('YES', 'Yes'),
    ('NO', 'No'),
    ('MAYBE', 'Maybe'),
)
option = models.CharField(max_length=255, choices=CHOICES)
```

## Testing

-   When using `@pytest.mark.parametrize`, use deterministic arguments.

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

-   Avoid running expensive operations in the global scope as it will
    increase the test collection time.

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
