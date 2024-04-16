# Using Stanza parser in Google Colaboratory

Official instructions:  
https://stanfordnlp.github.io/stanza/

(Extra, competing notebook:  
https://github.com/stanfordnlp/stanza/blob/main/demo/Stanza_Beginners_Guide.ipynb )

## Paste these into Colab cells:

### But first, select GPU:

1. *Edit*-menu -> *Notebook Settings*:  
1. Select a **GPU** (eg. 'T4 GPU' instead of 'CPU')  
1. *Save*  

If you change this setting later, you might have to run all the cells again.

( How does this differ from:  
*Runtime* -> *Change runtime type* )

---------------------------------------------------------
At least paste the Code blocks into Code cells. Paste the texts into Text cells if you like.

---------------------------------------------------------
The following is optional, get rid of annoying nested scroll bars.
```python
#https://colab.research.google.com/gist/blois/635d82605b163fc8fbb8ae1128c7f388/no_vertical_scroll.ipynb
from google.colab import output

output.no_vertical_scroll()
```

## Install Stanza (in the Colab virtual machine):

```python
# Runs a shell command from Jupyter. Normally a ! is used instead of %, 
# but I've seen a recommendation to run pip with a magical %.
%pip install stanza
# %pip install -q stanza
# Should we use -q for quiet? Then it won't show the versions.
```

## Initialize Pipeline for English:

Change the language code `'en'` if you use another language, eg. `'et'` for Estonian. It should understand also language names, eg. `'English'`.

```python
# Import stanza to use it in Python:
import stanza

nlp = stanza.Pipeline('en')  # initialize English neural pipeline
```

( Stanza uses GPU by default, but if it reports: `INFO:stanza:Using device: cpu`  
See "But first" above.  

`INFO:stanza:Using device: cuda` means it uses GPU, which is good.

I processed a 500 kB document in Finnish:
- Using only CPU, I cancelled it after 8 minutes.
- With a T4 GPU it took 67 seconds. )

```python
# Put a two sentence document into the variable 'text':
text = """Barack Obama was born in 
Hawaii. He's in America."""
# See the separate instructions on how to load a file instead.
```

```python
# Run annotation over a document that is saved in the variable called 'text':
doc = nlp(text)
```

```python
# Convert doc to .conllu, and write to file.
# https://stanfordnlp.github.io/stanza/data_conversion.html#document-to-conll
from stanza.utils.conll import CoNLL

CoNLL.write_doc2conll(doc, "output.conllu")
# Warning: It will overwrite any file from a previous run, 
# rename the old file if you want to keep it!
```

See the conllu output by double clicking the file in Files sidebar of Colab. You can also download the file on your own computer and open it in a text editor or Excel (how? import?). It is a conventional tsv (tab separated values) text file.

Alternatively run one of these. They show the actual tabs, unlike the Colab editor:

```python
# Print out the whole file output.conllu:
# !cat output.conllu

# Print the first 50 lines of output.conllu:
!head -n 50 output.conllu

# Out of the 200 first lines, print only the last 150 lines,
# i.e. lines 50–200 roughly: 
# !head -n 200 output.conllu | tail -n 150
```

