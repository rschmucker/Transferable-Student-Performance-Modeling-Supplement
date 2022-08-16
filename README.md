# TransferableSPM - Appendix

This GitHub repository serves as appendix for the paper *Transferable Student Performance Modeling for Intelligent Tutoring Systems* which studies how transfer learning (TL) can be used to train accurate student
performance models (SPMs) for a new course by leveraging student log data collected from existing courses (Figure 1). This appendix offers detailed descriptions of the individual features and different SPMs which were evaluated in our experiments. It further provides additional experimental results for the inductive transfer experiments. 


<p align = "center"><img src = "./figures/concept.png" style="width:55%"></p><p align = "center">
<i>Figure 1.</i> Conceptual overview of how transfer learning can leverage  interaction log data from existing courses to train a student model for performance predictions in a new course for which only limited or no interaction log data is available.
</p>


## SPM Descriptions

Here we provide descriptions of the different SPMs which were evaluated in our experiments. For each SPM we look at its underlying feature set and identify a reduced feature set which induces a course-agnosic SPM that does not require target course data for training. By avoiding course-specific features course-agnostic SPMs can be trained on source data $D_S$ from existing courses and then can be used for any new target course $T$.

### Performance Factor Analysis (PFA) - (Pavlik et al., 2009)

PFA prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma \left( \sum\_{k \in KC(q\_{s, t+1})} \beta_k + \gamma_k c\_{s,k} + \rho_k f\_{s,k} \right) $

PFA variables:
  * $\beta_k$: difficulty parameter for KC $k$
  * $c\_{s,k}$: number of prior correct responses of student $s$ for KC $k$
  * $f\_{s,k}$: number of prior incorrect responses of student $s$ for KC $k$
  * $\gamma_k$ and $\rho_k$: trained learning rate parameters 

A-PFA prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma \left( \beta + \gamma c\_{s,k} + \rho f\_{s,k} \right) $

A-PFA variables:
  * $\beta$: single difficulty parameter shared for all KCs
  * $c\_{s,k}$: number of prior correct responses of student $s$ for KC $k$
  * $f\_{s,k}$: number of prior incorrect responses of student $s$ for KC $k$
  * $\gamma$ and $\rho$: trained learning rate parameters shared for all KCs

### Item-Response Theory (IRT) - (Rasch, 1960)

* IRT prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma (\alpha_s - \delta\_{q\_{s, t+1}})$

* IRT variables:
  * $\alpha_{s}$: ability parameter for student $s$
  * $\delta\_{q\_{s, t+1}}$: difficulty parameter for question $q\_{s, t+1}$

* A-IRT prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma (\alpha\_{s,k} - \delta)$

* A-IRT variables:
  * $\delta$: single difficulty parameter shared for all questions
  * $\alpha\_{s,k}$: iteratively fitted student ability parameter which for each KC $k$ is initialized with 0 and which is updated after each student response. 

### DAS3H - (Choffin et al., 2019)

DAS3H prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma \left( \alpha_s - \delta\_{q\_{s, t+1}} + \sum\_{k \in KC(q\_{s, t+1})} \left( \beta_k  + \sum\_{w \in W} \theta\_{k, 2w + 1} \phi(c\_{s,k,w}) - \theta\_{k, 2w + 2} \phi(a\_{s,k,w}) \right) \right)$

DAS3H variables:
  * $\alpha_{s}$: ability parameter for student $s$
  * $\delta\_{q\_{s, t+1}}$: difficulty parameter for question $q\_{s, t+1}$
  * $\beta_k$: difficulty parameter for KC $k$
  * W: defines time windows used to summarize student history. $W = \lbrace 1/24, 1, 7, 30, +\infty \rbrace$ in days.
  * $c\_{s,k,w}$: number of prior correct responses of student $s$ for KC $k$ in time window $w$
  * $f\_{s,k,w}$: number of prior attempts of student $s$ for KC $k$ in time window $w$
  * $\phi$: scaling function $\phi(x) = \log(1 + x)$
  * $\theta\_{k, 2w + 1}$ and $\theta\_{k, 2w + 2}$: trained learning rate parameters for KC $k$ and time window $w$

