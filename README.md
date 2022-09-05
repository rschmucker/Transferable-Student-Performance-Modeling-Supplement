# TransferableSPM - Appendix

This GitHub repository serves as appendix for the paper *Transferable Student Performance Modeling for Intelligent Tutoring Systems*. The paper studies how transfer learning (TL) can be used to train accurate student
performance models (SPMs) for new courses by leveraging student log data collected from existing courses (Figure 1). We provide detailed descriptions of the individual features and different SPMs which were evaluated in our experiments. We further provide additional experimental results for the inductive transfer experiments. Below is the feature set which yielded the best transfer performance. 

<p align = "center"><img src = "./figures/concept.png" style="width:55%"></p><p align = "center">
<i>Figure 1.</i> Conceptual overview of how transfer learning can leverage  interaction log data from existing courses to train a student model for performance predictions in a new course for which only limited or no interaction log data is available.
</p>

## Feature Set with Best Transfer Performance 

In our experiments we found the following set of features to be most effective for training a model on data from existing courses and using it to make predictions for a new course:

  * single ability parameter shared for all students
  * number of all prior correct responses of student $s$
  * number of all prior incorrect responses of student $s$
  * number of prior correct responses of student $s$ for KC $k$
  * number of prior incorrect responses of student $s$ for KC $k$
  * R-PFA's recency-weighted incorrectness count and success proportion features
  * PPE’s spacing time features 
  * DAS3H’s time window-based count features
  * Response patterns
  * Smoothed average correctness
  * SAINT+'s current lag time and prior response time features
  * learning context one-hot and count features
  * difficulty one-hot and count features

All count features were subjected to scaling function $\phi(x) = \log(1 + x)$. A single set of model parameters is trained for all KCs. At prediction time our model only uses KC count information related to the KCs of the current question. Information about the students attempts on other unrelated KCs does not go into the prediction.

More detailed descriptions of the individual features can be found below.

## SPM Descriptions

Here we provide descriptions of the different SPMs which were evaluated in our experiments. For each SPM we look at its underlying feature set and identify a reduced feature set which induces a course-agnostic SPM that does not require target course data for training. By avoiding course-specific features course-agnostic SPMs can be trained on source data $D_S$ from existing courses and then can be used for any new target course $T$.

### Performance Factor Analysis (PFA) - (Pavlik et al., 2009)

PFA prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}_{s, 1:t}) = \sigma \left( \sum\_{k \in KC(q\_{s, t+1})} \beta_k + \gamma_k c\_{s,k} + \rho_k f\_{s,k} \right) $

PFA variables:
  * $\beta_k$: difficulty parameter for KC $k$
  * $c\_{s,k}$: number of prior correct responses of student $s$ for KC $k$
  * $f\_{s,k}$: number of prior incorrect responses of student $s$ for KC $k$
  * $\gamma_k$ and $\rho_k$: trained learning rate parameters for KC $k$

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
  * $\alpha$: single ability parameter shared for all students
  * $\delta$: single difficulty parameter shared for all questions and KCs
  * W: defines time windows used to summarize student history. $W = \lbrace 1/24, 1, 7, 30, +\infty \rbrace$ in days.
  * $c\_{s,k,w}$: number of prior correct responses of student $s$ for KC $k$ in time window $w$
  * $f\_{s,k,w}$: number of prior attempts of student $s$ for KC $k$ in time window $w$
  * $\phi$: scaling function $\phi(x) = \log(1 + x)$
  * $\theta\_{2w + 1}$ and $\theta\_{k, 2w + 2}$: trained learning rate parameters for time window $w$ shared for all KCs

### Best-LR - (Gervet et al., 2020)

Best-LR prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}\_{s, 1:t}) = \sigma \left( \alpha\_s - \delta\_{q\_{s, t+1}} + \tau_c \phi(c\_s) + \tau_f \phi(f\_s) + \sum\_{k \in KC(q\_{s, t+1})} (\beta\_k + \gamma\_k \phi(c\_{s, k}) + \rho\_k \phi(f\_{s, k})) \right)$

