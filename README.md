# 🌳 Extracting Operator Trees from Model Embeddings
This repository contains the code and data set for our paper "Extracting Operator Trees from Model Embeddings" (MathNLP 2022). In our paper we show that Transformer Encoder Models pre-trained on mathematical data include information about operator trees of formulas in their contextualized embeddings. We evaluated eigth models trained on math, one model on scientific documents and four baseline models that were not specifically pre-trained on mathematical documents.

# 🗃️ Data Set
Please find the train, dev and test split of our data in the directory `data/mathse` and `data/math`. Both contain txt files with line seperated formulas for each split. In order to use the data for OPT extraction the formulas need to be parsed and converted into the correct format (see below). The mathse data set in based on the Mathematics StackExchange data provided by ARQMath. The math data set is based on the MATH data set.

## 🛠️ Pre-Processing
Our pre-processing script expects a file of line-separated formulas. 
Usage:
```
bash preprocess_formulas.sh <path-to-data>
```
The script reads each formula, parses it using our OPT parser and converts it in the conll format. 

## ⚙️ Generate Embeddings for each Model
After we have the raw data, we need to get the contextualized embeddings that we want to investigate. 
Usage:
```
python3 convert_raw_to_bert_hf.py <train.conll>
```
Depending on the model, we prepared different scripts: 
- `convert_raw_to_bert_hf.py` for BERT-based models
- `convert_raw_to_albert.py` for ALBERT-based models
- `convert_raw_to_roberta.py` for RoBERTa-based models

The scripts were adapted to Huggingface Transformers from the original script of Hewitt and Manning.

# 🏋️ Training
All hyperparameters are contained in the dist.yaml file. An example can be found in the `example` directory. In contrast to the original code, we added a parameter for the tokenizer. 
We have also create a generator script for a new yaml file for each layer: `generate_yaml_files.sh`.
Usage:
```
bash run_experiment.sh
```

## 🔎 Results
The results will be in the `results` directory as specified in the `.yaml` file. There you can investigate samples parses and check the scores for both metrics.

# Adding your own Model
In order to add your own model you might need to:
- [ ] Add a `convert_to_raw.py` script for your model (or use adjust an existing one)
- [ ] add your model as the model parameter in your yaml file (change albert/bert/roberta to your model)
- [ ] Check that the correct tokenizer is called during training in `structural_probes/data.py` in the method `generate_subword_embeddings_from_hdf5()`