A-DAS3H prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma \left( \alpha - \delta + \sum\_{w \in W} \theta\_{2w + 1} \phi(c\_{s,k,w}) - \theta\_{2w + 2} \phi(a\_{s,k,w}) \right)$

A-DAS3H variables:
  * $s$: single ability parameter shared for all students
  * $\delta$: single difficulty parameter shared for all questions and KCs
  * W: defines time windows used to summarize student history. $W = \lbrace 1/24, 1, 7, 30, +\infty \rbrace$ in days.
  * $c\_{s,k,w}$: number of prior correct responses of student $s$ for KC $k$ in time window $w$
  * $f\_{s,k,w}$: number of prior attempts of student $s$ for KC $k$ in time window $w$
  * $\phi$: scaling function $\phi(x) = \log(1 + x)$
  * $\theta\_{2w + 1}$ and $\theta\_{k, 2w + 2}$: trained learning rate parameters for time window $w$ shared for all KCs

### Best-LR - (Gervet et al., 2020)

Best-LR prediction:

Best-LR variables:

A-Best-LR prediction:

A-Best-LR variables:

### Best-LR+ - (Schmucker et al., 2022)

Best-LR+ prediction:

Best-LR+ variables:

A-Best-LR+ prediction:

A-Best-LR+ variables:

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
# TransferableSPM - Appendix

This GitHub repository serves as appendix for the paper *Transferable Student Performance Modeling for Intelligent Tutoring Systems* which studies how transfer learning (TL) can be used to train accurate student
performance models (SPMs) for a new course by leveraging student log data collected from existing courses (Figure 1). This appendix offers detailed descriptions of the individual features and different SPMs which were evaluated in our experiments. It further provides additional experimental results for the inductive transfer experiments. 


<p align = "center"><img src = "./figures/concept.png" style="width:55%"></p><p align = "center">
<i>Figure 1.</i> Conceptual overview of how transfer learning can leverage  interaction log data from existing courses to train a student model for performance predictions in a new course for which only limited or no interaction log data is available.
</p>


## SPM Descriptions

Here we provide descriptions of the different SPMs which were evaluated in our experiments. For each SPM we look at its underlying feature set and identify a reduced feature set which induces a course-agnosic SPM that does not require target course data for training. By avoiding course-specific features course-agnostic SPMs can be trained on source data $D_S$ from existing courses and then can be used for any new target course $T$.

### Performance Factor Analysis (PFA) - (Pavlik et al., 2009)

PFA prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma \left( \sum\_{k \in KC(q\_{s, t+1})} \beta_k + \gamma_k c\_{s,k} + \rho_k f\_{s,k} \right) $

PFA variables:
  * $\beta_k$: difficulty parameter for KC $k$
  * $c\_{s,k}$: number of prior correct responses of student $s$ for KC $k$
  * $f\_{s,k}$: number of prior incorrect responses of student $s$ for KC $k$
  * $\gamma_k$ and $\rho_k$: trained learning rate parameters 

A-PFA prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma \left( \beta + \gamma c\_{s,k} + \rho f\_{s,k} \right) $

A-PFA variables:
  * $\beta$: single difficulty parameter shared for all KCs
  * $c\_{s,k}$: number of prior correct responses of student $s$ for KC $k$
  * $f\_{s,k}$: number of prior incorrect responses of student $s$ for KC $k$
  * $\gamma$ and $\rho$: trained learning rate parameters shared for all KCs

### Item-Response Theory (IRT) - (Rasch, 1960)

* IRT prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma (\alpha_s - \delta\_{q\_{s, t+1}})$

* IRT variables:
  * $\delta\_{q\_{s, t+1}}$: difficulty parameter for question $q\_{s, t+1}$
  * $\alpha_{s}$: ability parameter for student $s$

* A-IRT prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma (\alpha\_{s,k} - \delta)$

