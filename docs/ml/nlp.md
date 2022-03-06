# Natural Language Processing



---
## Summarization
### Extractive Summary
[Documentation](https://www.geeksforgeeks.org/python-text-summarizer/)

Extractive Summary: This method summarizes the text by selecting the most important subset of sentences from the original text. As the name suggests, it extracts the most important information from the text. This method does not have the capability of text generation by itself and hence the output will always have some part of the original text.

```python
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize, sent_tokenize
from collections import Counter

stopWords = set(stopwords.words("english"))

def Summary(text: str, sensitivity: float=1.2):
    words = [word.lower() for word in word_tokenize(text) if word.lower() not in stopWords]
    freqTable = dict(Counter(words))
    sentences = [sentence.lower() for sentence in sent_tokenize(text)]
    sentenceValues = {}
    for sentence in sentences:
        sentenceValues[sentence] = 0
        for word in sentence.split():
            sentenceValues[sentence] += freqTable.get(word, 0)
    average = int(sum(sentenceValues.values()) / len(sentenceValues))
    summary = " ".join(sentence for sentence in sentences if sentenceValues[sentence] > sensitivity * average)
    return summary
```
The above code removes stopwords from the text (the, and, or, at, etc.)
then counts how frequently each word is used. This sort of becomes the score for a word.
Then it loops through each word of each sentence and tallies up the score for the sentence.
Finally it takes an average score for the sentences and builds the summary from the highest 
scoring sentences (as a multiple of the sensitivity param).

### Abstractive Summary

Abstractive Summary: The idea behind this method is to understand the core context of the original text and produce new text based on this understanding. It can be compared to the way humans read and summarize text in their own way. The output of abstractive summary can have elements not present in the original text.