Best-LR variables:
  * $\alpha_{s}$: ability parameter for student $s$
  * $\delta\_{q\_{s, t+1}}$: difficulty parameter for question $q\_{s, t+1}$
  * $c\_s$ number of all prior correct responses of student $s$
  * $f\_s$ number of all prior incorrect responses of student $s$
  * $\beta_k$: difficulty parameter for KC $k$
  * $c\_{s,k}$: number of prior correct responses of student $s$ for KC $k$
  * $f\_{s,k}$: number of prior incorrect responses of student $s$ for KC $k$
  * $\phi$: scaling function $\phi(x) = \log(1 + x)$
  * $\tau\_c$ and $\tau\_f$: trained learning rate parameters
  * $\gamma\_k$ and $\rho\_k$: trained learning rate parameters for KC $k$

A-Best-LR prediction: $p(y_{s, t+1} = 1 \| q_{s, t+1}, \textbf{x}\_{s, 1:t}) = \sigma \left( \alpha + \tau_c \phi(c\_s) + \tau_f \phi(f\_s) + \gamma \phi(c\_{s, k}) + \rho \phi(f\_{s, k})) \right)$

A-Best-LR variables:
  * $\alpha$: single ability parameter shared for all students
  * $c\_s$ number of all prior correct responses of student $s$
  * $f\_s$ number of all prior incorrect responses of student $s$
  * $c\_{s,k}$: number of prior correct responses of student $s$ for KC $k$
  * $f\_{s,k}$: number of prior incorrect responses of student $s$ for KC $k$
  * $\phi$: scaling function $\phi(x) = \log(1 + x)$
  * $\tau\_c$ and $\tau\_f$: trained learning rate parameters
  * $\gamma$ and $\rho$: trained learning rate parameters shared for all KCs

### Best-LR+ - (Schmucker et al., 2022)

Best-LR+: This model builds on the Best-LR feature set and adds the following additional features which all can be computed from question-response logs:
  * R-PFA's recency-weighted incorrectness count and success proportion features
  * PPE’s spacing time features 
  * DAS3H’s time window-based count features
  * Response patterns
  * Smoothed average correctness

