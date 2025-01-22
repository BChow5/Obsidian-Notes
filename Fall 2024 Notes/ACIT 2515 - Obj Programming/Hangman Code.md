### Hangman with game class
```python
"""
ACIT2515 Lab
Tim G @ ACIT2515
"""
import random
import sys

class SecretWord:
    def __init__(self, word=None):
        if word is not None:
            # Make sure our word is always uppercase
            self.word = word.upper()
            return
            
        with open("words.txt") as fp:
            lines = fp.readlines()
            word = random.choice(lines)
            # Make sure our word is always uppercase
            self.word = word.strip().upper()

    def show_letters(self, letters):
        """
        This function RETURNS A STRING.
        This function scans the word letter by letter.
        First, make sure word is uppercase, and all letters are uppercase.
        If the letter of the word is in the list of letters, keep it.
        Otherwise, replace it with an underscore (_).

        DO NOT USE PRINT!

        Example:
        >>> reveal_letters("VANCOUVER", ["A", "V"])
        'V A _ _ _ _ V _ _'
        >>> reveal_letters("TIM", ["G", "V"])
        '_ _ _'
        >>> reveal_letters("PIZZA", ["A", "I", "P", "Z"])
        'P I Z Z A'
        """
        uppercase = [let.upper() for let in letters]
        filtered = [letter if letter in uppercase else "_" for letter in self.word]
        return " ".join(filtered)


    def check_letters(self, letters):
        """Returns True if all letters in word are in the list 'letters'"""
        return "_" not in self.show_letters(letters)
    
    def check(self, text):
        return self.word == text.strip().upper()

class Game:
    def __init__(self, turns=10, word=None):
        self.turns = turns
        self.word = SecretWord(word)
        self.letters = []

    def play(self):
        for turn_number in range(self.turns):
            try:
                result = self.play_one_turn()
            except KeyboardInterrupt:
                print("\n!!! You stopped the game.")
                return False
            
            if result:
                print("You found the word! It was", self.word.word)
                return True

        print("You lost!")
        return False
            
            
    def play_one_turn(self):
        # Ask the player for a letter
        letter = ""
        keep_going = True
        while keep_going:
            letter = input("Letter? ")
            # Make it uppercase
            letter = letter.upper().strip()

            if letter == "":
                print("You have to provide a letter.")
            elif len(letter) > 1:
                if self.word.check(letter):
                    keep_going = False
                else:
                    print("This is not the word.")
            elif letter in self.letters:
                print("You already tried this letter.")
            else:
                self.letters.append(letter.upper())
                keep_going = False
                # Show the hint
                print(self.word.show_letters(self.letters))
    

        # Check if we win
        if self.word.check_letters(self.letters) or self.word.check(letter):
            return True

        return False

if __name__ == "__main__":
    if len(sys.argv) > 1:
        word = sys.argv[1].strip()
        turns = int(input("How many turns?"))
        game = Game(turns, word)
    else:
        game = Game(10, "PYTHON")

    game.play()

```

