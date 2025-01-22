### Card Dunder Methods Code

```python
import pytest


class Card:
    def __init__(self, value, color):
        # Type checking for value
        if isinstance(value, str) and value.isnumeric():
            value = int(value)
        elif not isinstance(value, int):
            raise AttributeError

        # try:
        #     value = int(value)
        # except ValueError as e:
        #     raise AttributeError(e)

        # Boundary checks for value
        if value <= 0 or value > 10:
            raise AttributeError

        # Value check for color
        if color not in ("red", "black"):
            raise AttributeError

        # If all checks succeeded, we can set the attributes
        self.value = value
        self.color = color

    def __lt__(self, other):
        if not isinstance(other, type(self)):
            return TypeError("Can only compare cards!")
        
        return self.value < other.value
    
    def __str__(self):
        return f"{self.value} {self.color}"


def test_card_ok():
    ten_red = Card(10, "red")
    assert ten_red.value == 10
    assert ten_red.color == "red"


def test_card_str():
    ten_red = Card(10, "red")
    assert str(ten_red) == "10 red"


def test_card_invalid_value():
    with pytest.raises(AttributeError):
        Card(0, "red")

    with pytest.raises(AttributeError):
        Card(15, "red")

    with pytest.raises(AttributeError):
        Card("abcdef", "red")


def test_card_value_string():
    ten_red = Card("10", "red")
    assert ten_red.value == 10
    assert ten_red.color == "red"


def test_card_invalid_color():
    with pytest.raises(AttributeError):
        Card(10, "blue")


def test_stronger_than():
    five_black = Card(5, "black")
    ten_red = Card(10, "red")
    one_red = Card(1, "red")

    assert ten_red > five_black
    assert one_red < ten_red

```