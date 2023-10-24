---
layout: page
title: Naive Bayes Classifier
description: A naive Bayes classifier in pure Python with unigram features for sentiment analysis
importance: 1
category: fun
related_publications: jurafskymartin2023nlp
---

This was my attempt at implementing a naive Bayes model from scratch in Python using pseudocode from chapter 4 of *Speech and Language Processing* by Dan Jurafsky and James H. Martin. This post is simply meant to add context to how the notebook was written, rather than be a standalone note on naive Bayes.

The concept of individual words as features carried over from the previous chapter of the textbook on n-grams. N-grams are a topic that deserves its own note, but simply put they are just sequences of $$n$$ words whose occurrences we'd like to count. The fact that they are a sequence is important because a simplifying assumption of naive Bayes is that a text document can be represented as a *bag of words*, meaning unordered. An n-gram where $$n > 1$$ preserves some of the order for us.

I followed this pseudocode from Jurafsky and Martin to make the model:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/nb-pseudocode" title="naive Bayes pseudocode" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

The resulting notebook is below. I included a list of possible improvements at the end that I plan to implement at some point. I probably won't push every update here, so check out the [repository](https://github.com/njcolvin/naive-Bayes) or [Kaggle post](https://www.kaggle.com/code/njcolvin/naive-bayes-sentiment-analysis) for the latest version.

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/nbclassifier.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/nbclassifier.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
    {% jupyter_notebook jupyter_path %}
{% else %}
    <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}


