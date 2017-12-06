# THIS IS AN INITIAL **DRAFT** FOR A DATA REQUEST PROTOCOL
## Beginning of URL
`https://example.cs.med.ac.uk/files`

## Requests
Assuming this is preceded by something like the URL above, a request would be
of the form:  
`/files/project/directory/images/image::interface_name`  
where  
- a request is of a 'path-like' form,
- `project` would be the project id,
- `?` token separates the 'path' from the 'interface',
- `interface_name` describes the data request and optionally? format.

## Interfaces
An interface would:
- be preceded by a `::` or similar, non-path token,
- start with the data type,
- then have a number of 'parameters', separated by slashes, probably including
some sort of keyword arguments.

Examples:
- `::binary/start/len`
- `::tabular/name&age&ttd/23-45/format=csv`  
  describes tabular data, taken from the 'name', 'age', and 'ttd' (time to death)
columns, entries 23 to 45, in csv format.
- `::tabular/<cols>/<rows>/format=csv`
- `::zoom_image/lod/`  

# 2017-12-06 - HCI-BE
1. Metadata
  - baseurl/project/fileuuidORfilepath?view=meta
  - col names, indexes, and types
2. Actual
  - baseurl/project/fileuuidORfilepath
    - assume actual unless otherwise specified  

## CSV Filters/Keywords:
Delimited by `&`
- view
- cols
  - lack of cols means everything
  - comma separated positive integers including zero, e.g. `cols=0,3,7`
  - not required to be in order
- rowstart & rowcount
  - rowstart default is 0
  - rowcount default is from start to end of table  

`?view=actual&cols=0,2,3,5,7&rowstart=153&rowcount=1000`

## Image filters
- channel
  - index of colour channel
- zoom
- img_xoffset
- img_yoffset
  - both rel to top left corner
  - real pixels, i.e. agnostic of zoom levels
- img_width
- img_height
  - real pixels, i.e. agnostic of zoom levels



# Adding/Uploading Files
- POST request
- post the file
