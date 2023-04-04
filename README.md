# bayesian-spelling-correction

## Understanding the problem

With the advent of small screens on smartphones, mistyping words is a common annoyance. So, what if we implemented a way to correct mispellings using the bayes rule? I will be doing exactly that here.

### Formulating the problem 

For a mistyped word, w, we can have many candidate words c. Amongst the candidate words, we may or may not have the actual correct word.

The goal is to use the Bayes rule to evaluate each candidate word c, and calculate the probability of the candidate word given a mistyped word. In mathematical terms, we are trying to maximise this expression.

<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture.PNG" alt="Maximisation problem">

Notice that P(w), that is the probability of a spelling error occuring, is constant across all of the candidates c. This is as we are comparing possible candidates c for the same mispelling w. So we can simplify our optimisation problem to:

<img src="https://github.com/jjasim/bayesian-spelling-correction/blob/main/images/Capture2.PNG" alt="Maximisation problem">
