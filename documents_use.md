## Choose / Get a text to parse

### Choose a supported language:  
See this table for Stanza: https://stanfordnlp.github.io/stanza/performance.html#system-performance-on-ud-treebanks  
or this one for Trankit: https://trankit.readthedocs.io/en/latest/pkgnames.html#pretrained-languages-their-code-names

Preferably get .txt files in utf-8 encoding.

## How to get the text file into Colab?

### Where the files are? Where are the files?
- Local to the Colab Virtual Machine – Gets deleted (soon?) after the session.
- Files in Google Drive – Access is slower, copy them to local disk for training and other heavy use.
- Local files on user's computer – Not visible in Colab, but can be uploaded.
- Public Internet

### Step 1: How to get the files?
- Download from Internet: `wget`. 
  - Option `-nc` => not downloading if a file with the same name already exists.
  - `!wget -nc https://www.gutenberg.org/cache/epub/44217/pg44217.txt`
  - `!wget -nc https://www.gutenberg.org/cache/epub/70941/pg70941.txt`
  - These include licenses and other crap, clean them maybe!
    - The Colab editor can be used (but how to *save as*?)
- Upload from your local machine:
  - Files sidebar in Colab: Upload button
  - `google.colab.files.upload()`
- Google Drive can be mounted
- ...?
- https://colab.research.google.com/notebooks/io.ipynb

## Step 2: Load the file into a Python variable
```python
# Open file pg70941.txt from the same directory.
with open('pg70941.txt', 'r', encoding='utf8') as f:
    text = f.read()
# And read it into a variable called 'text'
```
Now it can be used for parsing, eg:
```python
doc = nlp(text)
```

(How to read from Google Drive?)
```python
# mount Google Drive in Colab

# read file
```
