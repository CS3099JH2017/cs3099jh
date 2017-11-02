# ML and BE

### Summary - Decisions:

* BE stores any model that ML decides must be stored.
* BE are responsible for the converting into different file types, ML are expected to work with only one file type
* ML are to train a model defined by a user; executed on an ML server independent of the BE
As the user may want data in different file formats, BE should convert data into different file types, while ML only works with CSVs
* ***Extension*** - When making an API call, a JSON attribute will be used to specify file types; if the provided type is not available the server defaults to a CSV (Tom)

### Models

Machine Learning groups are expected to generate a model when specified columns to train on. These trained models will be stored on a database (regardless of their size) on a dedicated ML server (storing of the models in databases in BE?). Models stored on the same server as the ML processing provides no problems - issues only arise when storing models on a different server to the processing.

HCI is expected to request a model (based on the columns that are specified); if the given model exists in the database, rather than recreating and retraining the model, the stored one will be utilised. The sole purpose of storing the models is to prevent wasting time training a model which has been previously trained.

Incremental training may be used on stored models to allow for a model to provide a greater abstraction upon given data as well as allowing for its generalisation to larger data sets (Ryan).
An API call can be made to allow ML groups to check whether a model exists in the database (Daphne) . If the dedicated ML server is maintained solely by ML this is not needed, if this server is managed by BE, they must determine whether a reference to the model is sent to ML. **BE stores any model as determined by ML**. It is expected that the model is an abstraction on the data retrieved from HCI.

### Files

ML can request BE to store any data as a binary object if the need arises. Subsequently an API method will be needed to allow for binary objects to be stored/retrieved to/from the backend.

**_Discussion_** - In terms of I/O file formats between BE and ML in the API the possible options were:

* Default to always CSV
* Allow ML to specify the file formats that they can handle
* ML are not expected to differentiate between different file formats
* Allow BE to handle CSV and XLS
* Expect BE to convert from standardised formats to alternate formats

When saving data, an additional parameter can be added to specify the ideal file type to save given data.

ML can create plots on the performance of a given model, which may be sent to BE to be converted to “common file formats”.

BE are to deal with file persistence - how long they remain alive on the server (Ryan?).

### Miscellaneous

The results generated from any model will be the only values that ML send to HCI, however HCI can determine whether they intend for the results to be stored. The meaning of the data - what it actually represents (i.e. an output of 0.1338 indicates the patient has HIV but they also have a dog) - is to be determined by ML (Alex).
BE can send a reference to a generated result to HCI, who determine whether they want the result to be stored.

**A separate meeting will take place to determine the names of JSON attributes. (Alex)**


# ML and HCI

#### Summary - Decisions:

* **HCI needs to do some work**
* HCI will be asking ML for data - the reverse is deemed pointless (Daphne)
* ML will have their own dedicated server for processing - to limit the amount of client-side processing
* We’re ignoring Oggie’s spec (who knew?) and K-Folds are not to be done by ML
* ML do not have to generate plots?
* ***Extension*** - ML and HCI must agree on the labelling of images for image segmentation and classification
Extension - A socket between the ML server and HCI will be necessary to allow for communication
* ***Extension*** - HCI have a panic api call to make ML generate any plot if HCI cannot do so - any ML team not wishing to implement such a feature may also return an error (Daphne)

### ***Discussion*** - Plots and Graphs

* HCI could provide a way for ML to produce their own plots - Is this seen as ML doing HCI’s job?
* HCI to draw trees
* ML should be generating graphs as per the given spec in the case HCI cannot generate the graph - ML can create plots to be sent to HCI (much faster)
* ML should never do plots?
* HCI should specify how data should be plotted - or this can be done by the user - a sensible assumption is that this should be done by ML e.g. data that should be a pie chart determined by ML can be interpreted as a histogram by HCI (obviously).

### API Specifics

HCI may have access to commands which would grant them the ability to ask ML if a model is trained (boolean) and allow HCI to request the termination of the training of a model (Daphne).

ML can tell HCI when a given model has finished training - HCI should tell ML when it has been too long training the model.

The results generated from any model will be the only values that ML send to HCI, however HCI can determine whether they intend for the results to be stored. The meaning of the data - what it actually represents (e.g. an output of 0.1338 indicates the patient has HIV but they also have a dog) - is to be determined by ML (Alex).

#### Miscellaneous

* ML are expected to tell HCI which tests it is possible to execute on specified data?
* ML should be able to create a model based on the columns that HCI specify
* Should the termination of training a model be determined by HCI or ML?
* Return a reference to the results (to HCI) which are stored in BE instead of sending results to BE.
* ML algorithms all have different input parameters HCI specifies a field for the parameters which can be used for different algorithms.
HCI may need a constant connection to the proposed dedicated ML server so ML can send a message to HCI whenever required.
* Some basic computation should be done by HCI - prevent ML from performing miniscule tasks such as averages
* ML must validate their analysis - this does not include the model?
* Metadata could be attached to the generated results depicting ML’s validation on their result
* HCI should be able to allow for the zooming and panning of images

