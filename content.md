#From XML to Name Entities

## Subtitile

Exploring Name Entity Recognition in Repertorium Germanicum I

## Introduction

What is Repertorium Germanicum I, what do I want to do with it

## Research Questions

## The XML Dataset

## Corpus Exploration

Basic descriptive data and data exploration

## NER Experiments

Three models are selected to represent 3 different training contexts: general Latin NER, medieval documentary languages, and modern multilingual NER

1. LatinCy: 这个是最常用的based on spaCy的拉丁语模型。可以用于做NER任务，识别出来的命名体包括：人名，地名，机构名

2. Multilingual Medieval RoBERTa：这个模型在大约8，000条medieval chaterts written in medieval Latin, Old French, and Old Spanish上fine-tuned过。在时间和领域上都更接近RG1

3. XLM-RoBERTa: 这是一个用现代多语言fined-tuned过的模型，优势在于，RG1里可能存在很多德国人名，城市名，这种情况下，经过多语言训练的模型可能更加robust



## Model Performance

## Error Analysis

## Conclusion and Limitations


Project Spec

1. The interest 
    I'm curious about how the historical data was modelled into this database. And also if the existing NER tools can identify named entities, especially the mixture of Latin and German entities, historical spelling.

2. The answerable question
    Which model would perfom the best and what kinds of named entities will cause most errors 

3. Hypothesis: what do you expect and why
    I expect the multilingual model will have the best performance, because the texts were maily written in Latin but contain a lot of German cities and names. 

4. Dataset: What data, from where, and how much
    https://rg-online.dhi-roma.it/denqRG/index.htm

5. Method
    Parse XML, extract and clean the text, try different NER tools, and compare their results 

6. The result: What will show it
    The result will be a clear comparison between different NER tools and also some manual inspection about which entity tpyes and which features are causing systematic errors

7. One real limitation: what could undercut it
    Mixed language. I don't think any of the NER tools were pre-trained on this kind of text. So I expect none of the NER tools will have good results. Maybe from this project, I can choose the best one for future fine-tuning. But then the question is, is it really worth it to train a model? Or maybe doing this manually is better. 

