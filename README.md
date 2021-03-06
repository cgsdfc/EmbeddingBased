# Embedding-based Metrics
Embedding-based metrics are ways to evaluate semantic similarity between two sentences
based on some pre-trained word vectors. These word vectors are trained to represent distributed
meanings of words using models like *Skip-grams*, *CBOW (Continuous Bag of Words)* or *Glove (Global Vector)*.
They can also be applied to evaluation of dialogue systems.
This repository contains code to compute these embedding-based metrics:
- Average Score, where the average of all word vectors composing a sentence is taken into account as
a summary of a sentence and consine similarity of these averages is used as the final score.

    
    Average(sent) = sum(sent) / norm(sent)
    AverageScore(source, target) = cosine_similarity(Average(source), Average(target))

- Greedy Matching, as its name implies, it tries to find the maximum cosine similarity in a word-to-word basic, where
each word of the source sentence is matched against all words of the target sentence to find the maximum consine similarity.
It then sums up these maximum consine similarity for all words in a source sentence, normalized by the length of the source,
following the pseudo code:


    SumMaxCosine(source, target) = sum(max(cosine_similarity(s, t) for s in source) for t in target) / len(source)
    
The same procedure is performed on the (target, source) pair and the final result is the average of both, namely:


    GreedyMacthing(source, target) = (SumMaxCosine(source, target) + SumMaxCosine(target, source)) / 2

- Vector Extrema, use a different way to generate a sentence representation from its constitute word embeddings.
For each dimension of the word vectors, the extrema value is selected as below:


    Extrema(i, embeddings) = max(abs(x) for x in embeddings[i])

The sentence vector is then made up of these extrema values from all dimensions:


    SentVec[i] = Extrema(i, embeddings)
    
Finally the score is obtained by taking cosine similarity of two `SentVec`.


# Dependencies
- Python 3.6
- gensim 3.4.0

You can install these deps with conda:

    conda create --name <env> --file ./package-list.txt
    
# Usage

    python embedding_metrics.py path_to_ground_truth.txt path_to_predictions.txt path_to_embeddings.bin

The script assumes one example per line (e.g. one dialogue or one sentence per line), 
where line n in `'path_to_ground_truth.txt'` matches that of line n in `'path_to_predictions.txt'`.

# Recommended Word Embedding
The word embedding you are recommended to use is the *Word2Vec* vectors trained on the *Google News Corpus*.
This is also recommended by the original repository. To download this pre-trained embedding easily, here are some useful links:
- [word2vec Google News model](https://github.com/mmihaltz/word2vec-GoogleNews-vectors.git) is a mirror of the google archive on Github,
you need *git lfs* to be able to clone it.
- [Google Code Archive](https://code.google.com/archive/p/word2vec/)
- [Google Drive](https://drive.google.com/file/d/0B7XkCwpI5KDYNlNUTTlSS21pQmM/edit)


# Where did the code come from?
The main script `embedding_metrices.py` is adapted from [hed-dlg-truncated](https://github.com/julianser/hed-dlg-truncated).
Thanks for their great script!

# References
[1] Section 3.1 of How NOT To Evaluate Your Dialogue System: An Empirical Study of
Unsupervised Evaluation Metrics for Dialogue Response Generation
