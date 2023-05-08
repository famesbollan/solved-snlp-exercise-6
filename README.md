Download Link: https://assignmentchef.com/product/solved-snlp-exercise-6
<br>
Long-Range Dependencies 1) Correlation Function

Words in natural languages exhibit both short-range local dependencies as well as long-range global dependencies to various degrees. To analyse these dependencies, the temporal correlation function can be used. The correlation between two words <em>w</em><sub>1 </sub>and <em>w</em><sub>2 </sub>is defined as

(1)

where <em>P<sub>D</sub></em>(<em>w</em><sub>1</sub><em>,w</em><sub>2</sub>) is the probability of observing <em>w</em><sub>2 </sub>after <em>w</em><sub>1 </sub>separated by a distance <em>D</em>, estimated from a (continuous) text corpus, and <em>P</em>(<em>w</em><sub>1</sub>) and <em>P</em>(<em>w</em><sub>2</sub>) are the unigram probabilities of <em>w</em><sub>1 </sub>and <em>w</em><sub>2</sub>, respectively, estimated from the same text corpus. The term <em>P<sub>D</sub></em>(<em>w</em><sub>1</sub><em>,w</em><sub>2</sub>) can be defined as

(2)

where <em>N<sub>D</sub></em>(<em>w</em><sub>1</sub><em>,w</em><sub>2</sub>) is the total number of times <em>w</em><sub>2 </sub>was observed after <em>w</em><sub>1 </sub>at distance <em>D </em>and <em>N </em>is the total number of word tokens in the text corpus.

<ul>

 <li>Pre-process the corpus English_train.txt by applying white-space tokenization and removing punctuation. You should ignore sentence boundaries. You can use the tokenize function provided in the script.</li>

 <li>Implement a function correlation that takes two words, <em>w</em><sub>1 </sub>and <em>w</em><sub>2</sub>, and distance <em>D </em>as inputs and returns as an output the correlation value.</li>

 <li>Use the correlation function and compute it for the word pairs (“he”,”his”),</li>

</ul>

(“he”,”her”), (“she”,”her”), (“she”,”his”) with different distances of <em>D </em>∈ {1<em>,</em>2<em>,..,</em>50}. Plot the correlation vs. distance with the values obtained. Explain your findings.

Your solution should contain the plot for (c), a sufficient explanation of the findings in this exercise, as well as the source code to reproduce the results.

<h1>Language Models</h1>

1) Linear Interpolation

In this exercise, you will explore simple linear interpolation in language models as an extension of the Lidstone smoothing you already implemented in assignment 3.

In the case of a higher-order ngram language model, there is always the problem of data sparseness. One way to solve this problem is by mixing the higher-order ngram estimates with lower-order probability estimates. This interaction is called interpolation. A simple way to do this is as follows:

<em>P<sub>I</sub></em>(<em>w</em><sub>2</sub>|<em>w</em><sub>1</sub>) = <em>λ</em><sub>1</sub><em>P</em>(<em>w</em><sub>2</sub>) + <em>λ</em><sub>2</sub><em>P</em>(<em>w</em><sub>2</sub>|<em>w</em><sub>1</sub>)                                 (3)

and

<em>P<sub>I</sub></em>(<em>w</em><sub>3</sub>|<em>w</em><sub>1</sub><em>,w</em><sub>2</sub>) = <em>λ</em><sub>1</sub><em>P</em>(<em>w</em><sub>3</sub>) + <em>λ</em><sub>2</sub><em>P</em>(<em>w</em><sub>3</sub>|<em>w</em><sub>2</sub>) + <em>λ</em><sub>3</sub><em>P</em>(<em>w</em><sub>3</sub>|<em>w</em><sub>1</sub><em>,w</em><sub>2</sub>) (4) where 0 ≤ <em>λ<sub>i </sub></em>≤ 1 and <sup>P</sup><em><sub>i </sub></em><em>λ<sub>i </sub></em>= 1

<ul>

 <li>First, implement a function to estimate interpolated bigram probabilities. Your implementation of this exercise can be an extension of your code in assignment 3 according to formula (3). Use the Lidstone smoothing technique that you implemented in Assignment 3 to obtain <em>P</em>(<em>w</em><sub>2</sub>) and <em>P</em>(<em>w</em><sub>2</sub>|<em>w</em><sub>1</sub>) where</li>

</ul>

(5)

and

(6)

<ul>

 <li>Create a bigram language model using interpolation with <em>λ</em><sub>1<em>,</em>2 </sub>= 1<em>/</em>2<em>,</em>1<em>/</em>2 and <em>α </em>= 0<em>.</em>3 on the English_train.txt corpus.</li>

 <li>Compute the perplexity of the model on the English_test.txt corpus and compare it with the perplexity of the original model with Lidstone smoothing (<em>α </em>= 0<em>.</em>3) and without interpolation. How do you explain the differences?</li>

</ul>

Your solution should contain the perplexity values for part (c), a sufficient explanation of the findings in this exercise, as well as the source code to reproduce the results.

