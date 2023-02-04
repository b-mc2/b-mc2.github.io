# Natural Language Processing

Natural language processing (NLP) is a branch of artificial intelligence that helps computers understand, interpret and manipulate human language. NLP draws from many disciplines, including computer science and computational linguistics, in its pursuit to fill the gap between human communication and computer understanding. 

___
## Text Similarity
Text similarity can be broken down into two components, semantic similarity and lexical similarity. Given a pair of text,
the semantic similarity of the pair refers to how close the documents are in meaning. Whereas, lexical similarity is a measure 
of overlap in vocabulary. If both documents in the pairs have the same vocabularies, then they would have a lexical similarity of 1 and vice versa of 0 if there was no overlap in vocabularies.



### Gestalt pattern matching
[Documentation]("https://en.wikipedia.org/wiki/Gestalt_pattern_matching")

Gestalt pattern matching, also Ratcliff/Obershelp pattern recognition, is a string-matching algorithm for determining the similarity of two strings.

| String | letter | letter | letter | letter | letter | letter | letter | letter | letter |
|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|
| **S1** | W      | I      | K      | I      | **M**  | **E**  | D      | I      | A      |
| **S1** | W      | I      | K      | I      | **M**  | **A**  | N      | I      | A      |

The longest common substring is WIKIM (grey) with 5 characters. There is no further substring on the left. The non-matching substrings on the right side are EDIA and ANIA. They again have a longest common substring IA (dark gray) with length 2. The similarity metric is determined by:

![Gestalt Formula](https://wikimedia.org/api/rest_v1/media/math/render/svg/e91c645324aeba5092b44adbc8b2403b07658104)

```python
# Drqr Implementation in Python
def real_quick_ratio(s1: str, s2: str) -> float:
    """Return an upper bound on ratio() very quickly."""
    l1, l2 = len(s1), len(s2)
    length = l1 + l2

    if not length:
        return 1.0

    return 2.0 * min(l1, l2) / length
```

```python
# Dqr Implementation in Python
def quick_ratio(s1: str, s2: str) -> float:
    """Return an upper bound on ratio() relatively quickly."""
    length = len(s1) + len(s2)

    if not length:
        return 1.0

    intersect = collections.Counter(s1) & collections.Counter(s2)
    matches = sum(intersect.values())
    return 2.0 * matches / length
```

---

### Levenshtein Distance
[Documentation]("https://towardsdatascience.com/text-similarity-w-levenshtein-distance-in-python-2f7478986e75")

Levenshtein distance is very impactful because it does not require two strings to be of equal length for them to be compared. Intuitively speaking, Levenshtein distance is quite easy to understand.

Essentially implying that the output distance between the two is the cumulative sum of the single-character edits. 
The larger the output distance implies that more changes were necessary to make the two words equal to each other, and 
the lower the output distance implies that fewer changes were necessary. For example, given a pair of
words `dream` and `dream` the resulting Levenshtein distance would be 0 because the two words are the same. However, 
if the words were `dream` and `steam` the Levenshtein distance would be 2 as you would need to make 2 edits to change `dr` to `st`.

```python
def LevenshteinRatio(string1: str, string2: str):
    """ levenshtein_ratio_and_distance:
        Calculates levenshtein distance between two strings.
        If ratio_calc = True, the function computes the
        levenshtein distance ratio of similarity between two strings
        For all i and j, distance[i,j] will contain the Levenshtein
        distance between the first i characters of s and the
        first j characters of t
    """
    rows = len(string1) + 1
    cols = len(string2) + 1
    # Initialize matrix of zeros
    distance = np.zeros((rows, cols), dtype = int)
    # Iterate over the matrix to compute the cost of deletions,insertions and/or substitutions
    for i in range(1, rows):
        for k in range(1, cols):
            distance[i][0] = i
            distance[0][k] = k
    for col in range(1, cols):
        for row in range(1, rows):
            # the cost of a substitution is 2. If we calculate just distance, then the cost of a substitution is 1
            cost = 0 if string1[row-1] == string2[col-1] else 2
            distance[row][col] = min(
                distance[row-1][col] + 1, # Cost of deletions
                distance[row][col-1] + 1, # Cost of insertions
                distance[row-1][col-1] + cost # Cost of substitutions
            )
    # Computation of the Levenshtein Distance Ratio
    Ratio = ((len(string1) + len(string2)) - distance[row][col]) / (len(string1) + len(string2))
    return Ratio

print(LevenshteinRatio("apple", "apple"))
# 1.0
print(LevenshteinRatio("apple", "apples"))
# 0.9090909090909091
print(LevenshteinRatio("applez", "apples"))
# 0.8333333333333334
print(LevenshteinRatio("apples", "oranges"))
# 0.46153846153846156

```

---

## Extractive Summary

### Naive Extractive Summary
[Documentation]("https://en.wikipedia.org/wiki/Gestalt_pattern_matching")

Summarizing is based on ranks of text sentences using a variation of the TextRank algorithm

Extractive summarization based on TextRank involves extracting all non-stopwords (e.g. The, if, and etc) 
and giving words a score based on the number of occurrences. You then extract sentences from the text and loop through 
each sentence and tallying a score based on the words used.

A naive implementation is below: 
```python
from collections import Counter

class Summarizer:
    def __init__(self):
        self.stopWords = stopWords = ['i', 'me', 'my', 'myself', 'add more stopwords']
	def summary(self, text: str, summary_sentences: int=3, sensitivity: float=1.2) -> dict:
		words = [
			word
			for word in text.split()
			if word.lower() not in self.stopWords
		]
		freqTable = dict(Counter(words))
		sentences = [
			sentence
			for sentence in text.split('. ')
		]
		sentenceValues = {}
		for sentence in sentences:
			sentenceValues[sentence] = 0
			for word in sentence.split():
				sentenceValues[sentence] += freqTable.get(word, 0)
		average = int(sum(sentenceValues.values()) / len(sentenceValues))
		summary = " ".join(
			sentence
			for sentence in sentences
			if sentenceValues[sentence] > sensitivity * average
		)
		n_sentence_summary = " ".join(
			dict(
				sorted(sentenceValues.items(), key=lambda x: x[1], reverse=True)[:summary_sentences]
			)
		)
		top_ten_words = {key: val for key, val in sorted(freqTable.items(), key = lambda ele: ele[1], reverse = True)}
		top_ten_words = [word for word in top_ten_words if word.isalpha()]
		data = {
			"keywords":list(set(words)),
			"frequency_table":freqTable,
			"top_ten_words": top_ten_words,
			"average_score":average,
			"sensitivity_summary":summary,
			"top_sentence":max(sentenceValues.items(), key=lambda x: x[1])[0],
			"n_sentence_summary":n_sentence_summary,
			"sentence_values":sentenceValues,
			"summary_count":len(n_sentence_summary.split())
		}
		return data
```

