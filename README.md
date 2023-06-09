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

<br>

### Formulating the problem 

For a mistyped word, w, we can have many candidate words c. Amongst the candidate words, we may or may not have the actual correct word.

The goal is to use the Bayes rule to evaluate each candidate word c, and calculate the probability of the candidate word given a mistyped word. In mathematical terms, we are trying to maximise this expression.

<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture.PNG" alt="Maximisation problem" style="width: 38%; height: auto;"/>

<br>

Notice that P(w), that is the probability of a spelling error occuring, is constant across all of the candidates c. This is as we are comparing possible candidates c for the same mispelling w. So we can simplify our optimisation problem to:

<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture2.PNG" alt="Maximisation problem" style="width: 45%; height: auto;"/>

<br>

## P(c): Probability of candidate word

For a given candidate word c, P(c) is essentially the probability of that  candidate word occuring. It makes intuitive sense that we have to take this into account for our calculation as if a candidate word is uncommon or doesn't exist (so P(c) ≈ 0), then it is lessly likely that the a user was trying to type that word.

To determine this value, we can take a look at words.txt and derive the following:  

_For a word c_:  
* If word exists in words.txt:  
  * P(c) = $\text{total occurances of c} \over \text{total occurances of all words}$
* Else:
  * P(c) = 0

<br>
  
## P(w|c): Probability of the mistyped word w, given a candidate c

It makes sense that a correct candidate c should not be too "far" from the mistyped word w. We will be using edit distance to measure the distance between words.

<br>

### Edit distance aka Levenshtein distance

For any word, it can be transformed into another word _only_ using the following operations:
1. Insertion: Add a character to the string.
2. Deletion: Remove a character from the string.
3. Substitution: Replace a character in the string with another character.
<br>

Each of these operations are given a cost of 1. So the edit distance between two words A and B would simply be the sum of costs of the operations required to turn A into B.

Eg.
A = Cat, B = Bat, C = Code, D = Ha
* dist(A, B) = 1
  * Substitute C with B

* dist(A, C) = 3
  * Substitute a with o
  * Substitute t with d
  * Insert e

* dist(A, D) = 2
  * Substitute C with H
  * Delete t

### Choosing P(w|c) based on edit distance
Tt makes more sense that for a mistyped word it is more likely to be have a smaller edit distance. So that implies that: </br>
<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture3.jpg" style="width: 30%; height: auto;"/>

So we need to select a distribution that allows us to draw probabilities for a given edit distance x such that: <br/>
<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture4.JPG" style="width: 21%; height: auto;"/>

To achieve this, I chose an exponential distribution. </br>
<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture5.JPG" style="width: 37%; height: auto;"/>

### Choosing λ for the exponential distribution
</br>
λ was chosen based on out of sample test accuracy such that it solves the following constrainted optimisation problem: </br>
<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture6.JPG" style="width: 37%; height: auto;"/>

where P is the percentage of correctly guessed words </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;f is a function representing the test program that takes in values </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;x is the edit distance </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;λ is tuning parameter </br>

Solving this problem using scipy.Optimization, I calculated the value λ = 9.08732 </br>

Notice that optimisation problem solves our original problem.

## Results
So for our simple bayesian spelling correction, the out-of-sample performance at correcting errors was 76% at a speed of 10 words per second.
</br>
<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture7.JPG" style="width: 37%; height: auto;"/>

Considering the relatively simple nature of this algorithm, this is good performance. Of course, this solution can be tweaked to give better performance.
