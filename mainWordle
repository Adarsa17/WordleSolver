#imports
import string
from collections import Counter
from itertools import chain
import operator

#Files for all 5 letter words and wordle words
file = open("/Users/adarsapedada/Downloads//fullWordle_List.txt", "r")
fileTwo = open("/Users/adarsapedada/Downloads//WordleAnswers_List.txt", "r")

#Lists to hold them
list1 = []
list2 = []

#loops to fill up both lists with words from files
for line in file:
    line_strip = line.strip()
    line_split = line_strip.split()
    list1.append(line_split)

for line in fileTwo:
    line_strip = line.strip()
    line_split = line_strip.split()
    list2.append(line_split)
file.close()
fileTwo.close()

WORDS = set ( )
ANSWER_WORDS = set ( )

#Add the lists into a set
for j in list1:
    s = ""
    for k in j:
        s = s + k
    WORDS.add(s)

for j in list2:
    s = ""
    for k in j:
        s = s + k
    ANSWER_WORDS.add(s)

#Setting parameters for wordle challenge
ALLOWABLE_CHARACTERS = set(string.ascii_letters)
ALLOWED_ATTEMPTS = 6
WORD_LENGTH = 5

#Counts number of of each letter in the answers
LETTER_COUNTER = Counter(chain.from_iterable(ANSWER_WORDS))

#determines the frequency of each letter
LETTER_FREQ = {
    character: value / LETTER_COUNTER.total()
    for character, value in LETTER_COUNTER.items()
}

#For each word, determines how "commmon" it is
def calculate_word_commonality(word):
    score = 0.0
    for char in word:
        score += LETTER_FREQ[char]
    return score / (WORD_LENGTH - len(set(word))+1)

#Sorts each possible answer by commonality
def sort_by_word_commonality(words):
    sort_by = operator.itemgetter(1)
    return sorted(
        [(word, calculate_word_commonality(word)) for word in words],
        key=sort_by,
        reverse=True,
    )

#Displays the best words
def display_word_table(word_commonalities):
    for (word, freq) in word_commonalities:
        print(f"{word:<10} | {freq:<5.2}")


def input_word():
    while True:
        word = input("Input the word you entered>")
        if len(word) == WORD_LENGTH and word.lower() in WORDS:
            break
    return word.lower()


def input_response():
    print("Type the color-coded reply from Wordle:")
    print("  G for Green")
    print("  Y for Yellow")
    print("  ? for Gray")

    while True:
        response = input("Response from Wordle> ")
        if len(response) == WORD_LENGTH and set(response) <= {"G", "Y", "?"}:
            break
        else:
            print(f"Error -> invalid answer{response}")
    return response



def match_word_vector(word, word_vector):
    assert len(word) == len(word_vector)
    for letter, v_letter in zip(word, word_vector):
        if letter not in v_letter:
            return False
    return True

def match(word_vector, possible_words):
    return[word for word in possible_words if match_word_vector(word, word_vector)]


#Enter in the word you typed and the response world gave, gives best suggest words
#Gives error if the response given does not lead to any word
def solve():
    possible_words = ANSWER_WORDS.copy()
    word_vector = [set(string.ascii_lowercase) for _ in range(WORD_LENGTH)]
    for attempt in range(1+ALLOWED_ATTEMPTS+1):
        print(f"Attempt {attempt} with {len(possible_words)} possible words")
        display_word_table(sort_by_word_commonality(possible_words)[:15])
        word = input_word()
        response = input_response()
        for index, letter in enumerate(response):
            if letter == "G":
                word_vector[index] = {word[index]}
            elif letter == "Y":
                try:
                    word_vector[index].remove(word[index])
                except KeyError:
                    pass
            elif letter == "?":
                for vector in word_vector:
                    try:
                        vector.remove(word[index])
                    except KeyError:
                        pass
        possible_words = match(word_vector, possible_words)

solve()
