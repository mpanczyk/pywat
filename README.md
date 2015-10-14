# Python wats

A "wat" is what I call a snippet of code that demonstrates a counterintuitive edge case of a programming language. The name comes from [this excellent talk by Gary Bernhardt](https://www.destroyallsoftware.com/talks/wat). If you're not familiar with the language, you might conclude that it's poorly designed when you see a wat. Often, more context about the language design will make the wat seem reasonable, or at least justified.

Wats are funny, and learning about a language's edge cases probably helps you with that language, but I don't think you should judge a language based on its wats. (It's perfectly fine to judge a language, of course, as long as it's an *informed* judgment, based on whether the language makes it difficult for its developers to write error-free code, not based on artificial, funny one-liners.) This is the point I want to prove to Python developers.

If you're a Python developer reading these, you're likely to feel a desire to put them into context with more information about how the language works. You're likely to say "Actually that makes perfect sense if you consider how Python handles *X*", or "Yes that's a little weird but in practice it's not an issue." And I completely agree. Python is a well designed language, and these wats do not reflect badly on it. You shouldn't judge a language by its wats.

## The Python wat quiz

Are you unimpressed by these wats? Do you think these edge cases are actually completely intuitive? Try your hand at [the Python wat quiz](https://github.com/cosmologicon/pywat/blob/master/quiz.md)!

## The wats

### Inheritance of `bool` from `int`

```python
>>> isinstance(True, int)
True
>>> bool.mro()
[bool, int, object]
```

That is why these are possible:
* the [converse implication](https://en.wikipedia.org/wiki/Converse_implication) operator
  ```python
  >>> False ** False == True
  True
  >>> False ** True == False
  True
  >>> True ** False == True
  True
  >>> True ** True == True
  True
  ```

* the alternative for `expr1 if cond else expr2`:
  ```python
  >>> x = 10
  >>> ('x is not equal to 10', 'x is equal to 10')[x==10]
  'x is equal to 10'
  ```

### Operator precedence?

```python
>>> False == False in [False]
True
```

[Source](https://www.reddit.com/r/programming/comments/3cjjgp/why_does_return_the_string_10/csxak65).

It is interpreted as `False == False and False in [False]`.
The same as `1 < 2 < 3` is interpreted as `1 < 2 and 2 < 3`.

### Circular types

```python
>>> isinstance(object, type)
True
>>> isinstance(type, object)
True
```

[Source](https://www.reddit.com/r/Python/comments/3c344g/so_apparently_type_is_of_type_type/csrwwyv).

### Comparing `NaN`s

```python
>>> x = 0*1e400  # nan
>>> {x, x, float(x)}
{nan}
>>> {0*1e400, 0*1e400}
{nan, nan}
>>> {0*1e400, 0*1e400, x, x}
{nan, nan, nan}
```

