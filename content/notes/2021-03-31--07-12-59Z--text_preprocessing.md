+++
title = "text-preprocessing"
author = ["felladog"]
date = 2021-03-31T12:57:00+05:45
lastmod = 2021-06-07T14:49:40+05:45
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


## Bag of Words {#bag-of-words}

Bag of word approach, it totally ignores the order of words and simply uses word counts as their basis. For each word in the post, its occurrence is counted and noted in a vector. Not surprisingly, this step is also called vectorization. The vector is typically huge as it contains as many elements as words occur in the whole dataset.

Take, for instance, two example posts with the following word counts:

| Word     | Occurrences in Post 1 | Occurrences in post 2 |
|----------|-----------------------|-----------------------|
| disk     | 1                     | 1                     |
| format   | 1                     | 1                     |
| how      | 1                     | 0                     |
| hard     | 1                     | 1                     |
| my       | 1                     | 0                     |
| problems | 0                     | 1                     |
| to       | 1                     | 0                     |

The columns occurrences in post 1 and occurrences in post 2 can now be treated as
simple vectors.


### Creating Bag of words using Sklearn {#creating-bag-of-words-using-sklearn}

```python
from sklearn.feature_extraction.text import CountVectorizer
vectorizer = CountVectorizer(min_df=1)
content = ["How to format my hard disk", " Hard disk format problems "]
X  = vectorizer.fit_transform(content)
print(vectorizer.get_feature_names())
print("vectors from post 1 and 2")
print(X.toarray().transpose())
```

```text
['disk', 'format', 'hard', 'how', 'my', 'problems', 'to']
vectors from post 1 and 2
[[1 1]
 [1 1]
 [1 1]
 [1 0]
 [1 0]
 [0 1]
 [1 0]]
```


## TF-IDF {#tf-idf}

Counting term frequencies for every post and in addition discount those that appear in many posts. In other words, we want a high value for a given term in a given value, if that term occurs often in that particular post and very seldom anywhere else.
TF stands for the counting part, while IDF factors in the discounting.


### How to Compute: {#how-to-compute}

Typically, the tf-idf weight is composed by two terms: the first computes the normalized Term Frequency (TF), aka. the number of times a word appears in a document, divided by the total number of words in that document; the second term is the Inverse Document Frequency (IDF), computed as the logarithm of the number of the documents in the corpus divided by the number of documents where the specific term appears.


#### TF {#tf}

Term Frequency, which measures how frequently a term occurs in a document. Since every document is different in length, it is possible that a term would appear much more times in long documents than shorter ones. Thus, the term frequency is often divided by the document length (aka. the total number of terms in the document) as a way of normalization:

-   TF(t) = (Number of times term t appears in a document) / (Total number of terms in the document).


#### IDF {#idf}

Inverse Document Frequency, which measures how important a term is. While computing TF, all terms are considered equally important. However it is known that certain terms, such as "is", "of", and "that", may appear a lot of times but have little importance. Thus we need to weigh down the frequent terms while scale up the rare ones, by computing the following:

-   IDF(t) = log\_e(Total number of documents / Number of documents with term t in it).


### Example: {#example}

Consider a document containing 100 words wherein the word cat appears 3 times. The term frequency (i.e., tf) for cat is then (3 / 100) = 0.03. Now, assume we have 10 million documents and the word cat appears in one thousand of these. Then, the inverse document frequency (i.e., idf) is calculated as log(10,000,000 / 1,000) = 4. Thus, the Tf-idf weight is the product of these quantities: 0.03 \* 4 = 0.12.
