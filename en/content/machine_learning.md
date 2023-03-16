# Machine learning module

In the previous module, we introduced how to use ASTK for sequence and epigenetic feature extraction and visualization analysis. 
In the this module, **ASTK** offers three common machine learning algorithms(SVM, RF, KNN) to perform alternative splicing mechanism studies. 
Currently, **ASTK** provides three subcommands for hpyper-parameter optimization, model evaluation and feature importance scoring.


## feature extraction

The follow is code to extract sequence feature for next machine learning model.

```bash
## filter high PSI or low PSI AS event
astk pf -i result1/fb_e11_based_tpm1/psi/fb_e11_16_SE_c2.psi \
    -minq 0.75 -o result1/fb_e11_based_tpm1/psi/fb_16_SE_high.psi
astk pf -i result1/fb_e11_based_tpm1/psi/fb_e11_16_SE_c2.psi \
    -maxq 0.25 -o result1/fb_e11_based_tpm1/psi/fb_16_SE_low.psi

## extract features
astk sss -e result/fb_e11_based_tpm1/psi/fb_16_SE_high.psi \
    -od result/fb_e11_based_tpm1/seqf/e16/sss_fb_16_SE_high \
    -fi GRCm38.primary_assembly.genome.fa \
    -app SUPPA2 -alt 
astk sss -e result/fb_e11_based_tpm1/psi/fb_16_SE_low.psi \
    -od result/fb_e11_based_tpm1/seqf/e16/sss_fb_16_SE_low \
    -fi GRCm38.primary_assembly.genome.fa \
    -app SUPPA2 -alt

astk gcc -e result/fb_e11_based_tpm1/psi/fb_16_SE_high.psi \
    -od result/fb_e11_based_tpm1/seqf/e16/gcc_fb_16_SE_high \
    -fi GRCm38.primary_assembly.genome.fa \
    -app SUPPA2 -p 4  -bs 150 -alt 
astk gcc -e result/fb_e11_based_tpm1/psi/fb_16_SE_low.psi \
    -od result/fb_e11_based_tpm1/seqf/e16/gcc_fb_16_SE_low \
    -fi GRCm38.primary_assembly.genome.fa \
    -app SUPPA2 -p 4  -bs 150  -alt

## merge feature files
astk merge -i result/fb_e11_based_tpm1/seqf/e16/sss_fb_16_SE_high/splice_scores.csv \
    result/fb_e11_based_tpm1/seqf/e16/gcc_fb_16_SE_high/gcc.csv \
    -o result/fb_e11_based_tpm1/seqf/e16/seqf_fb_16_SE_high.alt.csv \
    -gl ss gcc -axis 1  
astk merge -i result/fb_e11_based_tpm1/seqf/e16/sss_fb_16_SE_low/splice_scores.csv \
    result/fb_e11_based_tpm1/seqf/e16/gcc_fb_16_SE_low/gcc.csv -gl ss gcc  \
    -o result/fb_e11_based_tpm1/seqf/e16/seqf_fb_16_SE_low.alt.csv -axis 1  

```

## hpyper-parameter optimization

The subcommand **hpo** uses the grid search method for hyperparameter optimization of machine learning algorithms.

common parameters::

* -i: feature files
* -clf: classifier choices
* -cv: cross-validation fold
* -p: processor number

SVM classifier:

* -C: regularization parameter, format: [start stop number] 
* -g: classifier choices, format: [start stop number] 
* -k: kernel function type [linear|poly|rbf|sigmoid]

Random Forest classifier parameters:

* --n_estimators: regularization parameter, format: [start stop number] 
* --criterion: the criterion to measure the quality of a split: [gini|entropy|log_loss]
* --max_features: the number of features to consider when looking for the best split [sqrt|log2] 
* --max_depth: the maximum depth of the tree, format: [start stop number] 

K-nearest neighbors classifier parameters:

