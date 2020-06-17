# Arithmetic Operators

| Operator |      Name      | Example  |
| :------: | :------------: | :------: |
|    +     |    Addition    |  a + b   |
|    -     |  Subtraction   |  a - b   |
|    \*    | Multiplication |  a \* b  |
|    /     |    Division    |  a / b   |
|    %     |    Modulus     |  a % b   |
|   \*\*   | Exponentiation | a \*\* b |
|    //    | Floor Division |  a // b  |

#### Modulus

The modulo `%` operation finds the remainder or signed remainder after division of one number by another.

```python
# The statement can be read as ‘is the remainder of dividing year by 4 equal to 0?’.
def is_leap(year):
    leap = False

    if year % 400 == 0:
        return True
```

#### Exponentiation

The operation of raising one quantity to the power of another.

#### Floor Division

Floor division returns the quotient(answer or result of division) in which the digits after the decimal point are removed.
