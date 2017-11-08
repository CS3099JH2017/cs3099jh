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
receiver requests for analysis from the frontend implemented by any Data Visualization group and 
either retrieve the necessary data from the backend, perform the model training and return the 
result or, if the model has previously been trained, retrieve the model and use that to generate 
the result.

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

K-fold cross validation is to be done on the HCI side instead of ML. The plots for the other 
statistical performance measures, namely:
  - Receiver-operating curves
  - Specificity-precision curves
  - Plots of feature importance
could be done by either HCI or, upon request, by ML.

### API Specifics

The basic idea of the part of the API that involves the ML side is the following: HCI sends a 
request to train a model on particular attributes of a particular dataset (columns in a CSV 
file), provides the ML server with the authentication key required to access the dataset, and 
requests a result to be predicted from a set of variables matching the sample data.

### File Format

The basic file format that the ML server has to work with is CSV (apart from images). The 
backend is responsible for converting the data to CSV on request. However, the file format for 
the data should be specified as part of the request for the data and as an extension the ML 
server can support other file formats.

### Model Storage

The ML server can store any model it trains on the backend server. If a request is made for a 
model that has already been computed, it may be reused to save time. The result of the 
prediction requested by the HCI server may also be stored in the backend server, if it were to 
request that.

### Plot Generation

Plot generation should be done by both HCI and, upon request, by ML. 

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
