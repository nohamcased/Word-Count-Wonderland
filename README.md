# word-freq
Word frequency in Moby Dick

## 1. Tools for text processing
![wonderland-cover](https://user-images.githubusercontent.com/86967515/236856607-a0251966-9e95-4afa-a683-e94c3f092b11.jpeg)
<p>What are the most frequent words in Lewis Carrol's novel, Alice in Wonderland, and how often do they occur?</p>
<p>In this notebook, I'll scrape the novel <em>Alice in Wonderland</em> from the website <a href="https://www.gutenberg.org/">Project Gutenberg</a> (which contains a large corpus of books) using the Python package <code>requests</code>. Then I'll extract words from this web data using <code>BeautifulSoup</code>. Finally, I'll dive into analyzing the distribution of words using the Natural Language ToolKit (<code>nltk</code>) and <code>Counter</code>.</p>

```
# Importing requests, BeautifulSoup, nltk, and Counter
import requests
from bs4 import BeautifulSoup
import nltk
from collections import Counter
```

## 2. Request Alice in Wonderland
<p>To fetch the HTML file with Alice in Wonderland I'm going to use the <code>request</code> package to make a <code>GET</code> request for the website, which means I'm <em>getting</em> data from it. This is what you're doing through a browser when visiting a webpage, but here I'm getting the requested page directly into Python instead. </p>

```
# Getting the Alice in Wonderland HTML 
r = requests.get('https://www.gutenberg.org/cache/epub/11/pg11-images.html')

# Setting the correct text encoding of the HTML page
r.encoding = 'utf-8'

# Extracting the HTML from the request object
html = r.text

# Printing the first 2000 characters in html
print(html[0:2000])
```

## 3. Get the text from the HTML
<p>This HTML is not quite what I want. However, it does <em>contain</em> what we want: the text of <em>Alice in Wonderland</em>. What I need to do now is <em>wrangle</em> this HTML to extract the text of the novel. For this I'll use the package <code>BeautifulSoup</code>.</p>
<p>Firstly, a word on the name of the package: Beautiful Soup? In web development, the term "tag soup" refers to structurally or syntactically incorrect HTML code written for a web page. What Beautiful Soup does best is to make tag soup beautiful again and to extract information from it with ease! In fact, the main object created and queried when using this package is called <code>BeautifulSoup</code>.</p>

```
# Creating a BeautifulSoup object from the HTML
soup = BeautifulSoup(html, "html.parser")

# Getting the text out of the soup
text = soup.get_text()

# Printing out text between characters 32000 and 34000
print(text[32000:34000])
```

## 4. Extract the words
<p>Now that I have the text of interest, it's time to count how many times each word appears, and for this we'll use <code>nltk</code> – the Natural Language Toolkit. I'll start by tokenizing the text, that is, remove everything that isn't a word (whitespace, punctuation, etc.) and then split the text into a list of words.</p>

```
# Creating a tokenizer
tokenizer = nltk.tokenize.RegexpTokenizer('\w+')

# Tokenizing the text
tokens = tokenizer.tokenize(text)

# Printing out the first 8 words / tokens 
print(tokens[:8])
```

## 5. Make the words lowercase
<p>OK! Nearly there. Note that in the above 'Or' has a capital 'O' and that in other places it may not, but both 'Or' and 'or' should be counted as the same word. For this reason, I'll build a list of all words in <em>Alice in Wonderland</em> in which all capital letters have been made lower case.</p>

```
# Create a list called words containing all tokens transformed to lower-case
words = [token.lower() for token in tokens]

# Printing out the first 8 words / tokens 
print(words[:8])
```

## 6. Load in stop words
<p>It is common practice to remove words that appear a lot in the English language such as 'the', 'of' and 'a' because they're not so interesting. Such words are known as <em>stop words</em>. The package <code>nltk</code> includes a good list of stop words in English that I can use.</p>

```
# Getting the English stop words from nltk
sw = nltk.corpus.stopwords.words('english')

# Printing out the first eight stop words
print(sw[:8])
```

## 7. Remove stop words in Alice in Wonderland
<p>I want to create a new list with all <code>words</code> in Alice in Wonderland, except those that are stop words (that is, those words listed in <code>sw</code>).</p>

```
# Create a list words_ns containing all words that are in words but not in sw
words_ns = [word for word in words if word not in sw]

# Printing the first 5 words_ns to check that stop words are gone
print(words_ns[:5])
```

## 8. We have the answer
<p>Our original question was:</p>
<blockquote>
  <p>What are the most frequent words in Lewis Carrol's novel Alice in Wonderland and how often do they occur?</p>
</blockquote>
<p>I'm now ready to answer that! </p>

```
# Initialize a Counter object from our processed list of words
count = Counter(words_ns)

# Store 10 most common words and their counts as top_ten
top_ten = count.most_common(10)

# Print the top ten words and their counts
print(top_ten)
```
<img width="1222" alt="Screen Shot 2023-05-08 at 09 49 00" src="https://user-images.githubusercontent.com/86967515/236856675-e9a212b9-57e4-44e3-a599-deb95bcae240.png">


## 9. The most common word
<p>So, what word turned out to be the most common word in Alice in Wonderland?</p>

```
# What's the most common word in Alice in Wonderland?
most_common_word = 'said'
```

