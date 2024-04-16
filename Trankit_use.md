# Using Trankit parser in Google Colaboratory

## Use Trankit

https://github.com/nlp-uoregon/trankit

https://trankit.readthedocs.io/en/latest/overview.html

### Paste these into Colab cells:

At least paste the Code blocks into Code cells. Paste the texts into text cells if you like.

---------------------------------------------------------
Optional, get rid of annoying nested scroll bars. It does not seem to work for the pretty print later though.
```python
#https://colab.research.google.com/gist/blois/635d82605b163fc8fbb8ae1128c7f388/no_vertical_scroll.ipynb
from google.colab import output
output.no_vertical_scroll()
```

```python
# Runs a shell command from Iuppiter. Often ! is used instead of %, but I've
# seen a recommendation to run pip with a magical %.
%pip install trankit
# Should we use -q
# for quiet?
```

The `Pipeline()` will automatically download the huge (1 GB) xlm-roberta-base.
Can we somehow share a common copy? Through Google Drive or something?
Or just save the downloaded copy for the next session for the same user? 

```python
from trankit import Pipeline, trankit2conllu

# initialize a multilingual ??? pipeline
p = Pipeline(lang='english', gpu=True, cache_dir='./cache')

doc = '''Hello! This is Trankit.'''

# perform all tasks on the input
processed_doc = p(doc)
```
A long document would be best to load from a file.

```python
from pprint import pprint  

pprint(processed_doc)
```

## How to write conllu?

#### The following don't work, why?
How to save this *Python dictionary* as a .conllu file?  
https://github.com/nlp-uoregon/trankit/blob/00848efe75e0fd10a8f0a40c589659da5f8941b3/trankit/utils/conll.py#L162  
(or  
https://github.com/nlp-uoregon/trankit/blob/00848efe75e0fd10a8f0a40c589659da5f8941b3/trankit/utils/conll.py#L171  
(erroneous docstring?)  
)

#### Trankit built-in method:
```python
from trankit import trankit2conllu

conlludoc = trankit2conllu(processed_doc)
# This will not write  # text = ...
# Works with multi word tokens.
```
See: https://github.com/nlp-uoregon/trankit/releases/tag/v1.1.0

Problems:
- it doesn' write `# text = ...` and ConlluEditor refuses to work with them `General error: Cannot invoke "String.length()" because the return value of "com.orange.labs.conllparser.ConllSentence.getText()" is null`. Even `--relax` option doesn't help.
- it doesn't put `SpaceAfter=No` in the last column before a stop, and ConlluEditor complains `|incoherent "# text" and forms`.

#### Jenna's methods modified:
```python
# define functions for processing the trankit output format

def token2line(token):
  line = "\t".join([str(token["id"]), token["text"], token.get("lemma", "_"), token.get("upos", "_"), token.get("xpos", "_"), token.get("feats", "_"), str(token.get("head", "_")), token.get("deprel", "_"), token.get("deps", "_"), "_"])
  return line

def dict2conllu(d, filename):
    with open (filename, "w", encoding="UTF8") as f:
        for sent in d["sentences"]:
            print("# text =", sent["text"], file=f)
            for token in sent["tokens"]:
              print(token2line(token), file=f)
            print(file=f)
```

## Test multi word tokens (mwt)

```python
p_fr = Pipeline('french')

doc_text = '''Je sens qu'entre ça et les films de médecins et scientifiques fous que nous
avons déjà vus, nous pourrions emprunter un autre chemin pour l'origine. On
pourra toujours parler à propos d'Averroès de "décentrement du Sujet".'''

out_doc = p_fr.tokenize(doc_text)
```

```python
# Use trankit's method:
print(trankit2conllu(out_doc))
# mwt works.
# text = ... is missing.
```

```
12-13	du	_	_	_	_	_	_	_	_
12	de	_	_	_	_	11	_	_	_
13	le	_	_	_	_	12	_	_	_
14	Sujet	_	_	_	_	13	_	_	_
```

```python
# Use Jenna's functions modified:
dict2conllu(out_doc)
# text = ... ok.
# mwt broken:
# (12, 13)	du	_	_	_	_	_	_	_	_
# 14	Sujet	_	_	_	_	_	_	_	_
```

## Options:

If you don't have multi word tokens, use the Jenna's functions.

-------------------------------------------------
If you need mwt's, use the trankit method:

If you don't need the `# text = ...`'s, you're done

If you need them for ConlluEditor, put in some dummy sentences by using regexes or something:

Search for:  
`^1\t`  
or  
`\n\n1\t`  

Replace with:  
`# text = Not available\n1\t`  
or  
`\n\n# text = Not available\n1\t`  
, respectively. (The second options don't work for the first sentence.)

Then ConlluEditor will complain, but should work(?)

If you really need the `# text = `'s to be valid, hmm.... Use a pretokenized input and combine them.

Or write a script to put the text's in from the FORM column. But then, as the SpaceAfter's are also missing from trankit, you will have space before commas and other punctuation.

## Save / Download your result conllu

