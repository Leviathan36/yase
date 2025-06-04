# Type Checking

## Dynamic Type Checking

Python is a dynamically typed language, meaning that type checking of variables occurs at runtime during code interpretation. However, what often happens is that the Python interpreter performs implicit conversions at runtime, or, simply because we don't necessarily have to specify the types of the objects we use, we might pass an object of the wrong type to a function. These situations, obviously, lead the code to generate errors.

In the best-case scenario, the program crashes and throws an exception, thus avoiding the production of incorrect and unexpected results. In the worst-case scenario, however, the latter situation occurs: the code terminates correctly (returns 0), but the results produced are completely inaccurate, making it difficult to notice and debug the code.

These type-related errors happen because Python doesn't require specifying the type of function parameters. This makes it easier for a programmer to mistakenly call a function with parameters of the wrong type. Or, even if we do specify the parameter types, Python doesn't have a default type enforcement mechanism and doesn't check if the function is actually called with the correct parameters. Type hints are merely an indication for the programmer, helping with documentation.

However, thanks to external packages, we can implement dynamic type checking by leveraging the type hints in the function signature.

To do this, we can use `beartype` ([https://github.com/beartype/beartype](https://github.com/beartype/beartype)).

### Installation

```python
pip install beartype
```

### Basic Usage

```python
from beartype import beartype # decorator to add to functions and classes
from beartype.typing import * # imports all beartype types, overwriting classes from the standard typing package

@beartype
def foo(a: int, b: str) -> bool:
	a += 1
	print(b + str(a))
	return True
```

This function, in its simplicity, could cause the code to crash if called with two string parameters or two integer parameters. The "beartype" decorator, in this case, doesn't help prevent this type of error directly but rather provides a clearer error message and stops the function's execution right at the start—as soon as it's invoked with parameters of the wrong type. This prevents any strange and potentially dangerous behavior (think about functions that save to databases).

In some cases, however, the function might execute and terminate with code 0 even if the input parameters are incorrect. In this situation, using `beartype` is the only way to prevent the code from executing incorrectly and unpredictably:

```python
from beartype import beartype # decorator to add to functions and classes
from beartype.typing import * # imports all beartype types, overwriting classes from the standard typing package

@beartype
def foo(a: int, b: int) -> None:
	c = a + b
	print(c)
	# save "c" into db
```

The function above would terminate correctly whether it's passed two integers or two strings. The only way to prevent this is by using `beartype`.

This package does what could be done more simply with 30 lines of code in a "handmade" decorator; however, the package guarantees a signature analysis time with constant computational complexity, which is truly remarkable!

## "Static" Type Checking

The best approach, however, would be to also check types statically, before executing the code. Python doesn't provide any mechanism for this, but we can use third-party tools that allow us to achieve a similar result. For example, we can use a linter, such as Pylint. When installed within your IDE, it can check if the function signatures are consistent with their usage (this obviously requires inserting types in the function signatures).

It's also possible to use Pylint as a standalone command; you just need to install it with pip:

```python
pip install pylint
```

Another excellent linter is Flake8. You can also use this linter within your IDE (most popular code editors have ready-to-use extensions in their marketplace).

Theoretically, you could use both to validate your code, aiming to highlight as many errors as possible before sending it to production.

To install it, again, just run:

```python
pip install flake8
```
