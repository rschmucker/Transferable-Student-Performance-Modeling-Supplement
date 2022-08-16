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

For detailed definitions of each individual feature we refer to the Best-LR+ [paper](https://jedm.educationaldatamining.org/index.php/JEDM/article/view/541).

A-Best-LR+: This model builds on the A-Best-LR feature set and adds Best-LR+'s response pattern and smoothed average correctness features. In addition it trains a single set DAS3H time-window, R-PFA and PPE parameters shared for all KCs.


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