* A-IRT variables:
  * $\delta$: single difficulty parameter shared for all questions
  * $\alpha\_{s,k}$: iteratively fitted student ability parameter which for each KC $k$ is initialized with 0 and which is updated after each student response. 

### DAS3H - (Choffin et al., 2019)

DAS3H prediction: 

DAS3H variables:

A-DAS3H prediction:

A-DAS3H variables:


### Best-LR - (Gervet et al., 2020)

Best-LR prediction:

Best-LR variables:

A-Best-LR prediction:

A-Best-LR variables:

### Best-LR+ - (Schmucker et al., 2022)

Best-LR+ prediction:

Best-LR+ variables:

A-Best-LR+ prediction:

A-Best-LR+ variables:

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
# TransferableSPM - Appendix

This GitHub repository serves as appendix for the paper *Transferable Student Performance Modeling for Intelligent Tutoring Systems* which studies how transfer learning (TL) can be used to train accurate student
performance models (SPMs) for a new course by leveraging student log data collected from existing courses (Figure 1). This appendix offers detailed descriptions of the individual features and different SPMs which were evaluated in our experiments. It further provides additional experimental results for the inductive transfer experiments. 


<p align = "center"><img src = "./figures/concept.png" style="width:55%"></p><p align = "center">
<i>Figure 1.</i> Conceptual overview of how transfer learning can leverage  interaction log data from existing courses to train a student model for performance predictions in a new course for which only limited or no interaction log data is available.
</p>


## SPM Descriptions

Here we provide descriptions of the different SPMs which were evaluated in our experiments. For each SPM we look at its underlying feature set and identify a reduced feature set which induces a course-agnosic SPM that does not require target course data for training. By avoiding course-specific features course-agnostic SPMs can be trained on source data $D_S$ from existing courses and then can be used for any new target course $T$.

### Performance Factor Analysis (PFA) - (Pavlik et al., 2009)

PFA prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma \left( \sum\_{k \in KC(q\_{s, t+1})} \beta_k + \gamma_k c\_{s,k} + \rho_k f\_{s,k} \right) $

PFA variables:
  * $\beta_k$: difficulty parameter for KC $k$
  * $c\_{s,k}$: number of prior correct responses of student $s$ for KC $k$
  * $f\_{s,k}$: number of prior incorrect responses of student $s$ for KC $k$
  * $\gamma_k$ and $\rho_k$: trained learning rate parameters 

A-PFA prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma \left( \beta + \gamma c\_{s,k} + \rho f\_{s,k} \right) $

A-PFA variables:
  * $\beta$: single difficulty parameter shared for all KCs
  * $c\_{s,k}$: number of prior correct responses of student $s$ for KC $k$
  * $f\_{s,k}$: number of prior incorrect responses of student $s$ for KC $k$
  * $\gamma$ and $\rho$: trained learning rate parameters shared for all KCs

### Item-Response Theory (IRT) - (Rasch, 1960)

* IRT prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma (\alpha_s - \delta\_{q\_{s, t+1}})$

* IRT variables:
  * $\delta\_{q\_{s, t+1}}$: difficulty parameter for question $q\_{s, t+1}$
  * $\alpha_{s}$: ability parameter for student $s$

* A-IRT prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma (\alpha\_{s,k} - \delta)$

* A-IRT variables:
  * $\delta$: single difficulty parameter shared for all questions
  * $\alpha\_{s,k}$: iteratively fitted student ability parameter which for each KC $k$ is initialized with 0 and which is updated after each student response. 

### DAS3H - (Choffin et al., 2019)

DAS3H prediction: 

DAS3H variables:

A-DAS3H prediction:

A-DAS3H variables:


### Best-LR - (Gervet et al., 2020)

Best-LR prediction:

Best-LR variables:

A-Best-LR prediction:

A-Best-LR variables:

### Best-LR+ - (Schmucker et al., 2022)

Best-LR+ prediction:

Best-LR+ variables:

A-Best-LR+ prediction:

A-Best-LR+ variables:

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
