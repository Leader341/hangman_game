# Import random module for word selection
import random

# ASCII art stages of the Hangman
stages = [
"""
=========
  +---+
  |   |
      |
      |
      |
      |
=========
""",
"""
=========
  +---+
  |   |
  O   |
      |
      |
      |
=========
""",
"""
=========
  +---+
  |   |
  O   |
  |   |
      |
      |
=========
""",
"""
=========
  +---+
  |   |
  O   |
 /|   |
      |
      |
=========
""",
"""
=========
  +---+
  |   |
  O   |
 /|\\  |
      |
      |
=========
""",
"""
=========
  +---+
  |   |
  O   |
 /|\\  |
 /   |
      |
=========
""",
"""
=========
  +---+
  |   |
  O   |
 /|\\  |
 / \\  |
      |
=========
"""
]
# Each element represents a drawing step for wrong guesses

# Function to explain the rules of the game
def rule():
    # Valid "yes" and "no" inputs
    yes = ["y", "ye", "yes", "yah", "sure", "present", "yeah", "ok"]
    no = ["n", "no", "nah", "not", "nope", "absent"]

    # Ask the user if they want to read the rules
    laws = input("\nDo you want to know the rules of the game? ( Yes, No )").lower().strip()

    if laws in yes:
        print(""" 
        *** Hangman Game Rules ***\n
1) Try guessing the letters of the hidden word.
2) Each wrong guess uses up one of your 6 attempts.
3) If you lose all 6 attempts, the man gets hanged!
        """)
        input("Press [ Enter ] to start the game : ")

    elif laws in no:
        input("Press [ Enter ] to start the game : ")

    else:
        print("Please write yes or no.")
        rule()  # Ask again if input is invalid

# Call the rule function before starting the game
rule()

# Main Hangman game function
def hangman_game():
    print("""
*** Welcome to the Hangman Game ***	
""")

    # Lists of accepted responses
    yes = ["y", "ye", "yes", "yah", "sure", "present", "yeah", "ok"]
    no = ["n", "no", "nah", "not", "nope", "absent"]

    # Word bank and random word selection
    words = ["python", "chatgpt", "software", "photo", "photographer", "calculator", "bad", "good", "heater", "programming", "telephone"]
    random_word = random.choice(words)

    # Display underscores for the hidden word
    word_list = ["_" for _ in random_word]
    cut = "=" * 25  # Divider for visual clarity

    print(cut)
    print(f"(Hint): The first letter of the word is: {random_word[0]}\n")
    print(" ".join(word_list) + "\n")

    # Track previous guesses and remaining attempts
    ex_guess = []
    attempts = 6

    print(stages[0])  # Display the first stage (no body parts yet)

    # Main game loop
    while attempts > 0 and "_" in word_list:
        print(f"Previous letters: {', '.join(ex_guess)}")
        letter = input("Enter a letter: ").lower().strip()

        if len(letter) > 1:
            print("Please enter only one letter.")
            print(f"\n{cut}\n")
            print(" ".join(word_list))
            continue

        if letter in ex_guess:
            print("You already tried that letter. Try again.")
            print(f"\n{cut}\n")
            print(" ".join(word_list))
            continue

        if letter in random_word:
            for index in range(len(random_word)):
                if random_word[index] == letter:
                    word_list[index] = letter
            print(f"Correct! You still have [ {attempts} ] attempts.")
            print(f"\n{cut}\n")
            print(" ".join(word_list))
            ex_guess.append(letter)

        else:
            attempts -= 1
            print(f"Wrong letter! You have [ {attempts} ] attempts left.")
            print(stages[6 - attempts])
            print(f"\n{cut}\n")
            print(" ".join(word_list))
            ex_guess.append(letter)

        # Win condition
        if "_" not in word_list:
            print(f"""
         ***** YOU WIN *****\n
Congratulations!
You had {attempts} attempts left.
The word was: {random_word}
            """)

        # Lose condition
        if attempts <= 0:
            print(f"""
The word was: [ {random_word} ]			

               !!! YOU LOSE !!!
""")
            print(stages[-1])
            print(f"\n{cut}\n")

    # Ask user if they want to play again
    replay = input("Do you want to play again? ( Yes , No ) ").lower().strip()

    if replay in yes:
        hangman_game()
    elif replay in no:
        input("Press [ ENTER ] to exit . . .")
        print("Thank you for playing! See you next time.")
    else:
        print("Please answer Yes or No.")

# Start the game
hangman_game()
