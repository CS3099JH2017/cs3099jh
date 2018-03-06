# HCI
- **drop-down menus are good**
- **k-means clustering heat-map**
    - (look it up)
- names for annotations
- hide/show annotations
- draw shapes for annotations, e.g. squares, circles...
- rotation of graphs (mainly relevant in 3D graphs)
- brightness + contrast control
- RGB channel control
- colour dots in plots based on categories, e.g. colour all 'death 1' red
- a 'help' button explaining what a setting does
- clicking on dots highlights the other values of that row in that plot
- documentation or a user-guide/how-to would be really nice to have
- potentially support mouse-hover showing annotations (and toggling this feature)
- differences between images for certain features
- user should know if corner of a plot/graph is (0, 0) or a different point
- change data point shape, e.g. cross, dot, circle, etc...
- look into Principal Component Analysis (PCA)
    - often generates two new parameters based on multiple given parameters
    - used for data reduction
    - specify number of Principal Components to generate


# ML
- **time estimate of jobs before starting**
- **confusion matrices**
- data is not guaranteed to have 'time until event' => Cox's regression might not be possible. Signal this
- Cox's regression:
    - specify time to event **OR**
    - specify start+end and extrapolate from that
- optionally try out all algorithms on data and from their evaluation suggest an algorithm to use
    - prior to this, warn that it may take a long time to get the evaluations
- supervised annotation suggestion, e.g. based on all previous things marked "liver" you might want to mark this bit of the new image "liver"
- data sets can be imbalanced:
    - e.g. a lot more patients dead than alive
    - change the bias so that there might be a lower accuracy in predicting death, but at least not everyone is predicted as dead


# BE
- export graphs to various formats, e.g. **TIFF**, **SVG**, pdf, png, jpg, etc...
- superadmin + local admins
- [QuPath](https://qupath.github.io/) is free, open-source, and supports CZI->PNG conversion
- **do not** need to be able to create new CZI files (and this is actually forbidden by Zeiss)
- original CZI **must** never be modified, all changes, annotations, etc. should be done on a copy

# HCI & BE
- **Shareable links**
- **No institutional limits**, e.g. only people from the University of St Andrews can view this project.
- row selection w. ranges of values, e.g. "select rows 1-50 where values are < 1573"
- 360° auto-spin of 3D-plots saved as GIF or similar

# BE & ML
- [ZenLite](https://www.zeiss.com/microscopy/int/products/microscope-software/zen-lite.html) is free and can be used to get smaller, sub-parts of CZI files

# HCI & BE & ML
- n-times k-fold should be supported, e.g. 5 x 10-fold cross validation
- supply both train and test data sets
- supply a large dataset and specify proportion to be used for testing
