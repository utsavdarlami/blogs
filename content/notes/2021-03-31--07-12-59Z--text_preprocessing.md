+++
title = "text-preprocessing"
author = ["felladog"]
date = 2021-03-31T12:57:00+05:45
lastmod = 2021-12-11T14:29:40+05:45
tags = ["bag of words", "TF-IDF"]
categories = ["NLP", "data preprocessing"]
draft = false
+++

---

-   References :
    -   [Book] Builiding the ML systems with python
    -   <http://tfidf.com/>
-   Questions :

---

Ways to preprocess text before [Feature Extraction in NLP]({{<relref "20211211110611-feature_extraction_in_nlp.md#" >}})

When preprocessing, we can perform the following:

1.  Eliminate handles and URLs
2.  Tokenize the string into words.
3.  Remove stop words like "and, is, a, on, etc."
4.  [Stemming](#stemming) or convert every word to its stem.
    -   Like dancer, dancing, danced, becomes 'danc'. You can use porter stemmer to take care of this.
5.  Convert all your words to lower case.

For example

-   tweet

    ```nil
    @YMourri and @AndrewYNg are tuning a GREAT AI model at https://deeplearning.ai!!!
    ```
-   After preprocessing becomes

    ```nil
    [tun,great,ai,model]
    ```

    Hence we can see how we eliminated handles, tokenized it into words, removed stop words, performed stemming, and converted everything to lower case.


## Stemming {#stemming}

Stemming is the process of converting a word to its most general form, or stem. This helps in reducing the size of our vocabulary.

Consider the words:

-   **learn**
-   **learn** ing
-   **learn** ed
-   **learn** t

All these words are stemmed from its common root **learn**. However, in some cases, the stemming process produces words that are not correct spellings of the root word. For example, **happi** and **sunni**. That's because it chooses the most common stem for related words. For example, we can look at the set of words that comprises the different forms of happy:

-   **happ** y
-   **happi** ness
-   **happi** er

We can see that the prefix **happi** is more commonly used. We cannot choose **happ** because it is the stem of unrelated words like **happen**.

NLTK has different modules for stemming and we will be using the [PorterStemmer](<https://www.nltk.org/api/nltk.stem.html#module-nltk.stem.porter>) module which uses the [Porter Stemming Algorithm](<https://tartarus.org/martin/PorterStemmer/>). Let's see how we can use it in the cell below.


## Code {#code}

```python
import nltk                                # Python library for NLP
from nltk.corpus import twitter_samples    # sample Twitter dataset from NLTK
import matplotlib.pyplot as plt            # library for visualization
import random
```

-   Download stopwords from NLTK

<!--listend-->

```python
nltk.download('stopwords')
# download the stopwords from NLTK
```

-   Imports

<!--listend-->

```python
import re                                  # library for regular expression operations
import string                              # for string operations

from nltk.corpus import stopwords          # module for stop words that come with NLTK
from nltk.stem import PorterStemmer        # module for stemming
from nltk.tokenize import TweetTokenizer   # module for tokenizing strings
```


### Remove hyperlinks, Twitter marks and styles {#remove-hyperlinks-twitter-marks-and-styles}

```python
tweet2 = re.sub(r'^RT[\s]+', '', tweet)

# remove hyperlinks
tweet2 = re.sub(r'https?://[^\s\n\r]+', '', tweet2)

# remove hashtags
# only removing the hash # sign from the word
tweet2 = re.sub(r'#', '', tweet2)

print(tweet2) #
```


### Tokenize the string {#tokenize-the-string}

```python
# instantiate tokenizer class
tokenizer = TweetTokenizer(preserve_case=False, strip_handles=True,
			       reduce_len=True)

# tokenize tweets
tweet_tokens = tokenizer.tokenize(tweet2)

print()
print('Tokenized string:')
print(tweet_tokens)
```


### Stop words and punctuation {#stop-words-and-punctuation}

```python
#Import the english stop words list from NLTK
stopwords_english = stopwords.words('english')

print('Stop words\n')
print(stopwords_english)

print('\nPunctuation\n')
print(string.punctuation)
```

-   Removing stop words and punctuation

<!--listend-->

```python
tweets_clean = []

for word in tweet_tokens: # Go through every word in your tokens list
    if (word not in stopwords_english and  # remove stopwords
	word not in string.punctuation):  # remove punctuation
	tweets_clean.append(word)

print('removed stop words and punctuation:')
print(tweets_clean)
```


### Stemming {#stemming}

```python
# Instantiate stemming class
stemmer = PorterStemmer()

# Create an empty list to store the stems
tweets_stem = []

for word in tweets_clean:
    stem_word = stemmer.stem(word)  # stemming word
    tweets_stem.append(stem_word)  # append to the list

print('stemmed words:')
print(tweets_stem)
```