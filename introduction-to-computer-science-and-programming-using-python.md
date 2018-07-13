# Introduction to Computer Science and Programming Using Python (MIT's 6.00.1x)

## Course Syllabus

### Exercise Problems

#### 1) Longest substring

Given a string, find the longest substring whose characters are in alphabetical order

##### Solution

```python
def longest_alphabetical_substring(s):
    main_counter, main_longest = 0, ''
    while main_counter < len(s)-1:
        sub_counter, sub_longest = main_counter, s[main_counter]
        while sub_counter < len(s)-1:
            if s[sub_counter] > s[sub_counter+1]:
                main_counter = sub_counter
                break
            else:
                sub_longest += s[sub_counter+1]
                sub_counter += 1
        if len(sub_longest) > len(main_longest):
            main_longest = sub_longest
        main_counter += 1
    print('Longest substring in aplhabetical order is: ', main_longest)
    return
```
#### 2) Write a program that plays the hangman game (words file found in repository)

##### Solution

```python
import random, os

WORDLIST_FILENAME = "/Users/Aditya/Documents/SelfEducation/EdX/words.txt"

def loadWords():
    """
    Returns a list of valid words. Words are strings of lowercase letters.

    Depending on the size of the word list, this function may
    take a while to finish.
    """
    print("Loading word list from file...")
    # inFile: file
    inFile = open(WORDLIST_FILENAME, 'r')
    # line: string
    line = inFile.readline()
    # wordlist: list of strings
    wordlist = line.split()
    print("  ", len(wordlist), "words loaded.")
    return wordlist

def chooseWord(wordlist):
    """
    wordlist (list): list of words (strings)

    Returns a word from wordlist at random
    """
    return random.choice(wordlist)

# end of helper code
# -----------------------------------

# Load the list of words into the variable wordlist
# so that it can be accessed from anywhere in the program
wordlist = loadWords()

def isWordGuessed(secretWord, lettersGuessed):
    '''
    secretWord: string, the word the user is guessing
    lettersGuessed: list, what letters have been guessed so far
    returns: boolean, True if all the letters of secretWord are in lettersGuessed;
      False otherwise
    '''
    letters_secretWord = [letter for letter in secretWord]
    for letter in letters_secretWord:
        if letter not in lettersGuessed:
            return False
    return True


def getGuessedWord(secretWord, lettersGuessed):
    '''
    secretWord: string, the word the user is guessing
    lettersGuessed: list, what letters have been guessed so far
    returns: string, comprised of letters and underscores that represents
      what letters in secretWord have been guessed so far.
    '''
    partial = ['_ ' for i in range(len(secretWord))]
    letters_secretWord = [letter for letter in secretWord]
    for i,letter in enumerate(letters_secretWord):
        if letter in lettersGuessed:
            partial[i] = letter
    return ''.join(partial)



def getAvailableLetters(lettersGuessed):
    '''
    lettersGuessed: list, what letters have been guessed so far
    returns: string, comprised of letters that represents what letters have not
      yet been guessed.
    '''
    import string
    all_letters = [l for l in string.ascii_lowercase]
    remaining_letters = [l for l in all_letters if l not in lettersGuessed]
    return ''.join(remaining_letters)


def hangman(secretWord):
    '''
    secretWord: string, the secret word to guess.

    Starts up an interactive game of Hangman.

    * At the start of the game, let the user know how many
      letters the secretWord contains.

    * Ask the user to supply one guess (i.e. letter) per round.

    * The user should receive feedback immediately after each guess
      about whether their guess appears in the computers word.

    * After each round, you should also display to the user the
      partially guessed word so far, as well as letters that the
      user has not yet guessed.

    '''
    print('Welcome to the game, Hangman!')
    print('I am thinking of a word that is ' + str(len(secretWord))+ ' letters long.')
    lettersGuessed = []
    counter = 8
    secretWord_letters = [letter.lower() for letter in secretWord]
    while counter > 0:
        print('--------------')
        print('You have', counter, 'guesses left!')
        print('Available letters:', getAvailableLetters(lettersGuessed))
        l = input('Please guess a letter: ').lower()
        if l in lettersGuessed:
            print("Oops! You've already guessed that letter:", getGuessedWord(secretWord, lettersGuessed))
            continue
        else:
            lettersGuessed.append(l)
            counter -= 1
        if l not in secretWord_letters:
            print('Oops! That letter is not in my word:', getGuessedWord(secretWord, lettersGuessed))
        else:
            counter += 1
            print('Good guess:', getGuessedWord(secretWord, lettersGuessed))
        if isWordGuessed(secretWord, lettersGuessed):
            print('--------------')
            print('Congratulations, you won!')
            return None
    print('--------------')
    print('Sorry, you ran out of guesses.', 'The word was', secretWord + '.')
    return None
```
