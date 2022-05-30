# NBME_patient_notes
The project represents an automated method to map clinical concepts from an exam rubric (e.g., “diminished appetite”) to various ways in which these concepts are expressed in clinical patient notes written by medical students (e.g., “eating less,” “clothes fit looser”).

## Main information
- The model from this project has been created for the ["NBME - Score Clinical Patient Notes"](https://www.kaggle.com/competitions/nbme-score-clinical-patient-notes/overview/description) competition on kaggle
- Main file is created in Ipython notebook format (.ipynb)
- The model is transformer Deberta-v3-large
- This project achives 0.88633 [micro-averaged F1](https://scikit-learn.org/stable/modules/model_evaluation.html#from-binary-to-multiclass-and-multilabel) score
- Main notebook is based on the [baseline](https://www.kaggle.com/code/yasufuminakama/nbme-deberta-base-baseline-train) granted by Y.Nakama

## What's going on here?
The main idea is to tokenize patient note and symptom as input data and create labels for each word in patient note according to locations of correct answers.  All base work to create and train such model represented in ```nbme_deberta_v3_large-train_M1.ipynb``` file. 
There are a lot of BERTs to choose, I could compare just several of them, that looks more suitable according to the data they has been training on: Deberta-base, Med-BERT, Roberta. Deberta-base showed best results on cross validation. So then we compared Deberta-base and Deberta-v3-large, as expected large model performed better.


## What else we can do?
- We can check if there any typos in the phrases from locations (Yes, there are). Fix them to improve models right uderstanding of the symptoms. The idea is represented in ```fix_typos_create_extra_dataset.ipynb``` file. (*This idea improved the result.*)
- Another idea is to change special abbreviations to the full form to let the model know more about meaning. ```abbr_dict.txt``` contains dictionary for the abbreviations. (*This idea didn't improve the result.*)
- According to the fact that we have not much examples for every symptom, we can generate more train data from unlabeled part of dataset. The model will not learn anymore new information, but smoothing due to more examples can affect for the better result. (*The idea wasn't tested properly due to the very slow hardware, but the idea inspired by [this](https://lilianweng.github.io/posts/2021-12-05-semi-supervised/) article and looks like it might work very good*)
- Play with hyperparameters such as step size, number of epochs, number of folds. (*The idea hasn't been tested due to the slow hardware*)

## Main files and directories
### Folders
**Data** - contains all datasets from the competition<br />
**dicts** - contains dictionaries for typos processing<br />
**prediction_mistakes** - contains undocumented raw files to make semi-labeled data from unlabeled<br />
**typos** - cotains extra dataset with the lines where typos has been fixed
### Files
**nbme_deberta_v3_large-train_M1.ipynb** - main file that contains datasets importing and model training<br />
**fix_typos_create_extra_dataset.ipynb** - typos of the annotation processed in this file
