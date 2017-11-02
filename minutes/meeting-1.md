
# JH Group Meeting Minutes
## Aim: To organize protocols and disscuss Ideas in terms of the practical

#### AGENDA:
BE walkthrough
HCI walkthrough
ML walkthrough
API Decisions
SCRUM

#### Communication: 
There is a slack channel: there is a slack group
There is also a FB group: CS class of 2019
Do not use emails
Specs:
Alex on Backend
Dasha on ML
Daisy on HCI
BACKEND WALKTHROUGH:
1) Data has been given out. Gives a rough data but does not include “dead” data.
Point of order:
Will Every group be ding front end or back end?
“are prople confusing HCI and frontend”
Two different ways of doing
HCI deals with all front end stuff
Or its all systematized 
Alex: Hci will do CSS and template sort of system and the back end does the “stuff.
Current plan: Backend manages the logic and HCI manages front.
It is decided that there will be a login page

Decided: The website can be rendered on any decide back backend and ML is not involved in this.
Backend are responsible for data validation.
To be discussed…
Protocols
Point of order: 
Who is Server and who is client between ML and Backend.
Does everything run through backend
I.e. HCI <-> Backend <-> ML


Point of order: Comment on ML spec
The user should be able to choose to train the ML with random shit. E,g, deaths and pets?

A decision for backend: Super users have rights to change the rights of lower users. Is this done through the website or manual editing at the backend?
To do. Find libraries that can help.
Will medics with access be computer literate enough to edit rights not on HCI
Should changing permission be same as login 
Decide: backend should have an api for editing rights and HCI have access to it.
Discussion: Workspace does not mean anything. Simply a place to put files 
We don’t know how high the res will be for data.
Where is the data store going to be and ensuring validity?
Simply there must be files and folders
“Understanding” bullet point: trying to say that the backend should be responsible for ALL file type conversion.

DECIDED: Lossless compression not Lossy.
Lookup TIFF, Leica and Zeiss to see which is better for what circumstances.
Question for client: How important is it clarity.
Alex: Leica and Zeiss are data from microscopes etc. They have metadata. They have annotation such as labels within the image. Polygons are a thing. Rectangles are a thing
Tiffs also have heavy metadata support.
One option is to take all the meta data and convert to json and hold the metadata and image separate.
DECIDED: JSON to be the main format for communication.


Should users be allowed to delete stuff or should there be admin?
Many user admin rankings?
Admins can delete files?!

Deleting: How should be done?
What should the warning be when someone takes place?
If you delete a file. What warnings should be given when a errors will be caused by deleting files.
DECIDED: Cascading deletes not to be used.
 Decide on data&/file management once we have more data.

Because of scrum basic stuff is expected to be done by feb. and advanced will be done after. Supervisor has the right to give you the advanced if they think your group is far enough.

##### HCI:
1) Portability disused om backend
2) Visualization should be able to visualize both raw data as well as result data from ML.
DECIDED: so long as we can import data with metadata and export data adding on meta data all shold be good. To be discussed in protocol.
3) Should be able to filter the data the is given
a. Should we be able to do it on graphs?
4) Is normalization of data not backend?
Get a chunk of data. HCI should decided how it should be represented. Pie chart, histogram etc.
Should HCI have control on classification of data?

##### ML:
Going through points.
Exportable common file formats to be done by backend. There should be communication between ML and backend when it comes to data management to and from ML.

Point of order: How does data columns get selected in the front end 
To be decided by HCI groups.
We will have a Q&A with the client about data.
Oggie was not planning to give us data?

Point of order: How generic is the project supposed to be?
In theory the program should be able to grow past just biomedical data.
So the program should be pretty agnostic with the data it needs.
The user should be able to tell the program if the data is continuous or discreet.
Who will process the data to categories it ML or backend? To be disused

#### PROTOCAL & IMPORTANT DECISIONS:
JSON is currently to be main data format for communication.
HCI: simply requests data through apis and visualizes it. Event driven.
ML on a separate server or as a lib within backend?
Authentication token
Does HCI communicate with ML. 
HCI create a system where based on the data needed. Either request it from backend or from ML.
DECIDED: Backend should not have to differentiate between hci and ML interaction. Auth token to be passed around.
Decision: languages of ML and backend to be separate. 
Decision: Should HCI have there own light server for light data processing?
DECIDED:  If files are requested e.g. a csv file, the actual file will be given back and not be parsed into a json file and sent back.

ALEX to send info about backed libs.

#### Communications DECISIONS:
LOGIN: HCI <-> BACKEND. 
AUTHENTICATION: TOKEN BASED. PASSES AROUND.
We have a website with a login: Who build the logic behind the website?
Is HCI building the entire frontend. Including JS for logining in
DECIDED: HCI TO MAKE ALL FRONTEND!!!!
API TO BE DECIDED ON IN SLACK.
MANAGEMENT OF USERS & FILES:
HCI are to make 

#### CONCLUSION:
We are to organize weekly/bimonthly meetings with representatives of each teams to discuss decision. To be organized by Stacy and Tom. 
SLACK groups have been created for ML-Backend, Backend-HCI and HCI-ML decisions. 


