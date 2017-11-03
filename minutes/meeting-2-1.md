# ML and BE
For machine run data through model and store the model in the backend train the data on given columns

User requests a model and if the model exists in the backend the model can exist for ML to access the model again.

API for blobs which can be stored in BE and ML can request binary data.

The representation of what the data actually represents can be determined by ML 

The storing of models is used to return to the user a model if stored in the BE instead of retraining a model.

Incremental training? Training on pre-existing model with new data.

API call to check whether a given model exists in the BE.

**BE stores any model determined by ML.**

Results of the model will be all that will be sent to HCI from ML, FE determine whether the result will be saved

BE can send a reference to a given result to the FE, who determine whether the result will be stored.

BE should determine whether the model is sent as a reference to ML.

Model should be an abstraction of the data .

In terms of I/O file formats - **default to always CSV** / server specify the file formats that they can handle / ML cannot distinguish between different file formats / BE can handle CSV and XLS / Converting from standardised formats to alternate formats

User may want data in different file formats, BE converts into different file types, ML works with only one file type

Additional parameter in the API call to specify file format that ML would request the data to be stored as.

ML creates standard plots which can be exportable to “common file formats” - the performance of the models exported to common file formats, BE performs teh exporting

Storing of the models in databses in BE? 

Models stored on the same server as the ML processing provides no problems - issue when storing model on differrent server.

Models stored on a databases, regardless of their size

ML training a model given by a user, executed on an ML server independant of the BE

BE deal with file persistence - how long they remain alive on the server

JSON attribute to specify file types extension - if the provided type is not available the server defaults to a CSV

Meeting to determine the names of JSON attributes

# ML and HCI
HCI asking ML for data - reverse is useless

ML tell HCI which tests it is possible to execute 

ML creates model based on what HCI specify

Command for HCI to ask if the model is trained (boolean) and one where HCI asks to stop the training of the model

Return a reference to the results which are stored in BE instead of sending results to BE

ML algorithms all have different input parameters HCI specifies a field for the parameters which can be used for different algorithms

**_Silence_**

ML can tell HCI when a given model has finished training - HCI should tell ML when it has been too long training the model
HCI may need a constant connection to the ML server so ML can send a message to HCI

**Extension** for a websocket connection between HCI and the ML server 

K-Folds **should** be done by ML as determined by HCI spec ( NOPE IGNORE OGGIE’s  SPEC)

HCI needs to do some work

Some basic computation should be done by HCI - prevent ML from performing miniscule tasks
HCI specify how some data should be plotted - or this can be done by the user - a sensible assumption should be done by ML  e.g. this should be a pie chart determined by ML
ML must validate their analysis - this does not include the model.
Metadata could be attached to the generated results depicting ML’s validation on their result
Extension ML and HCI should agree on image labelling format for image classification

**ML have their own dedicated server**

## Plots
HCI provide a way for ML to produce their own plots - ML doing HCI’s job?

HCI to draw trees

HCI should be able to zoom and pan images

ML should be generating graphs as per the given spec in the case HCI cannot generate the graph - ML can create plots to be sent to HCI (much faster)

*Extension HCI have a panic api to make ML generate any plot if HCI cannot do so - implement an error if ML team does not want to generate plots*

ML should never do plots 


