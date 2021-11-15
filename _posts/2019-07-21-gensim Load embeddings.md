---
layout: post
title: 'gensim Load embeddings'
date: 2019-07-21 02:53:00 +0800
category: from_cnblogs
slug: p20190721025300
---
# gensim package



```python

from gensim.models.keyedvectors import KeyedVectors

twitter_embedding_path = 'twitter_embedding.emb'
twitter_vocab_path = 'twitter_model.vocab'
foursquare_embedding_path = 'foursquare_embedding.emb'
foursquare_vocab_path = 'foursquare_model.vocab'

# load the embedding vector using gensim
x_vectors = KeyedVectors.load_word2vec_format(foursquare_embedding_path, binary=False, fvocab=foursquare_vocab_path)
y_vectors = KeyedVectors.load_word2vec_format(twitter_embedding_path, binary=False, fvocab=twitter_vocab_path)

print('type(x_vectors)', type(x_vectors))
print('type(x_vectors.vocab)', type(x_vectors.vocab))
print('type(x_vectors.vocab.keys())', type(x_vectors.vocab.keys()))
```



**Content in 'twitter_embedding.emb':**

> 5120 64
> BarackObama -0.079930 0.106491 -0.075812 -0.026447 ...
> mashable 0.046692 -0.038019 -0.055519 ... 
> ...



**Content in 'twitter_model.vocab':**

> BarackObama 3475971
> mashable 2668606
> JonahLupton 2515250
> instagram 2359886
> TheEllenShow 2292545
> cnnbrk 2157283
> nytimes 2141588
> foursquare 2021352
>
> ...


#### Write the embeddings into file
for writing the embeddings into file
ref code patch:
```python
embedding_path = data_path + 'embedding/'
# ....
modelX = word2vec.Word2Vec(walkList_x, negative=10, sg=1, hs=0, size=100, window=4, min_count=0, workers=15, iter=30)
# save the embedding results
modelX.wv.save_word2vec_format(embedding_path + 'twitter.emb', fvocab=embedding_path + 'twitter.vocab')
```