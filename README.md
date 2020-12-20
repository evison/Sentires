# Sentires: A Toolkit for Phrase-level Sentiment Analysis

This is a package for phrase-level sentiment analysis, using the package, we can extract **feature|opinion|sentiment** triplets from free-text corpus, e.g., 

context-aware 


To help run the toolkit more smoothly, we have included some of the known issues and the corresponding solutions, hope they will help to save time when using the toolkit.

```
1. When you have generated the sentiment lexicon file, each row of the file will look like this: 

**[1] quality | good 5 3**

Here, “quality” is the feature word, “good” is the opinion word, [1] means that the sentiment of the feature-opinion pair is positive. What may be confusing is the meaning of 5 and 3. For a feature-opinion pair, users can use it in the “normal order” to describe his/her opinion, and they can also use it in inverse order. For example, the user can say that “the quality is good” (normal order), or can say that “it has a good quality” (inverse order), so 5 and 3 means the number of times that the pair is used in normal order and inverse order in your corpus, respectively. As a result, 5+3=8 is the total number of times that the pair was mentioned in your corpus.
```

2. After you generated the lexicon file, you may want to know how to determine which pairs are mentioned in each sentence. You do not have to do this manually, instead, you may want to run the “profile” step to generate the results. In the profile task file (e.g., the "2014.nus.profile.task” file), one configure is missing in this shared package, please add the following line into this file.

profile.indicatorfile = ${path.profile}/2014.nus.utf.indicator

In this way the package will run without bug. For the Chinese version, you need to modified the corresponding task file in a similar way.


The toolkits builds a lexicon based on 6 steps (corresponding to 6 tasks), the 6 steps should be executed one by one in the correct order:

1. “pre” task: it conducts some pre-processing over your input data, such as removing sentences that are too short or grammatically incorrect. It takes the “.raw” file as input and outputs a “.select” file.

2. “pos” task: it conducts part of speech tagging based on Stanford tagger. It takes a “.select” file as input and outputs a “.seg” file.

3. “validate” task: it refines the part-of-speech tagging results, to generate a POS tagging file better suited for feature-opinion extraction. It takes the above “.seg” file as input, and generate another “.seg” file. To distinguish them, you may take a look at the .seg file. The Stanford POS taggers have at least 2 characters (e.g., /NN, /RB, /VBD, etc.), while in the refined .seg file each tagger only has one character (e.g., /N, /V, /P, /X, etc.).

4. “lexicon” task: it extract the feature-opinion pairs and their sentiments. It takes the refined .seg file as input, and outputs the final “.lexicon" file. Each row in the lexicon file is a “feature-opinion-sentiment” triplet.

5. (Optional) “eval” task: it conducts some basic evaluation for the quality of the generated lexicon. However, you would need to provide some (small scale) human labeled pairs to run this task.

6. (Optional) “profile” tasks: it detects which feature-opinion pair(s) are included in each sentence in the input file. This seems a trivial task but it’s actually not very easy because we have to consider the context and grammar rules. It takes a .lexicon file and the original corpus file as input, and outputs two “.profile” files.

You don’t have to train anything domain specific, only need to make sure that your input corpus is domain specific (i.e., better not mix movie reviews with other reviews such as restaurant). The toolkit still works if the input corpus is a mixture of reviews from many domains, but it may influence the quality of the extracted feature-opinion pairs, because the underlying algorithms are based on statistical machine learning anyway.

A final note is that the parameters in the “lexicon” task will influence the trade-off between the quantity and quality of the extracted feature-opinion pairs. We provide two pre-set parameters: preset/relax.threshold and preset/strict.threshold. In the task/2014.nus.lexicon.task file, they are referenced at line 65. The default selection is relax.threshold, which extracts more (but some may be wrong) pairs. The strict.threshold selection will extract less (but more confident) pairs. For normal users using the relax.threshold is already sufficient because we have carefully tuned the parameters in many domains, so it should give us the best trade-off. However, advanced users can change the parameter settings to get the results they need.

We appreciate if you can cite one or more of the following papers or other related papers that is helpful to your research:

Y. Zhang et al. "Explicit factor models for explainable recommendation based on phrase-level sentiment analysis.” SIGIR 2014.
Yongfeng Zhang and Xu Chen. "Explainable Recommendation: A Survey and New Perspectives.” Foundations and Trends in Information Retrieval. 2020.
Y. Zhang et al. “Towards Conversational Search and Recommendation: System Ask, User Respond”. CIKM 2018.
Y. Xian et al. "Reinforcement knowledge graph reasoning for explainable recommendation.” SIGIR 2019.
Y. Xian et al. "CAFE: Coarse-to-fine neural symbolic reasoning for explainable recommendation.” CIKM 2020.

Hope this is helpful, and wish you success in your research.
