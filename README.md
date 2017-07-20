## GloVe: Global Vectors for Word Representation


| nearest neighbors of <br/> <em>frog</em> | Litoria             |  Leptodactylidae | Rana | Eleutherodactylus |
| --- | ------------------------------- | ------------------- | ---------------- | ------------------- |
| Pictures | <img src="http://nlp.stanford.edu/projects/glove/images/litoria.jpg"></img> | <img src="http://nlp.stanford.edu/projects/glove/images/leptodactylidae.jpg"></img> | <img src="http://nlp.stanford.edu/projects/glove/images/rana.jpg"></img> | <img src="http://nlp.stanford.edu/projects/glove/images/eleutherodactylus.jpg"></img> |

| Comparisons | man -> woman             |  city -> zip | comparative -> superlative |
| --- | ------------------------|-------------------------|-------------------------|
| GloVe Geometry | <img src="http://nlp.stanford.edu/projects/glove/images/man_woman_small.jpg"></img>  | <img src="http://nlp.stanford.edu/projects/glove/images/city_zip_small.jpg"></img> | <img src="http://nlp.stanford.edu/projects/glove/images/comparative_superlative_small.jpg"></img> |

We provide an implementation of the GloVe model for learning word representations, and describe how to download web-dataset vectors or train your own. See the [project page](http://nlp.stanford.edu/projects/glove/) or the [paper](http://nlp.stanford.edu/pubs/glove.pdf) for more information on glove vectors.

## Download pre-trained word vectors
The links below contain word vectors obtained from the respective corpora. If you want word vectors trained on massive web datasets, you need only download one of these text files! Pre-trained word vectors are made available under the <a href="http://opendatacommons.org/licenses/pddl/">Public Domain Dedication and License</a>. 
<div class="entry">
<ul style="padding-left:0px; margin-top:0px; margin-bottom:0px">
  <li> Common Crawl (42B tokens, 1.9M vocab, uncased, 300d vectors, 1.75 GB download): <a href="http://nlp.stanford.edu/data/wordvecs/glove.42B.300d.zip">glove.42B.300d.zip</a> </li>
  <li> Common Crawl (840B tokens, 2.2M vocab, cased, 300d vectors, 2.03 GB download): <a href="http://nlp.stanford.edu/data/wordvecs/glove.840B.300d.zip">glove.840B.300d.zip</a> </li>
  <li> Wikipedia 2014 + Gigaword 5 (6B tokens, 400K vocab, uncased, 300d vectors, 822 MB download): <a href="http://nlp.stanford.edu/data/wordvecs/glove.6B.zip">glove.6B.zip</a> </li>
  <li> Twitter (2B tweets, 27B tokens, 1.2M vocab, uncased, 200d vectors, 1.42 GB download): <a href="http://nlp.stanford.edu/data/wordvecs/glove.twitter.27B.zip">glove.twitter.27B.zip</a>
</ul>
</div>

## Train word vectors on a new corpus

<img src="https://travis-ci.org/stanfordnlp/GloVe.svg?branch=master"></img>

If the web datasets above don't match the semantics of your end use case, you can train word vectors on your own corpus.

    $ git clone http://github.com/stanfordnlp/glove
    $ cd glove && make
    $ ./demo.sh

The demo.sh scipt downloads a small corpus, consisting of the first 100M characters of Wikipedia. It collects unigram counts, constructs and shuffles cooccurrence data, and trains a simple version of the GloVe model. It also runs a word analogy evaluation script in python to verify word vector quality. More details about training on your own corpus can be found by reading [demo.sh](https://github.com/stanfordnlp/GloVe/blob/master/demo.sh) or the [src/README.md](https://github.com/stanfordnlp/GloVe/tree/master/src)

## Here starts Tony's memo

To use, please follow the instructions below. For details, please go to src/ directory and read the doc.

> Let's assume you have one or several txt files. Each line represents a sentence. You need to tokenize and then concatenate all together. You should do it with your own scripts. Then you need to count the tokens:
> 
> ./build/vocab_count -verbose 2 -max-vocab 100000 -min-count 5 < data/wb_glove_corpus.txt > data/wb_glove_vocab_count.txt
> 
> Then you generate co-occurance stats file:
> 
> ./build/cooccur -verbose 2 -symmetric 1 -window-size 5 -vocab-file data/wb_glove_vocab_count.txt -memory 8.0 -overflow-file tempoverflow < data/wb_glove_corpus.txt > wb_glove_cooccurrences.bin
> 
> Next you shuffle:
> 
> ./build/shuffle -verbose 2 -memory 8.0 < data/wb_glove_cooccurrences.bin > data/wb_glove_cooccurrence.shuf.bin
> 
> Finally compute vec:
> 
> ./build/glove -input-file data/wb_glove_cooccurrence.shuf.bin -vocab-file data/wb_glove_vocab_count.txt -save-file data/wb_glove_vectors -gradsq-file data/gradsq -verbose 2 -write-header 1 -vector-size 200 -iter 20 -threads 16 -alpha 0.75 -x-max 100.0 -eta 0.05 -binary 0 -model 2
> 
> Configuration details can be found in src/
> > 

### License
All work contained in this package is licensed under the Apache License, Version 2.0. See the include LICENSE file.
