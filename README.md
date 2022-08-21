import urllib.request 
from bs4 import BeautifulSoup
import random 

word_list = ["adjacent", "dimension", "obedient",  "accumulate", "drastic", "oblivious",  "adapt", "elaborate", "origin",  "adequate", "encourage", "peculiar",  "analyze", "equation", "persuade",  "anticipate", "evaluate", "prediction",  "appropriate", "exaggerate", "priority",  "artifact", "exhaust", "quote",  "benefit", "expression", "realistic",  "calculate", "extend", "recount",  "catastrophe", "extensive", "reinforce",  "chronological", "factor", "repetition",  "citizen", "ferocious", "retrieve",  "civilization", "frequent", "frequency",  "compose", "genuine", "solution",  "conclusion", "government", "strategy",  "congruent", "history", "substitute",  "consequence", "hypothesis", "suspense",  "construct", "insist", "tentative",  "continuous", "irrigate", "thesis",  "contrast", "lofty", "transfer",  "contribute", "manipulate", "unanimous",  "declare", "massive", "unique",  "democracy", "narrate", "variable",  "similar", "viewpoint", "violate"]

def get_definition(word):
    # this function looks up the definition of a word
    # based on https://codezup.com/web-scraping-word-meaning-dictionary-python-beautifulsoup/
    # input: word is a string
    # output: word_definition is a string
    try: 
        url = f"https://www.vocabulary.com/dictionary/{word}"
        htmlfile = urllib.request.urlopen(url)
        soup = BeautifulSoup(htmlfile, 'lxml')
        search_result = soup.find(class_="short")
        word_definition = search_result.get_text()
        return word_definition
    except:
        print(f"Cannot find the definition of {word}.")
        return

def play_round():
    word_options = random.sample(word_list,4)
    target_word = random.choice(word_options)
    target_definition = get_definition(target_word)
    target_definition = target_definition.replace(target_word, '-----')
    target_definition = target_definition.replace(target_word.title(), '-----')
    target_index = word_options.index(target_word)
    round_score = 125
    
    print('Definition: ' + target_definition)
    print()

    for i in range(len(word_options)):
        print(f"{i+1}. {word_options[i]}")

    print()
    print("Please select 1, 2, 3, or 4 to answer.")
    
    user_guess = -1
    while user_guess != (target_index + 1):
        user_guess = input()
        try:
            user_guess = int(user_guess)
        except:
            print("Please enter a number.")
            continue
        round_score -= 25
        if user_guess != (target_index + 1):
            print("That's incorrect. Please try again.")    
        
    
    print()
    print("That's correct! Great job.")
    return max(0, round_score) 

print()
print("Welcome to Lex's word puzzles!")
print()
print("Lex will provide a definition, and she would like for you to identify the word that is being described. You may guess up to 4 times, but you receive more points for less guesses. Good luck!")
print()

keep_playing = 'y'
score_list = []
while keep_playing == 'y':
    round_score = play_round()
    score_list.append(round_score)
    print()
    print("Would you like to keep playing? \nEnter y for yes or n for no.")
    keep_playing = input()
    print()

score = sum(score_list)/len(score_list)
print(f"Thanks for playing! You scored {score:.2f}%. Great job!")