For detailed definitions of each individual feature, we refer to the Best-LR+ [paper](https://jedm.educationaldatamining.org/index.php/JEDM/article/view/541).

A-Best-LR+: This model builds on the A-Best-LR feature set and adds Best-LR+'s response pattern and smoothed average correctness features. In addition, it trains a single set DAS3H time-window, R-PFA and PPE parameters shared for all KCs.


## Feature Descriptions

Here we provide detailed descriptions of the individual features which were evaluated in our naive transfer experiments (Section 5.2). For each feature we describe its corresponding feature function and its relevant information in the `ElemMath2021` dataset. Our implementation of these features builds on the public [GitHub repository](https://github.com/rschmucker/Large-Scale-Knowledge-Tracing) by Schmucker et al. (2022).

All of the count features below were subjected to scaling function $\phi(x) = \log(1 + x)$.

| Feature      | Description |
| ----------- | ----------- |
| current lag time      | We measures the time passed between the completion of the previous question until display of the current question (Shin et al., 2021). We provide this current lag time information to the model in two forms: (i) We subject the current lag time to scaling function $\phi$. (ii) We round the current lag time to integer minutes and assign it to one of 150 categories $\lbrace 0, 1, 2, 3, 4, 5, 10, 20, \dots, 1440 \rbrace$. We then encode these categories using a one-hot encoding.           |  
| previous response time   | We measures the time passed between the display of the previous question until response submission to the previous question (Shin et al., 2021). We provide this previous response time information to the model in two forms: (i) We subject the previous response time to scaling function $\phi$. (ii) We round the previous response time to integer seconds and assign it to one of 300 categories $\lbrace 0, 1, 2, 3, \dots, 300 \rbrace$. We then encode these categories using a one-hot encoding.        |
| context one-hot     | `ElemMath2021` records information about the learning context by assigning each learning activity to one of six categories of study modules (e.g., pre-test, post-test, review, …). We use this information to define a one-hot encoding which informs the model about the current learning context a student is placed in.        |
| context count     | `ElemMath2021` records information about the learning context by assigning each learning activity to one of six categories of study modules (e.g., pre-test, post-test, review, …). When student $s$ attempts a question targeting KC $k$ we determine the number of prior correct responses and number of prior overall attempts on questions that target KC $k$ for each study module.           |
| difficulty one-hot     | During content creation, human domain experts assigned each question a difficulty label in $\lbrace 10, 20, \dots,90 \rbrace$. Corresponding to these labels we define difficulty parameters $\delta\_{10}, \delta\_{20}, \dots, \delta\_{90}$ and define a one-hot encoding which informs the model about which difficulty parameter represents which questions.       |
| difficulty count     | During content creation, human domain experts assigned each question a difficulty label in $\lbrace 10, 20, \dots,90 \rbrace$. When student $s$ attempts a question targeting KC $k$ we determine the number of prior correct responses and number of prior overall attempts on questions that target KC $k$ for each difficulty label.         |
| prereq count     | Using the KC prerequisite graph, we determine student s's number of prior correct responses and number of overall attempts on KCs which are prerequiste to the KC of the current question.           |
| postreq count     | Using the KC prerequisite graph, we determine student s's number of prior correct responses and number of overall attempts on KCs which are postrequiste to the KC of the current question.        |
| videos count     | We compute (i) the total number of videos student $s$ has watched before and (ii) the number of videos student $s$ has watched before that target the KC of the current question. |
| readings count     | We compute (i) the total number of reading materials student $s$ has interacted with before and (ii) the number of reading materials student $s$ has interacted with before that target the KC of the current question.         |



## Additional Inductive Transfer Plots

Here we provide additional experimental results from our inductive transfer experiments (Section 5.3). Figure 2 compares the performance of our inductive transfer learning method (I-AugLR) with conventional SPM approaches and S-AugLR trained using only target course data $D_T$ for $T \in \lbrace C6, C7, C8, C9, C40\rbrace$. By tuning a pre-trained A-AugLR model, I-AugLR can mitigate the cold-start problem for all five courses and benefits from small-scale log data. Given as little as data from 10 students, the I-AugLR models consistently outperform standard BKT and PFA models that were trained on logs from thousands of target course students (Table 3, Section 5.1). Among all considered SPMs, I-AugLR yields the most accurate performance prediction up to 25 students for $C7$, up to 100 students for $C6$ and $C8$ and up to 250 students for $C9$ and $C40$. Among the non-transfer learning approaches, Best-LR is most data efficient and yields the best performance predictions when training on up to 500 students.

<p align = "center"><img src = "./figures/course_6_accs.jpg" style="width:50%"><img src = "./figures/course_6_aucs.jpg" style="width:50%"><img src = "./figures/course_7_accs.jpg" style="width:50%"><img src = "./figures/course_7_aucs.jpg" style="width:50%"><img src = "./figures/course_8_accs.jpg" style="width:50%"><img src = "./figures/course_8_aucs.jpg" style="width:50%"><img src = "./figures/course_9_accs.jpg" style="width:50%"><img src = "./figures/course_9_aucs.jpg" style="width:50%"><img src = "./figures/course_40_accs.jpg" style="width:50%"><img src = "./figures/course_40_aucs.jpg" style="width:50%"></p><p align = "center">
<i>Figure 2.</i> Training student performance models (SPMs) with different amounts of student log data (measured in number of students). For each of the five courses we show ACC/AUC metrics
achieved by the learned SPM. The dashed red line indicates the performance of a course-agnostic A-AugLR model that was pre-trained on student data from the other four courses and did not use
any target course data. The dot-dashed red line indicates the performance of our inductive transfer approach (I-AugLR) which
uses target course data to tune the pre-trained A-AugLR model to the target course. The course-specialized S-AugLR model is identical
to I-AugLR but does not leverage a pre-trained A-AugLR model. All results are averaged using a 5-fold cross validation.
</p>
