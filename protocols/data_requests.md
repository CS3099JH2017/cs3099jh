# THIS IS AN INITIAL **DRAFT** FOR A DATA REQUEST PROTOCOL
## Beginning of URL
`https://example.cs.med.ac.uk/files'

## Requests
Assuming this is preceded by something like the URL above, a request would be
of the form:  
`/files/project/directory/images/image::interface_name`  
where  
- a request is of a 'path-like' form,
- `project` would be the project id,
- `::` or a similar, non-path token separates the 'path' from the 'interface',
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