### Testing with fixtures and mocks
```python
import pytest
from unittest.mock import patch, mock_open
from hangman import Game


@pytest.fixture
@patch("builtins.open", new_callable=mock_open, read_data="aaaaa")
def game_word_is_a(mock_file):
    """Game object, 5 turns, word is aaaaa"""

    return Game(5)


@pytest.fixture
@patch("builtins.open", new_callable=mock_open, read_data="testword")
def game_word_is_testword(mock_file):
    """Game object, 10 turns, word is testword"""

    return Game(10)


def test_turns(game_word_is_a):
    # Default value is 10
    with patch("builtins.open", new_callable=mock_open, read_data="whatever"):
        assert Game().turns == 10

    # This particular game has 5 (see fixture)
    assert game_word_is_a.turns == 5


def test_play_one_round_a(game_word_is_a):
    with patch("builtins.input", return_value="a") as mock_input:
        assert game_word_is_a.play_one_round() is True
        assert game_word_is_a.turns == 4


def test_play_one_round_a_empty_input(game_word_is_a):
    with patch(
        "builtins.input", side_effect=["", "", "", "", "", "", "a", "b"]
    ) as mock_input:
        assert game_word_is_a.play_one_round() is True
        # play_one_round returns as soon as ONE valid guess is provided
        assert game_word_is_a.turns == 4


def test_play_one_round_aa(game_word_is_a):
    with patch("builtins.input", return_value="aa"):
        # Wrong guess - the word is not 'aa'
        assert game_word_is_a.play_one_round() is False
        # It still took one turn
        assert game_word_is_a.turns == 4


def test_play_one_round_check(game_word_is_testword):
    # Check lower/uppercase
    with patch("builtins.input", return_value="TESTWORD"):
        assert game_word_is_testword.play_one_round() is True


def test_play_game_a(game_word_is_a):
    with patch(
        "builtins.input", side_effect=["", "", "", "", "", "", "b", "a"]
    ) as mock_input:
        assert game_word_is_a.play() is True
        # Took 2 turns to find the word ('b', then 'a')
        assert game_word_is_a.turns == 3


def test_play_game_testword_win(game_word_is_testword):
    with patch(
        "builtins.input",
        side_effect=["t", "T", "E", "s", "w", "o", "r", "a", "b", "c", "d"],
    ) as mock_input:
        assert game_word_is_testword.play() is True
        # t = T, case does not matter = 10 turns required
        assert game_word_is_testword.turns == 0


def test_play_game_testword_win_goated(game_word_is_testword):
    with patch("builtins.input", side_effect=["dunno", "testWORd"]) as mock_input:
        assert game_word_is_testword.play() is True
        # Takes two turns to guess the full word
        assert game_word_is_testword.turns == 8


def test_play_game_testword_lose(game_word_is_testword):
    with patch(
        "builtins.input", side_effect=["a", "b", "c", "d", "e", "f", "g", "t", "s", "w"]
    ) as mock_input:
        assert game_word_is_testword.play() is False
        assert game_word_is_testword.turns == 0

```

```python
from unittest.mock import patch, mock_open
from hangman import SecretWord


def test_secret_word_show_letters():
    word = SecretWord("vancouver")
    assert word.show_letters(["V"]) == "V _ _ _ _ _ V _ _"
    assert word.show_letters(["V", "A"]) == "V A _ _ _ _ V _ _"


def test_secret_word_check_letters():
    word = SecretWord("pizza")
    assert word.check_letters(["P", "I", "Z", "A"]) is True
    assert word.check_letters(["A", "Z", "P", "I"]) is True

    word = SecretWord("Tim")
    assert word.check_letters(["G"]) is False


def test_secret_word_check():
    word = SecretWord("vancouver")
    assert word.check("VanCOuver") is True
    assert word.check("VANCOUVER") is True
    assert word.check("hello") is False


@patch("builtins.open", new_callable=mock_open, read_data="vancouver")
def test_secret_word_no_args(mock_file):
    word = SecretWord()
    assert mock_file.call_count == 1
    assert word.check("Vancouver") is True

```

### Hangman with no game class
```python
"""
ACIT2515 Lab
Tim G @ ACIT2515
"""
import random

# The number of turns allowed is a global constant
NB_TURNS = 10

class SecretWord:
    def __init__(self, word=None):
        if word is not None:
            # Make sure our word is always uppercase
            self.word = word.upper()
            return
            
        with open("words.txt") as fp:
            lines = fp.readlines()
            word = random.choice(lines)
            # Make sure our word is always uppercase
            self.word = word.strip().upper()

    def show_letters(self, letters):
        """
        This function RETURNS A STRING.
        This function scans the word letter by letter.
        First, make sure word is uppercase, and all letters are uppercase.
        If the letter of the word is in the list of letters, keep it.
        Otherwise, replace it with an underscore (_).

        DO NOT USE PRINT!

        Example:
        >>> reveal_letters("VANCOUVER", ["A", "V"])
        'V A _ _ _ _ V _ _'
        >>> reveal_letters("TIM", ["G", "V"])
        '_ _ _'
        >>> reveal_letters("PIZZA", ["A", "I", "P", "Z"])
        'P I Z Z A'
        """
        uppercase = [let.upper() for let in letters]
        filtered = [letter if letter in uppercase else "_" for letter in self.word]
        return " ".join(filtered)


    def check_letters(self, letters):
        """Returns True if all letters in word are in the list 'letters'"""
        return "_" not in self.show_letters(letters)
    
    def check(self, text):
        return self.word == text.strip().upper()


def main(turns):
    """
    Runs the game. Allows for "turns" loops (attempts).
    At each turn:
    1. Ask the user for a letter
    2. Add the letter to the list of letters already tried by the player
    3. If the letter was already tried, ask again
    4. Use the reveal_letters function to display hints about the word
    5. Remove 1 to the number of tries left
    6. Check if the player
        - won (= word has been found)
        - lost (= word has not been found, no tries left)

    Do not forget to pick a random word first :-)

    """
    # These are the letters tried by the player
    letters = []
    # Get a word
    word = SecretWord()
    # Cheat: this is the word we are looking for
    print(word.word)

    # We can assume that turns will be > 1 the first run
    print(word.show_letters(letters))

    for turn_number in range(turns):
        # Ask the player for a letter
        letter = ""
        while letter.upper() not in letters:
            letter = input("Letter? ")
            if letter.upper() not in letters:
                letters.append(letter.upper())
            else:
                print("You already tried this.")
        # Show the hint
        print(word.show_letters(letters))

        # Check if we win
        if word.check_letters(letters):
            print("You win!")
            return True

    print("You lost!")


if __name__ == "__main__":
    main(NB_TURNS)

```

