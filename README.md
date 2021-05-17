# Research_Response_Machine_Translation

This is a response to professor  <a href="https://yoavartzi.com/"><strong>Yoav Artzi's</strong></a> NLP application prompt.



- Submitted by Maitreyi Chatterjee(mc2259)

# Abstract:
In this paper, I am using attention and a transformer based architecture to detect whether the translation is a human translation or a machine translation. After training my model for 3 epochs with a learning rate of 1e-4 and a batch size of 4, I am able to achieve an F1 score of 0.885( without using BLEU and lexical features) and 0.8333( after using BLEU and lexical features). Currently, most machine translation systems are either static machine translation systems or neural machine translation systems. These systems fail on large sentences and out of vocabulary terms. In this project, we adapt a many-to-many transformer architecture for the binary classification task of predicting a translation as ‘H’ or ‘M’, based on syntactical, lexical and emotional features in the data.

 Although transformers are usually not used for binary classification problems and SVMs are often preferred, I use the T5 text-to-text model  and modify it for our binary classification task because it has been pre-trained on  translation tasks and this leads me to believe that it would perform better in differentiating between machine translation and human translation. This is because the closer the downstream task is to the pre-training, the better is the expected performance. T5  generally performs well on machine translation tasks, so I decided to adapt it to our custom task. Here is the paper for T5: 
 <a href="https://arxiv.org/abs/1910.10683"><strong>Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer</strong></a>

After model selection, our first task is to identify which features are most important for the model to learn the best. In particular the lexical and syntactic structure of the candidate sentence, as well as the emotion index of the target sentence are really significant features to consider. This is because a meaningful sentence should have a consistent emotion strength among different words. Other important features are the presence of out-of-vocabulary words and subject-verb disagreement. However in this project I only use the features below:
1.	Using Noun density by implementing POS tagging in the candidate sentence.
2.	Input-ids of the source, candidate and reference sentences concatenated together
3.	Attention-masks of the source, candidate and reference sentences concatenated together which are generated by the T5 tokenizer.
4.	BLEU score of the translation as provided in the dataset.

The next step is to modify the T5 model to suit our binary classification task. This is done by feeding in the manually generated feature vectors to the T5 model and keeping the labels as either ‘H’ or ‘M’ to signify Human generated or machine generated respectively. Thus we adapt the text generation abilities of T5 as a seq-to-seq model for binary classification .A typical input to our model would be of the form [WE, WE,WE,ND,BLEU] where WE are the word embeddings of the source, candidate and reference, ND is the noun density of the candidate sentence, and BLEU is their BLEU score. A typical output of our model is a string that is either ‘H’ or ‘M’ as per whether the example is Human generated or Machine generated. Thus our features take into account both the lexical capabilities of the candidate as well as the similarity between the candidate and the reference sentence.

The most surprising discovery during this project is that many-to-many architectures like transformers can also be modified for many-to-one tasks like binary classification and generates good results for that. 

# Future goals:
1.	I would experiment with other syntactical features like depth of parse tree, and sentence dependencies to see if performance improves
2.	Hyperparameter tuning and regularisation
3.	Comparing performance with an SVM model as per this paper: paper
