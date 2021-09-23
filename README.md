<p align="center">
  <h2 align="center">Monolingual Mapping</h2>
  <h3 align="center">Javanese and Sundanese Dictionary</h3>
</p>

## Model Description
First, monolingual mapping learns word vector representation from each monolingual corpus. Then, it learns transformation matrix that performs monolingual mapping from the source languages word embedding to the target languages. In this work, we used MUSE library to implement this approach. You can find the detail information about MUSE [here](https://github.com/facebookresearch/MUSE).

## Architecture Design
<p align="center">
    <img src="contents/monolingual mapping fix.png" alt="Model Architecture" height="auto">
</p>

1. The Javanese and Sundanese corpus preprocessed. The preprocessing process consist of deleting English words and sentence windowing. 
2. Then, for each corpus, monolingual word embedding generated using pre-trained model FastText, Word2Vec, and feature extraction on pre-trained multilingual BERT model. 
3. After that, the Javanese word embedding mapped to Sundanese embedding space to creates cross-lingual representation of Javanese words toward Sundanese. The monolingual mapping used supervised and unsupervised MUSE. 
4. Last, from the cross-lingual representation, pair translation retrieved using nearest-neighbor and cross-domain local scaling method.
## Experiment Result

<p align="center">
    <img src="contents/experiment-result.PNG" alt="Model Architecture" height="auto">
</p>

- The model with supervised or unsupervised MUSE that used pre-trained FastText and feature extraction on pre-trained Multilingual BERT achieved higher performance than the model used Word2Vec.
- All model with supervised MUSE achieved higher performances than the unsupervised MUSE model.
- Model Supervised MUSE – FastText achieved highest performance. The model achieved f1-score NN value 0.4230 and f1-score CSLS value 0.4468.

## How to use?
### 1. Data Preparation
You can collect [Javanese](https://dumps.wikimedia.org/suwiki/latest/suwiki-latest-pages-articles.xml.bz2) and [Sundanese](https://dumps.wikimedia.org/jvwiki/latest/jvwiki-latest-pages-articles.xml.bz2) corpus from Wikipedia dumps articles file. Then, preprocess the data using following command.
- Sentence Windowing
```
python3 corpus_windowing.py <corpus_file> <output_file>
```
- Remove English Words
```
python3 data_cleaning.py <embedding_file> <english_words_file> <output_file>
``` 
### 2. Merge: Length-ratio Shuffle
Create a pseudo-bilingual corpus using length-ratio shuffle method.
```
python3 merge.py <input_file1> <input_file2> <output_file>
```
### 3. Word Embedding
Create word representasion from a pseudo-bilingual corpus. We use Word2Vec, FastText, and feature extraction on pre-trained Multilingual BERT model.
- Word2Vec
```
python3 word2vec.py <corpus> <output_vector_file>
```
- FastText
```
python3 fasttext.py <corpus> <output_vector_file>
```
- Multilingual BERT
```
python3 mBERT.py <corpus> <output_vector_file>
```
### 4. Translation Retrieval and Evaluation
Generate pair translation and evaluate the translation result.
```
python3 eval_pseudo_bilingual.py <emb_file> <dict_file> <file_wrong> <file_corect>
```
