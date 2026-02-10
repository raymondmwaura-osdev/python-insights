# `pytest.mark.parametrize`

## 1. Introduction

In software testing, it is often necessary to run the same test function with different input values. Writing separate test functions for each input leads to repetition and poor maintainability.

The `pytest.mark.parametrize` decorator in pytest solves this problem by allowing a single test function to be executed multiple times with different parameters.

---

## 2. Basic Concept

The `parametrize` decorator defines:

1. The names of parameters used in the test function.
2. A list of values (or value combinations) assigned to those parameters.

### Example

```python
import pytest

@pytest.mark.parametrize("x", [1, 2, 3])
def test_positive(x):
    assert x > 0
```

In this example, the test function `test_positive` runs three times, once for each value of `x`.

---

## 3. Multiple Parameters

When a test requires more than one parameter, multiple names and corresponding value tuples are provided.

```python
import pytest

@pytest.mark.parametrize("a,b,expected", [
    (1, 2, 3),
    (2, 3, 5),
])
def test_add(a, b, expected):
    assert a + b == expected
```

Each tuple represents one test case.

---

## 4. Structured Test Data

Parameters can be complex data structures such as dictionaries or objects.

```python
import pytest

@pytest.mark.parametrize("credential", [
    {"service": "service1", "username": "user1"},
    {"service": "service2", "username": "user2"},
])
def test_credential(credential):
    assert "service" in credential
```

This approach is useful when testing functions that operate on structured input.

---

## 5. Test Case Identification

To improve readability of test results, identifiers can be assigned to test cases.

```python
import pytest

@pytest.mark.parametrize(
    "x",
    [1, 2, 3],
    ids=["case_one", "case_two", "case_three"]
)
def test_numbers(x):
    assert x > 0
```

Each test run will appear with a descriptive name in the output.

---

## 6. Combination of Parameters

Multiple `parametrize` decorators can be stacked to generate combinations of inputs.

```python
import pytest

@pytest.mark.parametrize("x", [1, 2])
@pytest.mark.parametrize("y", [10, 20])
def test_combinations(x, y):
    assert x < y
```

Pytest executes the test for all combinations of `x` and `y`.

---

## 7. Integration with Fixtures

`parametrize` can be used together with pytest fixtures.

```python
import pytest

@pytest.fixture
def base_value():
    return 10

@pytest.mark.parametrize("x", [1, 2, 3])
def test_with_fixture(base_value, x):
    assert base_value + x > 10
```

Fixtures provide shared setup, while parameters define variable inputs.

---

## 8. Common Pitfalls

### 8.1 Incorrect Parameter Structure

Incorrect:

```python
@pytest.mark.parametrize("a,b", [1, 2])
```

Correct:

```python
@pytest.mark.parametrize("a,b", [(1, 2)])
```

### 8.2 Mutating Parameter Objects

Mutable objects such as lists or dictionaries should not be modified directly within tests, because they may be reused across test runs.

---

## 9. Advantages of `parametrize`

Using `pytest.mark.parametrize` provides the following benefits:

* Reduces code duplication.
* Improves test clarity and maintainability.
* Produces detailed test reports.
* Encourages systematic and scalable test design.

---
