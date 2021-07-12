---
layout: post
title: 'The Power of WordNet and How to Use It in Python'
date: 2021-03-08 15:03:00 +0800
category: from_cnblogs
slug: p20210308150300
---
# The Power of WordNet and How to Use It in Python

Posted on [July 20, 2017](https://blog.xrds.acm.org/2017/07/power-wordnet-use-python/)  by  [Prachi Kumar](https://blog.xrds.acm.org/author/prachikumar/) in this [link](https://blog.xrds.acm.org/2017/07/power-wordnet-use-python/)

In this post, I am going to talk about the relations in WordNet (https://wordnet.princeton.edu) and how you can use these in a Python project. WordNet is a database of English words with different relations between the words.

Take a look at the next four sentences.

1.  “She went home and had pasta.”
2. “Then she cleaned the kitchen and sat on the sofa.”
3. “A little while later, she got up from the couch.”
4. “She walked to her bed and in a few minutes she was snoring loudly.”



In Natural Language Processing, we try to use computer programs  to find the meaning of sentences. In the above four sentences, with the  help of WordNet, a computer program will be able to identify the  following –

1. “pasta” is a type of dish.
2. “kitchen” is a part of “home”.
3. “sofa” is the same thing as “couch”.
4. “snoring” implies “sleeping”.

Let’s get started with using WordNet in Python. It is included as a part of the NLTK (http://www.nltk.org/) corpus. To use it, we need to import it first.

```
>>> from nltk.corpus import wordnet as wn
```

This allows us to invoke WordNet by  simply typing “wn” instead of “wordnet”. Now let us explore some of the  relations that WordNet provides.

# **Synonyms**

WordNet stores synonyms in the form of synsets where each word in the synset shares the same meaning. Basically, each synset is a group of  synonyms. Each synset has a definition associated with it. Relations are stored between different synsets as we’ll see in the next section.  Let’s explore synsets with an example.

Take the word ‘sofa’. The following line in Python gives all the synsets for ‘sofa’.

```
>>> wn.synsets('sofa') 
[Synset('sofa.n.01')]
```

We have only one synset for ‘sofa’ which means that it has only one  context or meaning. Another word like ‘ocean’ will give two synsets  because it has two meanings – one as ‘a water body’ and the other as  ‘being limitless in quantity’.

```
>>> wn.synsets('ocean') 
[Synset('ocean.n.01'), Synset('ocean.n.02')]
```

We can find the definition of a synset with the code given below.

```
>>> wn.synset('sofa.n.01').definition()
u'an upholstered seat for more than one person'
```

To find all the words that share the same meaning as sofa, we need to find all the synonyms in the sofa synset.

```
>>> wn.synset('sofa.n.01').lemma_names()
[u'sofa', u'couch', u'lounge']
```

As can be seen above, ‘couch’ and ‘sofa’ are synonyms and share the same meaning.

# **Hyponyms and Hypernyms**

Hyponyms and Hypernyms are specific and generalized concepts respectively.

For example, ‘beach house’ and ‘guest house’ are hyponyms of ‘house’. They are more specific concepts of ‘house’. And ‘house’ is a hypernym  of ‘guest house’ because it is the general concept.

‘Pasta’ is a hyponym of ‘dish’ and ‘dish’ is a hypernym of ‘pasta’.

Let’s look at the code.

After we find the synset whose hyponyms / hypernyms we want, the following code finds the hyponyms and hypernyms.

```
>>> wn.synset('pasta.n.01').hyponyms() 
[Synset('cannelloni.n.01'), Synset('lasagna.n.01'), 
Synset('macaroni_and_cheese.n.01'), Synset('spaghetti.n.01')]
>>> wn.synset('pasta.n.01').hypernyms()
[Synset('dish.n.02')]
>>> wn.synset('dish.n.02').definition()
u'a particular item of prepared food'
```

As seen, we found the hyponyms and hypernym of pasta and it can be seen that pasta is a type of dish.

# **Meronyms and Holonyms**

Meronyms and Holonyms represent the part-whole relationship. The  meronym represents the part and the holonym represents the whole. For  example, ‘kitchen’ is a meronym of ‘home'(the kitchen is a part of the  home), ‘bread’ is a meronym of ‘sandwich’, and ‘sandwich’ is a holonym  of ‘bread’.

In Python, we have –

```
>>> wn.synsets('kitchen') 
[Synset('kitchen.n.01')]
>>> wn.synset('kitchen.n.01').part_holonyms() 
[Synset('dwelling.n.01')]
>>> wn.synset('kitchen.n.01').part_meronyms() 
[]
```

From the above code, we see that ‘dwelling’ is a holonym of ‘kitchen’, and so ‘kitchen’ is a meronym (or a part) of ‘dwelling’

The ‘dwelling’ synset contains ‘home’ as one of its synonyms as seen below.

```
>>> wn.synset('dwelling.n.01').lemma_names()
[u'dwelling', u'home', u'domicile', 
u'abode', u'habitation', u'dwelling_house']
```

So we can figure out that the word ‘kitchen’ is a part of one’s ‘home’.

# **Entailments**

An entailment is an implication. For example, looking implies seeing. Buying implies choosing and paying.

We can find entailments as seen below in the code.

```
>>> wn.synset('snore.v.01').entailments()
[Synset('sleep.v.01')]
>>> wn.synset('buy.v.01').entailments()
[Synset('choose.v.01'), Synset('pay.v.01')]
>>> wn.synset('look.v.01').entailments()
[Synset('see.v.01')]
```

We can see that sleeping is an entailment (implication) of snoring.  So in our earlier example when she was ‘snoring loudly’, it can be  inferred that she was asleep.

In this post, we have explored the basics of WordNet relations and  how to integrate WordNet in a Python project. There is still a lot more  that you can do with WordNet like finding the similarity between  synsets, converting a noun to its verb form, and much more. You can read more about using WordNet in Python here – http://www.nltk.org/howto/wordnet.html