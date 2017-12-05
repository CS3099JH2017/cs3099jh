# CS3099 - Basic Specification
## 'Machine Learning and Data Analysis'

### Overview

The participants of CS3099 have been tasked with designing and creating a data hosting and 
analysis system for the University's biomedical department. The task has been divided into 3 'sections':

 1. Backend
 2. Machine Learning and Data Analysis
 3. Visualisation and Human-Computer Interaction

Several groups have been assigned to each section, and the best product from each section will 
be used in combination as the final system.
## Ognjen Arandjelovic (Oggie)\*

### Machine Learning and Data Analysis

Every group doing ML is to create a server with a RESTful API (over HTTPS). The server is to 
receiver requests for analysis from the back end implemented by any Back End group and 
on request either perform the model training and return the a model, or predict a result
based on a model passed in by the back end. 

### Analysis Algorithms

Depending on the type of input and request, the analysis techniques should be able to provide 
discrete results (eg presence of cancer tumor) or continuous results (eg estimated time until 
death). The following techniques for data analysis should be implemeneted/used by the ML server:
  - Naive Bayes
  - Support Vector Machines
  - Random Forests
  - Cox's regression
  - Kaplan-Meier plots

### Statistical Performance

The plots for the statistical performance measures, namely:
  - K-fold cross validation
  - Receiver-operating curves
  - Specificity-precision curves
  - Plots of feature importance
will be done by ML and stored/returned as data points.
It will be up to HCI to render the graphs from the data points.

### Plot Generation

Plot generation should be done by HCI. 

### Additional Points of Interest

- The specifications for the other two groups give extra context for the project as a whole.
- There will be opportunities for further context and clarification regarding the project as a whole: contact with the client is possible and encouraged. The client is invested in the outcome of the project and is available for assistance.
- Extensions on this specification, including more advanced requirements, can be requested after this initial specificatoin is satisfied.
- You may find it useful, as a means of inspiration, to have a look at the following applications which were developed in the same application realm but do not share or fully meet our client’s needs:
   - https://qupath.github.io/ and https://www.biorxiv.org/content/early/2017/01/12/099796
   - http://cellprofiler.org/cp-analyst/
   - http://www.tmanavigator.org/
   - https://spotfire.tibco.com/

\*Revised by JH Project Paticipants
