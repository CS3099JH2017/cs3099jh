# DATA REQUST PROTOCOLS
## Example base URL
`https://example.cs.med.ac.uk/files`  
From here onwards in this file, we will assume that requests are
preceded by something similar to the above 'base URL'.

## Requests
A request has the form:  
`/project/files/directory/file?filters`  
such that:  
- a request is of a 'path-like' form,
- `file`can be either the file UUID or the file path,
- `project` will be the project name,
- `?` token separates the 'path' from the 'interface',
- `filters` as described below.

## Filters
Filters will:
- be preceded by a `?`
- then have a number of 'key-value pairs', separated by `&`

## CSV File Requests
1. Metadata
  - `baseurl/project/files/fileuuid?view=meta`  
    OR  
    `baseurl/project/files/filepath?view=meta`
  - returns column names and indexes
2. Actual data
  - `baseurl/project/files/fileuuid`  
    OR  
    `baseurl/project/files/filepath`
    - assume actual data unless otherwise specified  
  - FILTERS: see below

### CSV Filters/Keywords:
Delimited by `&`
- `view`
  - if specified, value has to be `meta`
  - lack of `view` or value different from `meta` defaults to request for
    actual data
- `cols`
  - lack of `cols` defaults to all columns
  - comma separated positive integers including zero, e.g. `cols=0,3,7`
  - indexes not required to be in numerical order
- `rowstart` and `rowcount`
  - `rowstart` default is 0 and otherwise specifies the first row to be included in the result
  - `rowcount` default is from `rowstart` to end of table and otherwise specifies the count
    of rows, including the first row, to be returned

## Example
`?view=actual&cols=0,2,3,5,7&rowstart=153&rowcount=1000`

## Image filters
- `img_channel`
  - index of colour channel
- `img_zoomlevel`
  - a number representing the 'zoom level' desired <!--TODO-->
- `img_xoffset`
  - relative to top left corner
  - UNITS: 'real pixels', i.e. agnostic of zoom levels
- `img_yoffset`
  - relative to top left corner
  - UNITS: 'real pixels', i.e. agnostic of zoom levels
- `img_width`
  - UNITS: 'real pixels', i.e. agnostic of zoom levels
- `img_height`
  - UNITS: 'real pixels', i.e. agnostic of zoom levels


## Adding/Uploading Files
- POST request
- post the entire file
