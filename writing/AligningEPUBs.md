This is the writeup I did for a Hackathon Project coded by [Martin Quiazon](https://www.linkedin.com/in/martinquiazon/) and me while working at Benetech in 2021.  Nothing came of the project so I'm posting it here as a writing sample  [You can find the source code on GitHub](https://github.com/TalkyTeam/TalkyTalky).  After we completed the project, we saw that some similar projects had been attempted, so we didn't pursue it further. 

The audience was my coworkers, programmers very familiar with eBook standards like EPUB3 and SMIL, but less so with Natural Language Processing.

# Aligning an audio file with an EPUB3

## Problem

There are some public repositories of free books and free audio recordings, but they
are not synchronized in any meaningful way. We've found that synchronized text and
speech in a reader can help our users with certain reading disabilities.  Automated Text-to-Speech technologies can provide audio for books, but I've gotten feedback from users that they prefer the voices of real people.

The most prominent free audio book project is the [Librivox](https://librivox.org/) project, and the most
prominent free book project is [Project Gutenburg](https://www.gutenberg.org/). Both of these project deal in works in the public domain.  Librivox are human voice recordings which are unmatched with their corresponding texts.

## What we hope to achieve

Given an mp3 file and an existing EPUB3 file, produce a new EPUB3 file that contains
the mp3 and text synchronization information in a SMIL file. 

## What we did

We knew that we'd have to simplify the project a bit to give us something we could
complete in a few days.

To simplify the problem we added the following specifications:

- Synchronization is at the sentence level rather than the word level
- We will start by manually matching the mp3 chapters to html chapters for processing and work on automating the sentence-to-sentence matching within chapters.

## Tools

For the conversion of the mp3 file to text, we used [AWS Transcribe](https://aws.amazon.com/transcribe/). We considered  [Google Speech to Text](https://cloud.google.com/speech-to-text), but ultimately decided to take advantage of our experience
with AWS. We uploaded files manually to AWS Transcribe, but that could also be done
by API calls. AWS Transcribe returns a JSON document that contains, among other
things, a list of the words contained in the mp3 and their timings.

For parsing and assembling the parts, we used Python 3 and the [Natural Language
Toolkit (NLTK)](https://www.nltk.org/).

We used [Thorium](https://www.edrlab.org/software/thorium-reader/) to test the synchronized reading. It was the best thing available and it even runs on Linux.

## Representing timings in EPUB with SMIL

### Initial algorithm

Our plan was to

1. Turn the mp3 into a text transcription
2. Label the sentences in the HTML by inserting SMIL-friendly tags with id's
3. Strip out HTML and turn the EPUB3 chapter into text
4. Somehow use machine learning to match up the texts
5. Combine the HTML id's and the timing data from the transcription to create a SMIL file that the reader can use to synchronize them.

The problem that stands out is "Somehow use machine learning to match up the texts"

## Related work

Sometimes the hard part is to find how other people have framed the problem, so that
you can find different ways it's been solved.

Some terminology:

- Bi-text: Two side by side texts.
- Alignment: The "matching" of corresponding parts of two texts
- Word alignment
- Sentence alignment
- Prosody: The "rhythm" of language. Refers to accents and, what we care about,
  pauses.
- POS: Part of speech. As in, this POS tagger tags all of the words in a sentence.
- Most NLTK libraries use the "Brown" POS tags, from the [Brown University Corpus]([Brown Corpus - Wikipedia](https://en.wikipedia.org/wiki/Brown_Corpus#Part-of-speech_tags_used))
- ASR: Automatic speech recognition
- MT: Machine translation
- NLP: Natural language processing

### Multilingual machine translation

We found a lot of work very similar to what we were looking for was related to machine translation. "Bitext word alignment" refers to taking two texts, normally in different languages, and matching them word to word. In fact, there was much more work on this than on matching two texts in the same language.

#### Example

• [A Sentence Meaning Based Alignment Method for Parallel Text Corpora
Preparation](https://arxiv.org/pdf/1509.09093)

### Audio-to-text alignment

There was some work in audio-to-text alignment, but the existing work had two
problems:

1. They actually referred to examining the audio wave forms. We're much more comfortable dealing with the text transcription. Also, dealing with audio wave forms seems like it could take up a lot of memory.
2. In cases where they matched an audio file to a text file, the text file was already a transcription of the audio file (although perhaps a manual transcription.) What we wanted was a fuzzy match between two documents that might have omitted words 
   or phonetic spelling errors.

#### Examples

- [Experiments on Sentence Boundary Detection](https://aclanthology.org/A00-1012.pdf)
- [Audio-to-text alignment for speech recognition with very limited resources](https://xavieranguera.com/papers/IS2014_phonealignment.pdf)

## Preparing the data for alignment

One common theme among alignment algorithms available through the NLTK is that
they two lists of sentences, to align sentences or a pair of sentences, to align words.

As it turns out, splitting on words is pretty easy to do accurately. Splitting a transcripton on sentences is pretty hard. It's difficult to tell if a pause represents a period, ellipse, or comma.

You could skip the sentence alignment, and do word alignment instead. But with the
text size we're starting with, a whole chapter, this is not recommended. It's common
for alignment algorithms, which are written to handle multilingual sentence matches
with different word order, to compare every word in each sentence to other, resulting in an O-squared order of computation. To limit this computation, a common approach is to align successively smaller parts of the texts. For example, chapters, paragraphs, sentences, and words.

Most of the sentence matching algorithms make one or more of these assumptions to
work well:

- A sentence in each text corresponds on a 1:1 basis
- Sentences are already accurately marked
- There are no extraneous sentences at the beginning or end of either the epub list or transcription list that have no corresponding matches

## Preparing the HTML file

To break the chapter into paragraphs on the HTML side, we were fortunate enough to
find that HTML paragraph tags were accurately used. So, as a first pass, we created a list of paragraph text strings.

```python
def extract_paragraphs(infilepath):
"""
Extract text existing within paragraph elements
:param infilepath:
:return:
"""
if os.path.exists(infilepath):
with open(infilepath, 'r') as input_file:
xhtml = input_file.read()
if xhtml:
soup = BeautifulSoup(xhtml, 'html.parser')
paragraphs = []
for para in soup.find_all(["p","h1","h2","h3"]):
if para and para.strings and contains_interesting_text('
'.join(para.strings)):
paragraphs.append(' '.join(para.strings))
return paragraphs
```

The next pass was to break these paragraphs into sentences. NTLK has a default
sentence tokenizer called the "Punkt" tokenizer. It distinguishes itself from simply
splitting paragraphs on periods largely by using some heuristics to better recognize
abbreviations and exclude them as sentence breaks.

- Punkt algorithm paper: [Unsupervised Multilingual Sentence Boundary Detection](https://www.mitpressjournals.org/doi/pdf/10.1162/coli.2006.32.4.485)

Tokenizing the sentences with NLTK was this easy:

```python
def paragraphs_to_sentences(paragraphs):
"""
Turn each a list of paragraphs into a list of sentences
:param paragraphs::return:
"""
sentence_list = []
for paragraph in paragraphs:
sentences = sent_tokenize(paragraph, language='english')
for sentence in sentences:
sentence_list.append(sentence)
return sentence_list
```

After we found our sentence boundaries, we added span element wrappers around
each one with element id's that could be referred to by the future SMIL file.
We also removed the Gutenberg boilerplate text, since it would have no corresponding
audio.

**Before**

```
<p>But Buck was neither house-dog nor kennel-dog. The whole realm was his. He plunged
into the swimming tank or went hunting with the Judge’s sons; he escorted Mollie and
Alice, the Judge’s daughters, on long twilight or early morning rambles; on wintry
nights he lay at the Judge’s feet before the roaring library fire; he carried the
Judge’s grandsons on his back, or rolled them in the grass, and guarded their footsteps
through wild adventures down to the fountain in the stable yard, and even beyond, where
the paddocks were, and the berry patches. Among the terriers he stalked imperiously,
and Toots and Ysabel he utterly ignored, for he was king,—king over all creeping,
crawling, flying things of Judge Miller’s place, humans included.</p>
```

**After**

```
<p>
<span id="smil_sent19">But Buck was neither house-dog nor kennel-dog.</span>
<span id="smil_sent20">The whole realm was his.</span>
<span id="smil_sent21">He plunged into the swimming tank or went hunting with the
Judge’s sons; he escorted Mollie and Alice, the Judge’s daughters, on long twilight or
early morning rambles; on wintry nights he lay at the Judge’s feet before the roaring
library fire; he carried the Judge’s grandsons on his back, or rolled them in the
grass, and guarded their footsteps through wild adventures down to the fountain in the
stable yard, and even beyond, where the paddocks were, and the berry patches.</span>
<span id="smil_sent22">Among the terriers he stalked imperiously, and Toots and
Ysabel he utterly ignored, for he was king,—king over all creeping, crawling, flying
things of Judge Miller’s place, humans included.</span>
</p>
```

The result of the preparation is a data structure containing the list of sentences and their span element ID.

## Doing the alignment

Surprisingly, one of the standard algorithms for aligning two texts for machine
translation doesn't use any contextual information about those languages at all. It's called the Gale and Church algorithm, which takes a list of the character lengths of the sentences and uses that information to make the match. If the sentences are accurately transcribed, this should work pretty well.

While looking for people who had done "monolingual alignment" instead of multilingual alignment for machine translation, we noticed this troubling paragraph:

> For bilingual texts, Gale and Church (1991) demonstrated the extraordinary
> effectiveness of a global alignment dynamic programming algorithm, where
> the basic similarity score was based on the difference in sentence lengths,
> measured in characters. Such methods fail to work in the monolingual case.
> Gale and Church’s algorithm (using the implementation of Danielsson and
> Ridings (1997)) yields 2% precision at 2.85% recall on the Britannica corpus.

- [Towards Robust Context-Sensitive Sentence Alignment for Monolingual Corpora](https://www.aclweb.org/anthology/E06-1021.pdf)

However, after thinking this through a bit, we realized it might work after all. The
research done on *monolingual* corpora is usually about trying to bring together
different sources of data, like two news stories about same topic from different web
sites, and matching those sentences. In our case, it's more like the machine translation problem, and we expect not only the sentences, but the words to be in the same order and of the same length. So we decided to do this implementation with Gale and Church. We repeated the experiment using both character counts and word counts.

The former is how Gale and Church defined it, but we got pretty similar results with
word counts.

To call the NLTK implementation of the sentence alignment algorithm, you need to
pass in two lists: one is a list of lengths of the HTML sentences, the other a list of lengths of transcript sentences. The align_blocks call returns a list of mappings as pairs of indexes into the two original arrays.

```python
def align_trans_to_html_sentences(transcript_sentences, html_sentences,
match_criteria=WORD_LENGTH):
  transcript_lengths = list(map(lambda s: s.character_count, transcript_sentences))
  html_sentence_lengths = list(map(lambda s: s.character_count, html_sentences))
  result = align_blocks(html_sentence_lengths, transcript_lengths)
  sentence_pairs = []
  for sentence_index_pair in result:
    sentence_pair = (html_sentences[sentence_index_pair[0]],
    transcript_sentences[sentence_index_pair[1]])
  sentence_pairs.append(sentence_pair)
  return sentence_pairs
```

So, given these sentences

<table role="presentation">
<tbody>
<tr>
<th> Gutenberg EPUB </th>
<th> Librivox Transcription </th>
</tr>
<tr>
<td> THE TALE OF </td>
<td> The Tale of Peter Rabbit by Beatrix Potter. </td>
</tr>
<tr>
<td> PETER RABBIT </td>
<td> This liberal box recording is in the public domain. </td>
</tr>
<tr>
<td> BY </td>
<td> Once upon a time, there were four little rabbits and their names were Flopsy, Mops, E Cottontail and Peter. </td>
</tr>
<tr>
<td> BEATRIX POTTER </td>
<td> They lived with their mother in a sandbank underneath the root of a very big fir tree. </td>
</tr>
<tr>
<td> FREDERICK WARNE </td>
<td> Now, my dears said, old Mrs Rabbit, one morning you may go into the fields or down the lane. </td>
</tr>
<tr>
<td> FREDERICK WARNE </td>
<td> But don't go into Mr McGregor's garden. </td>
</tr>
<tr>
<td> Once upon a time there were four little Rabbits, and their names were--&#45; </td>
<td> Your father had an accident. </td>
</tr>
<tr>
<td> Flopsy,  Mopsy,  Cotton-tail, and Peter. </td>
<td> There he was. </td>
</tr>
<tr>
<td> They lived with their Mother in a sand-bank, underneath the root of a very big fir-tree. </td>
<td> Put in a pie by Mrs McGregor. </td>
</tr>
<tr>
<td> 'Now my dears,' said old Mrs. Rabbit one morning, 'you may go into the fields or down the lane, but don't go into Mr. McGregor's garden: your Father had an accident there; he was put in a pie by Mrs. </td>
<td> Now run along and don't get into mischief. </td>
</tr>
<tr>
<td> McGregor.' </td>
<td> I am going out. </td>
</tr>
<tr>
<td> 'Now run along, and don't get into mischief. </td>
<td> Then old Mrs Rabbit took a basket and her umbrella and went through the wood to the Bakers. </td>
</tr>
<tr>
<td> I am going out.' </td>
<td> She bought a loaf of brown bread and five current buns. </td>
</tr>
<tr>
<td> Then old Mrs. Rabbit took a basket and her umbrella, and went through the wood to the baker's. </td>
<td> Flopsy, mops and cottontail who were good little bunnies went down the lane together, together Blackberries. </td>
</tr>
<tr>
<td> She bought a loaf of brown bread and five currant buns. </td>
<td> But Peter, who was very naughty, ran straightaway to Mr McGregor's Garden and squeezed under the gate. </td>
</tr>
<tr>
<td> Flopsy, Mopsy, and Cotton-tail, who were good little bunnies, went down the lane to gather blackberries: </td>
<td> </td>
</tr>
<tr>
<td> But Peter, who was very naughty, ran straight away to Mr. McGregor's garden, and squeezed under the gate&#33; </td>
<td> </td>
</tr>
</tbody></table>

We have these two lists of sentence lengths (in words)
\[43, 51, 107, 86, 92, 39, 28, 13, 29, 42, 15, 91, 55, 108, 102]
\[11, 12, 2, 14, 15, 15, 70, 38, 88, 199, 10, 44, 16, 94, 55, 104, 105]
And, the align_blocks call returns the following mappings
\[(0, 0), (1, 0), (3, 1), (4, 1), (5, 2), (6, 2), (7, 3), (8, 3), (9, 4), (9, 5), (10,
6), (11, 7), (11, 8), (12, 9), (13, 10), (13, 11), (14, 12), (15, 13), (16, 14)]
Which look like these matches

<table role="presentation">
  <tbody>
<tr>
<th> Gutenberg EPUB </th>
<th> Librivox Transcription </th>
</tr>
<tr>
<td> THE TALE OF </td>
<td> The Tale of Peter Rabbit by Beatrix Potter. </td>
</tr>
<tr>
<td> PETER RABBIT </td>
<td> The Tale of Peter Rabbit by Beatrix Potter. </td>
</tr>
<tr>
<td> BEATRIX POTTER </td>
<td> This liberal box recording is in the public domain. </td>
</tr>
<tr>
<td> FREDERICK WARNE </td>
<td> This liberal box recording is in the public domain. </td>
</tr>
<tr>
<td> FREDERICK WARNE </td>
<td> Once upon a time, there were four little rabbits and their names were Flopsy, Mops, E Cottontail and Peter. </td>
</tr>
<tr>
<td> Once upon a time there were four little Rabbits, and their names were--&#45; </td>
<td> Once upon a time, there were four little rabbits and their names were Flopsy, Mops, E Cottontail and Peter. </td>
</tr>
<tr>
<td> Flopsy, </td>
<td> They lived with their mother in a sandbank underneath the root of a very big fir tree. </td>
</tr>
<tr>
<td> Mopsy, </td>
</tr>
<tr>
<td> Cotton-tail, </td>
</tr>
<tr>
<td> and Peter. </td>
</tr>
<tr>
<td> They lived with their Mother in a sand-bank, underneath the root of a very big fir-tree. </td>
<td> They lived with their mother in a sandbank underneath the root of a very big fir tree. </td>
</tr>
<tr>
<td> 'Now my dears,' said old Mrs. Rabbit one morning, 'you may go into the fields or down the lane, but don't go into Mr. McGregor's garden: your Father had an accident there; he was put in a pie by Mrs. </td>
<td> Now, my dears said, old Mrs Rabbit, one morning you may go into the fields or down the lane. </td>
</tr>
<tr>
<td> 'Now my dears,' said old Mrs. Rabbit one morning, 'you may go into the fields or down the lane, but don't go into Mr. McGregor's garden: your Father had an accident there; he was put in a pie by Mrs. </td>
<td> But don't go into Mr McGregor's garden. </td>
</tr>
<tr>
<td> McGregor.' </td>
<td> Your father had an accident. </td>
</tr>
<tr>
<td> 'Now run along, and don't get into mischief. </td>
<td> There he was. </td>
</tr>
<tr>
<td> 'Now run along, and don't get into mischief. </td>
<td> Put in a pie by Mrs McGregor. </td>
</tr>
<tr>
<td> I am going out.' </td>
<td> Now run along and don't get into mischief. </td>
</tr>
<tr>
<td> Then old Mrs. Rabbit took a basket and her umbrella, and went through the wood to the baker's. </td>
<td> I am going out. </td>
</tr>
<tr>
<td> Then old Mrs. Rabbit took a basket and her umbrella, and went through the wood to the baker's. </td>
<td> Then old Mrs Rabbit took a basket and her umbrella and went through the wood to the Bakers. </td>
</tr>
<tr>
<td> She bought a loaf of brown bread and five currant buns. </td>
<td> She bought a loaf of brown bread and five current buns. </td>
</tr>
<tr>
<td> Flopsy, Mopsy, and Cotton-tail, who were good little bunnies, went down the lane to gather blackberries: </td>
<td> Flopsy, mops and cottontail who were good little bunnies went down the lane together, together Blackberries. </td>
</tr>
<tr>
<td> But Peter, who was very naughty, ran straight away to Mr. McGregor's garden, and squeezed under the gate&#33; </td>
<td> But Peter, who was very naughty, ran straightaway to Mr McGregor's Garden and squeezed under the gate. </td>
</tr>
</tbody></table>

## Looking at the results

Although the alignment seems to be better than doing nothing, we are seeing some
problems. Now that we've given this a try we have a better idea of what they are and
how to improve the results.

### Sentence boundaries

The sentence boundaries in the transcription not matching those in the EPUB are really messing us up. They're causing totally different sentence lengths to be generated. Plus, if we are doing sentence highlighting, the audio won't correspond to one sentence.

### Headers and footers

Gutenberg has a boilerplate header and footer that we easily stripped with string
matching. The Librivox header is not as easy. It's often read by whomever is reading
the story, so it doesn't always sound exactly the same.

## What else to try

### Match on phonemes

Translate both the transcription and the EPUB into phonetic representations of the
text. They should be more similar than the current texts, because most of the
differences are in misinterpreted phonetics in the transcript. Then a fairly simple
similarity measure, like [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance), could be used to align those phonetic
representations.

### Use order more in alignment

We are not doing much to leverage the fact that the text we are comparing is in the
same order. We are using an algorithm designed for multilingual corpora which takes
into account that different languages reverse the order of verbs, nouns, and
adjectives. We don't have to take that into account. We could reduce the search space by comparing a moving window of words rather than the entire document.

If we could do word alignment for the entire chapter instead of sentence alignment, we would dispense with the problems we are seeing in recognizing sentence boundaries. 

### Use landmarks of high similarity confidence

Some similar sounding phrases look very different in the text, but some are dead on.
Can we find those dead on matches and line them up, then use those landmarks to
improve the likelihood of adjacent matches?

### Use smaller units than sentences

Although at this point, I am thinking windows of word matches might be our best bet,
we did also look at a potential method which an interesting chance to look at
more NLP tools. Late in the week, we started to look at several methods of breaking
sentences down into clauses. One was a dumb splitting at commas and semicolons.

Because people pause at commas and semicolons, and the transcriber takes a pause
as an end of sentence, we were seeing sentence breaks in the transcription where
there were commas and semicolons. This change didn't make much difference by
itself. For one thing, now that all of the sentences were replaced with short clauses, clauses length was not as useful as a similarity measure between corresponding clauses. For another, there were enough clauses still joined into sentences in the transcription to throw off the matching.

The other method of breaking into clauses that we tried was to use NLTK to break up
the sentences, using sentence parsing or sentence chunking. In NLTK, chunking seems
to refer to dividing a sentence into related chunks like "noun phrases". For example, "The big red dog" is a noun phrase because it's a noun and related words. 

On the other hand, sentence parsing results in a parse tree showing how every word is part of the sentence. We ran out of time to investigate this further, but here is an example of what 'chunking' looks like. In NLTK, both the chunking and parsing algorithms require you to create grammars to define what makes up the parts of a sentence, using POS abbreviations.

We didn't get very far investigating this, but the NLTK documentation contains a [section on chunking which might help us get useful clauses out of sentences](https://www.nltk.org/book_1ed/ch07.html#chunking).

# Conclusion

As is often the case with Machine Learning problems you can get most of the way there with an unexpectedly simple solution: in this case, sentence length.  (Which is probably why these sorts of problems and solutions are so beloved by hackathons.)   Getting to perfection becomes exponentially harder.

We covered some suggestions for other methods to try to improve accuracy, but I'd also be curious to see what users find to be "good enough."  Depending on how the book is being used, it might not be a big deal to mislabel a few words.  Or maybe it would be extremely frustrating.  This might be a good stage to look at our audience and collect their feedback.