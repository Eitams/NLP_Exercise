# Text analysis - Bacteria VS Bactriophage

The following three part notebook is part of an NLP course exercise aiming to analyse data extracted from wikipedia based on text analysis parameters.  
NLP_1_extract_data - data extraction from wikipedia    
NLP_1p2_EDA - Exploratory data analysis using NLP methods  
NLP_1p3_Word_embedding - Exploration of classes semantic space and compersion of word embedding models  

## Intro

Bacteria are small single-celled microorganisms, which were among the first life forms to appear on earth and are present in most of its habitats. Though some bacteria may be pathogenic to human, most bacteria are not, and many bacteria occupy different sites in the human body and perform essential roles in body maintenance. In fact, the human body is estimated to contain about 10 times more bacterial cells than human cells.

The natural enemies of bacteria in the evolutionary race are bacteriophages. A bacteriophage (often abbreviated to “phage”) is a virus that infects and replicates within bacteria.

## Summary 

The data contains the summary information of various Bacteria species extracted from the "List of clinically important bacteria" page, and information about different Bacteriophages from the "Category: Bacteriophages" page in Wikipedia.

Due to the two distinct categorical groups, we explored the data with a classification approach in mind.

The initial combined dataframe contained 180 records and 3 features (name, description and type).
The main target group column "Type" is unbalanced. When modeling the data it should be rebalanced first.

After dropping one unwanted row and performing feature engineering, a data frame containing 179 records and 34 features (31 new features) was left:

One language column (constant- can be left out)
Description_clean - clean version of description column:
Lower case, remove punctuation and stopwords, lemmatization (used to create other features out of it).
8 length features - 5 created out of "Description" column and 3 out of "Name" (including, word/character/sentence count).
Polarity column - Calculation of the description column's overall polarity
parsed - tokenized form of "Description_clean" column (used to create other features out of it).
entity_tags and entity_types -used to performed BOW count of entities
17 BOW columns of entities.

The effect/correlation of the new features (representing the text properties) to the type column (Bacteria and Bacteriophage categories) was then investigated:

Length features - Type
We can see a difference in distribution between the length properties of Bacteria and Bacteriophage descriptions. Longer descriptions, in respect to word, character and sentence count, tend to be Bacteria descriptions.
Bacteria names tend to have two words in them, in rare occasion one and never in our list more than two. Bacteriophage names are more versatile in the word length on the other hand (1,2 or 3 words).

Polarity vs Type
No significant difference in polarity between the descriptions of the two categories was noticable. Both categories' polarity is evenly distributed around the 0.

Word count analysis Description vs Type
The words "human" and "animal" which are the direct hosts of the bacteria are frequently used in the description of bacteria and not frequent when describing bactriophages. The word Bacteria (not like bacteriophage) is in frequently used in both categories (representing the name of one group and the direct host of the other).
Among the most repetitive words we can find:

    For Bacteria description - bacteria, species, infection, human, genus, cause and disease.
    For Bacteriophage description - phage, bacteriophage, bacteria, virus, DNA.

With the help of the scatter text analysis, the most frequent representative words of each category (bacteria and bacteriophage) can be distinguished. Those words can help to classify description text to the right category:

    Bacteria - pathogen, rodshape, agar, grampositive ...
    Bacteriophage - phage, bacteriophage, virus, viral ...

![image](https://user-images.githubusercontent.com/62335786/176383725-55676a38-5863-4ef7-9f84-0808f939c959.png)


N-gram analysis vs Type
The word "Bacteria" is the only noticable word which overlaps between the two groups' top gram words. A fact which implies that N-gram analysis could be a good approach to classify the data. A higher number of counts for unigram words than bigram and triplegram is visible.
The unigram words "known", "also" and "used" are not really domain specific, therefore their removal should be considered when using N-gram analysis for classification in the model.

Named-Entity Recognition (NER)
Initial use of the model using the spacy model did't yield in satisfying results,therefore a different model, more specific to healthcare data or refitting of the current model would be neccessary to receive more reliable results.

Nevertheless, to stay within the scope of this work, we explored the data with this model and focused on the entities which we assumed had more relible results by this NER model.
As assumed larger number of EVENTS entities can be seen within the bacteria class (no EVENT entity was dicovered in our dataset in the bacteriophage class), the entity type is rare though also for the bacteriophage class.
On a similiar note, a larger number of LOC entities can be found within the bacteria class than the bacteriophage class (found in almost ~25% of the descriptions). Those entities probably represent the location of interaction between bacteria and human beings.
A smaller difference can be found in the GPE and FAC entities.

## Word embedding summary
**Bacteria**  

![image](https://user-images.githubusercontent.com/62335786/176384996-72851841-71fa-4866-97c8-387b56e7761c.png)


Only a small difference is visible between model 1 and model 2 results although we changed the window size. One possible option for those results is our use of the clean tokenized column as our input. By using this column which has no stop words and only lemmatized words as an input we probably removed most of the structure of the sentence effects on the model (which other wise should be visible in model1 with the smaller window size).

While the results for models 1 and 2 are specific for our domain, the results of model 3 relate to the relationship of the words from other domain as well (the model was trained with a more diverse data).
Most of the results in model 3 are different then those of model 1 and 2
By looking at the results of model 3 we can see that the model was trained with the non lemmatized form of the tokens.

**Bacteriophage**  

![image](https://user-images.githubusercontent.com/62335786/176385232-d8bfa930-022b-4ffc-bc0d-a959b8cf0ddc.png)


Key insights Bacteriophage:

Bigger differances between model1 and model2 can be seen within the Bacteriophage group (in relation to the Bacteria group). This is potentially due to the smaller sample size.

In general, I would have expected that the results of the words phage and bacteriophage in model 3 would be similar to the results in model 1 and 2, this is due to thier specificity. Nevertheless, it seems as if this is not the case, and the only common words are just the lemmatized form of the words themself.
One assumption is that while our data cover the full reange of Bacteriphages evenly, with no biases (one summary for each type of bacteriophage), the data that the glove model is trained on is on one hand wider but on the other hand unbalanced. In this case "Popular" bacteriophages and thier use cases would probably have bigger presence in the data. (e.g., use of phages in the field of HIV-1)




