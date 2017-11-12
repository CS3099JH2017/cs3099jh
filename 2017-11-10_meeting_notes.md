# CS3099 meeting: 2017-11-10
## HCI
- The user should be able to create annotations.
  - E.g. squares around an area of the image.
  - Ideally, free-hand or smart/intelligent shapes.
  - The annotations should be accessible by all users with access to the
  data/picture.
  - An annotation should be editable by multiple users.
  - There should be a way to distinguish who wrote which comment in the
  annotation.
  - There should be a 'history' of when the annotation/comment was made
  (probably list them chronologically).
  - Annotations should be referencable.
  - Possibly a discussion section.
- Measure sections of images in terms of pixel density. Can then be convertible
to something more useful based upon scale?
- Mouse scroll-wheel zoom.  
- Toggle grey scale, general image colour manipulation.  
- The user should be able to record a video of the data they are visualising, e.g.
a video of a 3D-plots auto-rotating.
- Multiple plots to be visible simultaneously and it should be clear how the data 
points on the different plots relate, e.g. colouring in corresponding parts of 
the plots from a user-selected region of the plot.
- Relate graphs and pick cut points.
- Wavelengths are stored in the metadata of some images. These should be
toggleable, e.g. like different layers.  
- Plot of the density of the wavelengths in the image potentially desired.  
- The user should be able to overlay/merge images of the same thing but taken at
different wavelengths (again, like different layers).
- Have a look at [Miner3D](https://secure.miner3d.com/).

## ML
- Smart highlighting of pictures.
- Survival time/proportion of group.
  - And plots of these.
  - Splitting data into two groups: low risk and high risk.
- Survival/recurrence/prediction stats.
- Combination of parameters.
  - Want to see which parameters were weighted how.
- Kaplan-Meier estimates.
- Data normalisation is very important:
  - User does not want to have to modify the data if it does not fit with the ML
  model they picked. This should be done by the program.
- False-negative/-positive statistics desired.
- Image analysis to evaluate image with machine learning.
- Look at both raw data and analyse the images themselves.
- Spatial statistics.
  - E.g. hihghlighting which geographical areas have most cancer cases.

## Backend
- Ideally, unlimited data upload of any data type and format.

## HCI/Backend
- URL-generation linking to the view the user requesting the URL had at the time
of the request.
  - The URL should be shareable with and viewable by users who do not have the
  software.
- In general, views/software needs to be easily shareable.
- Retrieve specific X/Y-coordinates from the image, e.g. by clicking on it.
- Hover over and/or click on a specific data point in a plot should show the
detailed/actual data of that entry/point.
- Selection of subset of requested data, witout then having to reload the full
data set when subset is no longer required or the user wants to include more
from the original data set.
- Create users.
- Create groups.
  - Add/remove users from group.
  - E.g. all users in a group can view the data the group has access to.

## HCI/ML
- Control what kind of validation is used for the models.
- Control the value of _k_ in the k-fold validation.
- Potentially a progress bar for viewing how much is left of e.g. training the
requested model.
- Decision tree ouptut and visualisation.

## ML/Backend
- If data is invalid/incompatible, potentially highlight which bits and why...
