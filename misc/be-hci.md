# BE and HCI

## Data Visualization

### Get attribute names
- Gets names of all of the columns, along with what type of data they are (float, integer, categorical, etc.)
- BE should also keep track of what each of the categories mean (?)

Example:
HCI to BE:
```
{
    "dataset": "sample-data"
}
```

BE to HCI:
```
{
    attributes: [
        {
            "label": "Clinic",
            "type": "categorical",
            "description": "Description text" (maybe?)
        },
        {
            "label": "mean.width.panCK",
            "type": "float",
            "description": "Description text"
        },
        ...
    ]
}        
```

### Get data (with filters)
- I don't think the user needs to be able to select any specific rows...?

Example:
HCI to BE:
```
{
    "dataset": "sample-data",
    "filters": [
        "Clinic=1",
        "mean.width.panCK>=1000",
        "mean.width.panCK<=2000",
        ...
    ]
}
```

BE to HCI:
```
{
    "data": [
        {
            "column": "Sex",
            "values": [0, 0, 1, 0, ...]
        },
        {
            "column": "mean.width.panCK",
            "data": [1292.234, 1765.456, ...]
        },
        ...
    ]
}
```

## Image Annotation

### Send annotation 

Example 
HCI to BE:
```
{
   "imgfile": "img_0000.png",
   "x-axis": 245,
   "y-axis": 795,
   "text": "This is the text of the annotation"
}
```

BE to HCI:
```
{
    "id": 0001 (maybe?)
}
```

Two options here:

- Backend assigns an id to each annotation and sends it back to the front-end
- Disallow annotations at the same location of the same image
	- requires backend to look for duplicates each time a new annotation is submitted
	- duplicates are rejected and front end is notified of this

### Edit annotation

Example 
HCI to BE:
```
PUT /annotations
{
   "id": 0001, (maybe)
   "imgfile": "img_0000.png",
   "x-axis": 245,
   "y-axis": 795,
   "text": "Updated annotation text"
}
```

### Get all annotations for image

Example 
HCI to BE:
```
{
    "imgfile": "img_0000.png"
}
```
 
BE to HCI:
```
{ 
    "annotations": [
        {
            "id": 0001,
            "x-axis": 245,
            "y-axis": 795,
            "text": "Annotation text"
        },
        ...
    ]
}
```

### Get part of an image:

Example 
HCI to BE:
```
{
    "imgfile": "img_0000.png"
    "x-start": 120,
    "y-start": 450,
    "x-end": 1045,
    "y-end": 1620
}
```


## Login

OAuth