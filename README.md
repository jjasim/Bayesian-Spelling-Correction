# bayesian-spelling-correction

## Data
For this problem, we have the following files:
- words.txt
  - This file contains a large corpus of words to help train the model about the occurances of many words in the English language.
  - In particular, this file contains the full text of the _The Adventures of Sherlock Holmes_ by Sir Arthur Conan Doyle, encoded in ASCII

- testwords.txt
  - This file contains a list of words, along with possible mispellings of those words
  
## Understanding the problem

With the advent of small screens on smartphones, mistyping words is a common annoyance. So, what if we implemented a way to correct mispellings using the bayes rule? I will be doing exactly that here.

### Formulating the problem 

For a mistyped word, w, we can have many candidate words c. Amongst the candidate words, we may or may not have the actual correct word.

The goal is to use the Bayes rule to evaluate each candidate word c, and calculate the probability of the candidate word given a mistyped word. In mathematical terms, we are trying to maximise this expression.

<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture.PNG" alt="Maximisation problem">

<br>

Notice that P(w), that is the probability of a spelling error occuring, is constant across all of the candidates c. This is as we are comparing possible candidates c for the same mispelling w. So we can simplify our optimisation problem to:

<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture2.PNG" alt="Maximisation problem">

## P(c)

For a given candidate word c, P(c) is essentially the probability of that  candidate word occuring. It makes intuitive sense that we have to take this into account for our calculation as if a candidate word is uncommon or doesn't exist (so P(c) ≈ 0), then it is lessly likely that the a user was trying to type that word.

To determine this value, we can take a look at words.txt and derive the following:

_For a word c_:
  If word exists in words.txt:
$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a} $$