2) Absolute Discounting Smoothing

In this exercise, you will implement absolute discounting smoothing for a bigram language model with a back-off strategy. In contrast to interpolation, we only “back off” to a lower-order n-gram if we have zero evidence for a higher-order n-gram. When the higher-order n-gram model is a bigram LM, absolute discounting is defined as

(7)

(8)

where 0 &lt; d &lt; 1 is the discounting parameter, <em>P<sub>unif </sub></em>is a uniform distribution over the vocabulary V, <em>λ</em>(<em>w<sub>i</sub></em><sub>−1</sub>) and <em>λ</em>(<em>.</em>) are the backing-off factors defined as

(9)

where

<em>N</em><sub>1+</sub>(<em>w<sub>i</sub></em><sub>−1</sub><em>•</em>) = |{<em>w </em>: <em>N</em>(<em>w<sub>i</sub></em><sub>−1</sub><em>w</em>) <em>&gt; </em>0}|                               (10)

and

(11)

where

<em>N</em><sub>1+ </sub>= |{<em>w </em>: <em>N</em>(<em>w</em>) <em>&gt; </em>0}|                                           (12)

<ul>

 <li>Given the equations above, implement absolute discounting smoothing and build a bigram LM using the English_train.txt corpus. Your implementation of this exercise can be an extension of the ngram_LM class or your could design your own code from scratch if that is more convenient for you. The main adaptation should be on the estimate_smoothed_prob function. For a unigram LM, consider the empty string as the n-gram history. You can use the test function to make sure that your probabilities sum up to 1.</li>

 <li>Compute and report the perplexity of the backing-off LM for the test corpus with <em>d </em>= 0<em>.</em>7.</li>

 <li>Compare the perplexity values of the model with the best-performing model you implemented with Lidstone smoothing. Explain the difference in performance by comparing the two methods.</li>

 <li>Investigate the effect of the discounting parameter d on the perplexity of the test corpus. For each <em>d </em>∈ {0<em>.</em>1<em>,</em>0<em>.</em>2<em>,…,</em>0<em>.</em>9} compute the perplexity and plot a line chart where the x-axis corresponds to the discounting parameter and the y-axis corresponds to the perplexity value.</li>

</ul>

Your solution should contain the plot for (d), a sufficient explanation of the findings in this exercise, as well as the source code to reproduce the results.

3) Kneser-Ney Smoothing

In this exercise, you will explore Kneser-Ney smoothing. Consider the following notation

<ul>

 <li>V – LM Vocabulary</li>

 <li>N(x) – Count of the n-gram x in the training corpus</li>

 <li><em>N</em><sub>1+</sub>(•<em>w</em>) where |{<em>u </em>: <em>N</em>(<em>u,w</em>) <em>&gt; </em>0}| – number of bigrams ending in w</li>

 <li><em>N</em><sub>1+</sub>(<em>w</em>) where |{<em>u </em>: <em>N</em>(<em>w,u</em>) <em>&gt; </em>0}| – number of bigrams starting with w</li>

 <li><em>N</em><sub>1+</sub>(•<em>w</em>) where |{(<em>u,v</em>) : <em>N</em>(<em>w,w,v</em>) <em>&gt; </em>0}| – number of trigram types with w in the middle</li>

</ul>

For a trigram Kneser-Ney LM, the probability of a word <em>w</em><sub>3 </sub>is estimated as

(13)

(14)

As shown above, the definition of the highest order probability in Kneser-Ney LM is identical to that in absolute discounting. However, the main difference is in estimating the lower-order back-off probabilities

(15)

(16)

The base case of this recursive definition is the unigram probability which is estimated as

(17)

where <em>N</em><sub>1+</sub>(••) is the number of bigram types. Contrary to absolute discounting, a Kneser-Ney LM does not interpolate with the zerogram probability. In case w3 was not in the vocabulary of the LM (i.e., OOV), then <em>P<sub>KN</sub></em>(<em>w</em><sub>3</sub>|<em>w</em><sub>1</sub><em>w</em><sub>2</sub>) = <em>P<sub>KN</sub></em>(<em>w</em><sub>3</sub>) = <sub>|<em>V</em></sub><u><sup>1</sup></u><sub>|</sub>

<ul>

 <li>From the English_train.txt corpus, collect the necessary statistics and fill the table below</li>

</ul>

<table width="345">

 <tbody>

  <tr>

   <td width="105"> </td>

   <td width="126">w = “longbourn”</td>

   <td width="114">w = “pleasure”</td>

  </tr>

  <tr>

   <td width="105">N(w)</td>

   <td width="126"> </td>

   <td width="114"> </td>

  </tr>

  <tr>

   <td width="105"> </td>

   <td width="126"></td>

   <td width="114"> </td>

  </tr>

 </tbody>

</table>

Discuss the differences.

<ul>

 <li>Describe the idea behind the Kneser Ney smoothing technique. How does it differ from other backing-off language models?</li>

</ul>

Your solution should include the table and the source code of part (a), and a sufficient explanation of part (b).