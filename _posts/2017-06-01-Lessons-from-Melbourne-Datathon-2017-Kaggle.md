---
layout: post
title: Lessons from Melbourne Datathon 2017 Kaggle
---

My first Kaggle competiton was challenging, fun and addicting! Once it was all said and done, I'd finished at 4th place on the [private leaderboard](https://inclass.kaggle.com/c/dsm2017/leaderboard). In this blog post, I share the most important lessons I learned through the 2017 Melbourne Datathon Kaggle competiton. It was a predictive challenge, where the task involved classifying whether a given patient would get diabetes in the future based on the patient's medical history.

Unfortunately, I can't go into the exact details of the data as all competitors had to sign a Non-Disclosure Agreement. However, I will talk about machine learning and data science from a general perspective, which I think is much more useful and interesting to read, anyway.

# 1. Data is king
The first mistake I made was to gloss over the feature engineering. I was too excited to put the data (any data!) through some machine learning algorithms, just to see some numbers and feel the joy of 'progress'. In this competiton, manipulating the raw data in domain-specific and insightful ways was far more important than optimising an algorithm. I spent days cross-validating neural networks, gradient boosted trees, support vector machines (SVMs), etc. with little improvement to my public leaderboard score. ML algorithms can only go as far as the data you give it; they aren't as magical as we might think! The real accuracy gains occurred immediately after engineering just a few insightful features that captured (part of) the essense of the prediction task.

# 2. XGBoost is also king
My first foray into machine learning was deep learning, and so I'd never ventured far into more traditional machine learning like decision tree ensembles and support vector machines. After reading a couple Kaggle posts from illustrious Grandmaster and Master Kagglers and noting the omnipresence of `XGBoost`--a fast, multi-threadable implementation of *gradient boosted trees*--I decided to give it a try.

It turns out that gradient boosting is incredible for structured data (which was our case). The algorithm iteratively adds estimators to fix the so-called 'residual error' of its previous compadres. More on this later in section 4.

# 3. Development environment. Make it good.
For the project, I used `pandas` (an excellent Python package for manipulating and querying data tables). This also meant that I had to do it all locally with `csv` files. Next time I will definitely throw everything into a database either locally or on an AWS EBS, depending on my bank account balance and the size of the dataset. While I didn't make the effort to put everything into a database, I did (towards the end of the competition) leverage cloud computing on some large AWS EC2 instances to relieve the stress on my local CPU.

# 4. Ensembles: Bagging, boosting, and stacking
Prior to this Kaggle competition, I was used to working with a single convolutional neural network that churned out decent results. This time, I wanted to push myself, as well as the performance of my predictions, by learning to use **ensemble techniques**, which is a broad name for combining multiple instances and/or types of algorithms via averaging or voting ([read the Kaggle wiki](https://www.kaggle.com/wiki/Ensembling)).

- **Bagging**: this meta-algorithm derives its name from 'Bootstrap Aggregation'. Bagging involves combining the predictions of multiple instances of an algorithm. Each instance in the 'bag' is trained on a subset of the data. You can also choose to train each instance on a subset of the features. My best submission was a bagged meta-classifier that combined the predictions of 250 gradient boosted trees.
- **Stacking**: a stacked model is a multi-level model. You start with models that produce output that is in turn fed as input features into a higher level of the model. For example, I trained a neural network, XGBoost and SGDClassifier on the input features (level 1); these models produced predictions, which were then fed into a logistic regression model (level 2), which is supposed to learn the best way to combine the predictions of the three vastly different algorithms. This idea didn't improve my score.
- **Boosting**: technically, boosting is a technique that bands lots of *weak learners* together to produce a *strong learner*. That statement probably makes no sense at all. Weak learners are models that do slightly better than chance performance (i.e. a model that can guess if a coin to be heads or tails with 51% accuracy). Boosting pulls in another weak learner to help fix the error, and this is repeated until we have an group of weak learners that do quite well together, and we call this ensemble a *strong learner*.

# Wrap up
The 2017 Melbourne Datathon was a really fun experience. It has bolstered my passion for data science, and I am now actively looking for machine learning or data science gigs.

I definitely plan on competing in more Kaggle challenges in the (very) near future. Home to the perfect mix of competition and collaboration, Kaggle is the best platform for learning data science.
