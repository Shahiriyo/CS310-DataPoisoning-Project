# CS310-DataPoisoning-Project

My practical work relating to my dissertation project (CS310) "On Data Poisoning on Machine Learning" at the University of Warwick.


## About this repository

This repository stores example data poisoning attacks on machine learning. The attacks are carried out on multiple datasets to illustrate its efficacy in different contexts.


## Data Poisoning Attack

### Outline

This attack targets models using Stochastic Gradient Descent (SGD) parameter optimisation methods \[1\]. In general, a SGD model can be expressed mathematically as \
$\theta_{N+1} = \theta_1 - \eta\nabla\hat{L}_1(\theta_1) - \eta\nabla\hat{L}_2(\theta_2) - ... - \eta\nabla\hat{L}_N(\theta_N)$ \
where $\theta_N$ are the model parameters on training step $N$ within this current epoch, $\eta$ represents the learning rate used in SGD gradient sampling, $\nabla\hat{L}_i(\theta_i)$ is the first derivative of the average loss of data batch $i$ from the model using parameters $\theta_i$. The equation can be further transformed into the form \
$\theta_{N+1} = \eta\sum_{j=1}^{N}\nabla\hat{L}_j(\theta_1) + \eta^2\sum_{j=1}^{N}\sum_{k < j}\nabla\nabla\hat{L}_j(\theta_1)\nabla\hat{L}_k(\theta_1)+O(N^3\eta^3)$ \
Note that the coefficient to the $\eta^2$ term is not determinstic as it relies on the subset of $k$ data batches that the model has already been trained over in this current epoch. An attacker can exploit this non-determinstic behaviour through adversarial machine learning techniques.


### In this repo

```DataOrderingSGD.ipynb``` is a Jupyter Notebook file demonstration of the attack first on the Abalone Dataset \[2\] (was previously carried out on the Iris Dataset \[3\] until December 2021), using ```scikit-learn```'s ```SGDClassifier``` model \[4\]. There are three models in the experiment:
- a control model which is trained over the data without any adversarial interference to be used later when computing the efficacy of the attack
- a target model which we assume the attacker only maintains query access to (black-box attack); this model is identical to the control model
- a surrogate model which the attacker is using to *estimate* the target model; the configuration of this model is not identical to the others

The experiment trains all three models over one epoch for a first iteration of fitting. The data batches' classifications are then predicted and the loss function is evaluated. The mean average accuracy of the model is tracked over time.


## Notice Regarding Resources
All technologies and concepts in these experiments are being used in compliance with their respective licneses. No personal or sensitive data is being used in any experiments. All experiments are being carried out within a controlled local environment. The purpose for these experiments is not malicious but instead aims to raise awareness about the security of modern machine learning practices.

## Bibliography

1. Ilia Shumailov, Zakhar Shumaylov, Dmitry Kazhdan, Yiren Zhao, Nicolas Papernot, Murat A. Erdogdu, and Ross Anderson. Manipulating sgd with data ordering attacks, 2021.
2. Dheeru Dua and Casey Graff. UCI machine learning repository. http://archive.ics.uci.edu/ml, 2017.
3. R. A. Fisher. The iris dataset. https://www.openml.org/d/61, April 2014.
4. F. Pedregosa, G. Varoquaux, A. Gramfort, V. Michel, B. Thirion, O. Grisel, M. Blon-
del, P. Prettenhofer, R. Weiss, V. Dubourg, J. Vanderplas, A. Passos, D. Cournapeau,
M. Brucher, M. Perrot, and E. Duchesnay. Scikit-learn: Machine learning in Python.
Journal of Machine Learning Research, 12:2825–2830, 2011