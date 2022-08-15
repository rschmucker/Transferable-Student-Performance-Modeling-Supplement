# TransferableSPM - Appendix

This GitHub repository serves as appendix for the paper *Transferable Student Performance Modeling for Intelligent Tutoring Systems* which studies how transfer learning (TL) can be used to train accurate student
performance models (SPMs) for a new course by leveraging student log data collected from existing courses (Figure 1). This appendix offers detailed descriptions of the individual features and different SPMs which were evaluated in our experiments. It further provides additional experimental results for the inductive transfer experiments. 


<p align = "center"><img src = "./figures/concept.png" style="width:55%"></p><p align = "center">
<i>Figure 1.</i> Conceptual overview of how transfer learning can leverage  interaction log data from existing courses to train a student model for performance predictions in a new course for which only limited or no interaction log data is available.
</p>


## SPM Descriptions

| IRT      | Rasch (1960) |
| ----------- | ----------- |
| Name | Item-Response Theory |
| Prediction      | $p(y_{s, t+1} = 1 \,|\, q_{s, t+1}, \bm{x}_{s, 1:t}) = \sigma \left( w \trans \Phi(q_{s, t+1}, \bm{x}_{s,1:t}) \right)$       |
| Features   | Text        |
| A-IRT | Computes

| IRT      | Rasch (1960) |
| ----------- | ----------- |
| Name | Item-Response Theory |
| Prediction      | $$p(y_{s, t+1} = 1 \,|\, q_{s, t+1}, \bm{x}_{s, 1:t}) = \sigma \left( w \trans \Phi(q_{s, t+1}, \bm{x}_{s,1:t}) \right)$$       |
| Features   | Text        |
| A-IRT | Computes

* IRT (RASCH)
* PFA
* DAS3H
* Best-LR
* Best-LR+


### BASIC FEATURES - START WITH SPM DESCRIPTIONS

* student ability
* question difficulty
* KC difficulty
* total/KC count
* DAS3H
* PPE
* R-PFA
* Response Pattern
* Smoothed Average Correct


## Feature Descriptions

Here we provide detailed descriptions of the individual features which were evaluated in our naive transfer experiments (Section 5.2). For each feature we describe its corresponding feature function and its relevant information in the `ElemMath2021` dataset. Our implementation of these features builds on the public [GitHub repository](https://github.com/rschmucker/Large-Scale-Knowledge-Tracing) by Schmucker et al. (2022).

START WITH LIST OF FEATURES


### ADDITIONAL FEATURES

* current lag time
* previous response time
* context one-hot
* context count
* difficulty one-hot
* difficulty count
* prereq count
* postreq count
* videos count
* readings count


## Additional Inductive Transfer Plots

Here we provide additional experimental results from our inductive transfer experiments (Section 5.3). Figure 2 compares the performance of our inductive transfer learning method (I-AugLR) with conventional SPM approaches and S-AugLR trained using only target course data $D_T$ for $T \in \lbrace C6, C7, C8, C9, C40\rbrace$. By tuning a pre-trained A-AugLR model, I-AugLR can mitigate the cold-start problem for all five courses and benefits from small-scale log data. Given as little as data from 10 students, the I-AugLR models consistently outperform standard BKT and PFA models that were trained on logs from thousands of target course students (Table 3, Section 5.1). Among all considered SPMs, I-AugLR yields the most accurate performance prediction up to 25 students for $C7$, up to 100 students for $C6$ and $C8$ and up to 250 students for $C9$ and $C40$. Among the non-transfer learning approaches, Best-LR is most data efficient and yields the best performance predictions when training on up to 500 students.

<p align = "center"><img src = "./figures/course_6_accs.jpg" style="width:50%"><img src = "./figures/course_6_aucs.jpg" style="width:50%"><img src = "./figures/course_7_accs.jpg" style="width:50%"><img src = "./figures/course_7_aucs.jpg" style="width:50%"><img src = "./figures/course_8_accs.jpg" style="width:50%"><img src = "./figures/course_8_aucs.jpg" style="width:50%"><img src = "./figures/course_9_accs.jpg" style="width:50%"><img src = "./figures/course_9_aucs.jpg" style="width:50%"><img src = "./figures/course_40_accs.jpg" style="width:50%"><img src = "./figures/course_40_aucs.jpg" style="width:50%"></p><p align = "center">
<i>Figure 2.</i> Training student performance models (SPMs) with different amounts of student log data (measured in number of students). For each of the five courses we show ACC/AUC metrics
achieved by the learned SPM. The dashed red line indicates the performance of a course-agnostic A-AugLR model that was pre-trained on student data from the other four courses and did not use
any target course data. The dot-dashed red line indicates the performance of our inductive transfer approach (I-AugLR) which
uses target course data to tune the pre-trained A-AugLR model to the target course. The course-specialized S-AugLR model is identical
to I-AugLR but does not leverage a pre-trained A-AugLR model. All results are averaged using a 5-fold cross validation.
</p>
