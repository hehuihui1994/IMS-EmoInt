# IMS-EmoInt
This repository contains the IMS System submission for the WASSA-2017 Shared Task on Emotion Intensity (EmoInt)

<p align="center">
<a href="url"><img src="https://github.com/koepermn/IMS-EmoInt/blob/master/logo.png" align="center" height="250" width="250" ></a>
</p>


## -- Requirements:

1] You need to have **weka** installed
http://www.cs.waikato.ac.nz/ml/weka/

2] We use **Keras** with **Thenao** backend for our Regression feature

https://keras.io/
http://deeplearning.net/software/theano/

3] We need **Lemma** and **Part-of-Speech** tags, these were obtained using the **TweetNLP**
Download: http://www.cs.cmu.edu/~ark/TweetNLP/ (we used ark-tweet-nlp-0.3.2) 

4] Parts of the features were created by the Baseline System (**Affective Tweets**)
https://github.com/felipebravom/AffectiveTweets

5] The **extended ratings** (created by us) are available for download here (65MB):
http://www.ims.uni-stuttgart.de/forschung/ressourcen/experiment-daten/IMS_emoint_norms.tar.gz

## Example Usage:

Assuming you want to use IMS to predict intensity prediction for a given input file.
We provide a full pipeline for the example in the folder:
`run_through_example/anger_example/anger_plain.txt`
Note that you need to ajdust multiple paths with respect to the required tools (TwitterNLP, weka, ...) according to your local machine.
Then you need to to the following steps <br />
######  1) Parse the input file <br />
  - using a plain text file you can run `scripts/run_LemmaPOS.sh`
  - This will transform a one sentence/tweet per line format into a one word per line format
    
###### 2) Run the CNN-LSTM Regression model <br />
  - The scripts trains one model per emotion for the given test file
  - By default we rely on the official training data for training
  - Note that we provide here only a subset of our vectors 
  - _keras_regression/twitter_sgns_subset.txt.gz_ covers the shared task vocabulary
  - vectors are in word2vec format (can be gz, txt or binary)
  - The output of the regression is a single file per training emotion, for further processing we create one file containing    all four predictions by using `paste anger.txt. fear.txt. joy.txt sadness.txt  > afjs.txt`
     
###### 3) Create an Inputfile for weka <br/>
  - this can be done using the `scripts/createarff.jar` (fulll code `scripts/createarff_java/`)
  - This step combines the previous steps
  - Run `java -jar createarff.jar <parsedFile> <inputfile w.Ratings> ratings/Ratings.csv.gz <CNN-LSTM output>`
###### 4) Add Baseline features from Affective tweets <br/>
 - Using the GUI or the command line we add the features from AffectiveTweets
 - We apply default settings of `TweetToSentiStrengthFeatureVector` & `TweetToLexiconFeatureVector`
   
###### 5) Run wekas Random Forest <br />
 - `scripts/run_RandomForest_eval-only.sh` or `scripts/run_RandomForest_save-predictions.sh`
 - To apply the script link to the folder from the training (arff) files (`official_train_arff/`)
   A full and more detailed example how to use our system can be seen in the `run_full.sh` script. 

Contact: maximilian.koeper AT gmail.com
