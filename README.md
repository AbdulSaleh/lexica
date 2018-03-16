# Lexica #

Extract lexicon features from text. Available lexica are:
- LIWC 2007
- LIWC 2015
- NRC EmoLex v0.92
- Agency and Authority Connotation frame

## Running the code ##
Import it into python using `from util import *`

Select the dictionary you want to use:
- `lex = liwc.parse_liwc("2007")` for LIWC 2007
- `lex = liwc.parse_liwc("2015")` for the 2015 version
- `lex = nrc.parse_emolex()` for the NRC EmoLex
- `lex = nrc.parse_optpess()` for the NRC Optimism/Pessimism lexicon (weighted)
- `lex = conno.parse_connotation("agency")` for the Agency connotation frame
- `lex = conno.parse_connotation("authority")` for the Authority connotation frame

Optionally only select certain categories:
`lex = liwc.parse_liwc("2007",whitelist=["posemo","negemo"])`

Extract features using the `extract` function:
- `extract(lex,"this is a text")`:
    will return a dictionary of {category: percentage}
- `extract(lex,"this is a text",percentage=False)`:
    will return a dictionary of {category: raw word count}
    
If lex is a weighted lexicon, each matched word is multiplied by it's category weight

Example:
```
In [1]: from util import *

In [2]: lex = nrc.parse_emolex()

In [3]: extract(lex,"This is a story about a girl named lucky",False)
Out[3]: {'anticipation': 1, 'joy': 2, 'positive': 2, 'surprise': 2}

In [4]: lex = liwc.parse_liwc("2015")

In [5]: extract(lex,"This is a story")
Out[5]:
{'article': 0.25,
 'auxverb': 0.25,
 'focuspresent': 0.25,
 'function': 0.75,
 'ipron': 0.25,
 'pronoun': 0.25,
 'social': 0.25,
 'verb': 0.25}
       
```
## Connotation frames ##
You can also work at a verb-level, instead of the word-level.
Specifically, the connotation frames of *agency* and *authority* only work with verbs.

Use `extractVerbs` to only count verbs towards connotation frames.
Verbs are detected using the SpaCy POS tagger and lemmatizer.

*Note that the results will be different since some verbs are nouns/adjectives sometimes.*

Example:
```
In [1]: from util import *

In [2]: lex = conno.parse_connotation()

In [3]: extractVerbs(lex,"They grabbed and pulled the cool machine")
Out[3]: {'agency_pos': 1.0}

In [4]: extract(lex,"They grabbed and pulled the cool machine")
Out[4]: {'agency_neg': 0.14285714285714285}

```