### Hangman (basic)
```python
"""
ACIT2515 Lab
Tim G @ ACIT2515
"""
import random
import sys

def pick_random_word():
    """Opens the words.txt file, picks and returns a random word from the file"""
    with open("words.txt") as fp:
        lines = fp.readlines()
        word = random.choice(lines)
        return word.strip()


def reveal_letters(word, letters):
    """
    This function RETURNS A STRING.
    This function scans the word letter by letter.
    First, make sure word is uppercase, and all letters are uppercase.
    If the letter of the word is in the list of letters, keep it.
    Otherwise, replace it with an underscore (_).

    DO NOT USE PRINT!

    Example:
    >>> reveal_letters("VANCOUVER", ["A", "V"])
    'V A _ _ _ _ V _ _'
    >>> reveal_letters("TIM", ["G", "V"])
    '_ _ _'
    >>> reveal_letters("PIZZA", ["A", "I", "P", "Z"])
    'P I Z Z A'
    """
    uppercase = [let.upper() for let in letters]
    word = word.upper()
    filtered = [letter if letter in uppercase else "_" for letter in word]
    return " ".join(filtered)


def all_letters_found(word, letters):
    """Returns True if all letters in word are in the list 'letters'"""

    # Very easy - if there is a "_" left, we did not find all letters...
    return "_" not in reveal_letters(word, letters)

    # OPTION 2
    # Substracting the set of letters from the set of word should be the empty set
    return set(word.upper()) - set([let.upper() for let in letters]) == set()



def main(turns, word=None):
    """
    Runs the game. Allows for "turns" loops (attempts).
    At each turn:
    1. Ask the user for a letter
    2. Add the letter to the list of letters already tried by the player
    3. If the letter was already tried, ask again
    4. Use the reveal_letters function to display hints about the word
    5. Remove 1 to the number of tries left
    6. Check if the player
        - won (= word has been found)
        - lost (= word has not been found, no tries left)

    Do not forget to pick a random word first :-)

    """
    # These are the letters tried by the player
    letters = []
    
    # Get a word
    if word is None:
        word = pick_random_word()
    
    # Cheat: this is the word we are looking for
    print(word)

    # We can assume that turns will be > 1 the first run
    print(reveal_letters(word, letters))

    for turn_number in range(turns):
        # Ask the player for a letter
        letter = ""
        keep_going = True
        while keep_going:
            letter = input("Letter? ")
            # Make it uppercase
            letter = letter.upper().strip()

            if letter == "":
                print("You have to provide a letter.")
            elif len(letter) > 1:
                if letter.upper() == word.upper():
                    return True
                keep_going = False
                print("This is not the word.")
            elif letter in letters:
                print("You already tried this letter.")
            else:
                letters.append(letter.upper())
                keep_going = False
        
        # Show the hint
        print()
        print("Showing with letters:", " ".join(sorted(letters)))
        print(reveal_letters(word, letters))
        print()

        # Check if we win
        if all_letters_found(word, letters):
            print("You win!")
            return True

    print("You lost!")


if __name__ == "__main__":
    turns = 10
    word_to_find = None
    
    # We have a number of turns provided
    if len(sys.argv) >= 2:
        turns = int(sys.argv[1])
    
    # We have a word provided
    if len(sys.argv) >= 3:
        word_to_find = sys.argv[2].strip()

    try:
        main(turns, word=word_to_find)
    except KeyboardInterrupt:
        print()
        print("You don't like the game? Bye!")
```