* --n_neighbors: number of neighbors to use, format: [start stop number] 
* --weights:  weight function used in prediction: [uniform|distance] 
* --leaf_size: Leaf size passed to BallTree or KDTree, format: [start stop number] 
* --metric:  metric to use for distance computation: [minkowski|cityblock|euclidean|manhattan]


code:

```bash
astk hpo -clf RF -i result/fb_e11_based_tpm1/seqf/e${i}/seqf_fb_${i}_${et}_{high,low}.alt.csv \
    --n_estimators 50 200 20 --max_depth 4 10 6 -p 40

## command running output 
0.7188355523066268
criterion: entropy
max_depth: 6
max_features: sqrt
n_estimators: 121    
```


## model evaluation

**eval** sub-command could perform model evaluation using extracted feature files. The value of **eval** argument is range, whereas **hpo**'s arguments is a specific value.

The arguments setting is similar to **hpo**:

common parameters::

* -i: feature files
* -clf: classifier choices
* -cv: cross-validation fold
* -p: processor number

SVM classifier:

* -C: regularization parameter
* -g: classifier choices
* -k: kernel function type [linear|poly|rbf|sigmoid]

Random Forest classifier parameters:

* --n_estimators: regularization parameter
* --criterion: the criterion to measure the quality of a split: [gini|entropy|log_loss]
* --max_features: the number of features to consider when looking for the best split [sqrt|log2] 
* --max_depth: the maximum depth of the tree  

K-nearest neighbors classifier parameters:

* --n_neighbors: number of neighbors to use 
* --weights:  weight function used in prediction: [uniform|distance] 
* --leaf_size: Leaf size passed to BallTree or KDTree 
* --metric:  metric to use for distance computation: [minkowski|cityblock|euclidean|manhattan]


code

```bash

astk eval -clf RF  -i result1/fb_e11_based_tpm1/seqf/e16/seqf_fb_16_SE_{high,low}.alt.csv \
        -cv 5 -p 2 --criterion entropy --max_depth 7 --max_features sqrt --n_estimators 168 \
        -o result1/fb_e11_based_tpm1/eval/16/seqf_fb_16_SE_b150.alt.RF.txt
```
The **eval** command output is a ROC curve plot and detailed cross-validation results.

ROC curve plot:
<img src='static/img/seqf_fb_16_SE_b150.alt.RF.png' alt="seqf_fb_16_SE_b150.alt.RF.png"></img>

cross-validation results:

```text
                        0                         
0   339  185
1   164  360

      tp   fn   fp   tn   recall  precision  f1-score  
  0   339  185  164  360   0.65     0.67       0.66    
  1   360  164  185  339   0.69     0.66       0.67    
acc                                            0.67
mcc                                            0.33
-------------------------------------------------------
                        1                         
0   332  192
1   168  355

      tp   fn   fp   tn   recall  precision  f1-score  
  0   332  192  168  355   0.63     0.66       0.65    
  1   355  168  192  332   0.68     0.65       0.66    
acc                                            0.66
mcc                                            0.31
-------------------------------------------------------
                        2                         
0   358  166
1   193  330

      tp   fn   fp   tn   recall  precision  f1-score  
  0   358  166  193  330   0.68     0.65       0.67    
  1   330  193  166  358   0.63     0.67       0.65    
acc                                            0.66
mcc                                            0.31
-------------------------------------------------------
                        3                         
0   336  187
1   192  332

      tp   fn   fp   tn   recall  precision  f1-score  
  0   336  187  192  332   0.64     0.64       0.64    
  1   332  192  187  336   0.63     0.64       0.64    
acc                                            0.64
mcc                                            0.28
-------------------------------------------------------
                        4                         
0   356  167
1   164  360

      tp   fn   fp   tn   recall  precision  f1-score  
  0   356  167  164  360   0.68     0.68       0.68    
  1   360  164  167  356   0.69     0.68       0.69    
acc                                            0.68
mcc                                            0.37
-------------------------------------------------------
```
