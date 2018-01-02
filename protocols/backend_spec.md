# BE01: Authentication, Files and Projects
**THIS IS A DRAFT SPECFICATION AND IS NOT FINAL**

| name                                        | BE01               |
|---------------------------------------------|--------------------|
| status                                      | proposal           |
| author                                      | Ryan Wilson (rw86) |
| serving component(s)                        | backend            |
| consuming component(s)                      | all                |
| basic spec                                  | yes                |
| can be required by servers                  | yes                |
| can be required by clients                  | yes                |

    The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

## Foreword: URL

Within this document the URL `http://backend.endpoint/` will be used to indicate the base URL of the backend server's API endpoint.

## Foreword: Matching notation

At several points in this document a specification of a JSON value must be given. These specifications are to be interpreted as follows:

### literals
The template `integer` matches any integer value.

The template `string` matches any string value.

The template `boolean` matches any boolean value.

The template `anything` matches any value.

The template `not_present` requires that the property is not present in the containing object.
### raw object
objects match if the value contains corresponding values for each given key. The value may have additional, not specified, keys.
```javascript
{
    "required": integer
}
```
matches
```javascript
{
    "required": 1,
    "other": 2
}
```
but not
```javascript
{
    "other": 2
}
```
### exact matching
if an object is enclosed in `exact(...)` then it may not have additional keys, in particular
```javascript
exact({
    "required": integer
})
```
does not match
```javascript
{
    "required": 1,
    "other": 2
}
```
#### raw array
The array must match exactly including number of elements and the order of those elements.
```javascript
[1, 2, 3]
```
does not match
```javascript
[1, 2, 3, 4]
```
or
```javascript
[3, 2, 1]
```

### optional values
Templates of the form ` optional(x)` match either values that are not present or values that match `x`
```javascript
{
    "required": integer,
    "other": optional(2)
}
```
matches
```javascript
{
    "required": 1
}
```
and
```javascript
{
    "required": 1,
    "other": 2
}
```
but not
```javascript
{
    "required": 1,
    "other": 3
}
```
### variable length arrays
templates of the form ` array(x)` matches an array whose elements all match `x`
```javascript
array({"a": 1})
```
matches
```javascript
[
    {"a": 1},
    {"a": 1, "b": 2},
    {"a": 1, "c": 4}
]
```
but not
```javascript
[
    {"a": 1},
    {"a": 1, "b": 2},
    {"c": 4}
]
```
### alternatives
templates of the form ` alternative([x, y, ..., z])` matches values who are matched by any of `x`, `y` ... `z`
```javascript
{
    "url": alternative([
        string,
        {
            "host": string,
            "path": string
        }
    ])
}
```
matches both
```javascript
{
    "url": "http://example.com/path"
}
```

and
```javascript
{
    "url": {
        "host": "http://example.com",
        "path": "/path"
    }
}
```
### not present
```javascript
{
    "a": not_present
}
```

is matched by
```javascript
{
    "b": "thing"
}
```
but not
```javascript
{
    "a": "thing"
}
```
## Basic Response
Unless otherwise noted requests and responses are made in JSON, and the servers `Content-Type` header should indicate a mimetype of `application/json`. Requests and responses **SHOULD** be encoded in `UTF-8`.

Unless otherwise noted the form of the JSON response **MUST** match either:
```javascript
{
    "status": "error",
    "error": string,
    "error_description": string,
    "error_data": optional({})
}
```
in the case of an error. Where `error` gives a standard error name found from a specification and `error_string` some human readable error message.


Or
```javascript
{
    "status": "success",
    "data": anything
}
```
In the case of success. The specification may use the phrase "unwrapped response" or "unwrapped content" to refer to the contents of the `"data"` attribute (the response is required or assumed to be successful).


When the specification mentions the server "returning error `x`" the response should match
```javascript
{
    "status": "error",
    "error": x,
    "error_description": string
}
```

A successful response without any data should match
```javascript
{
    "status": "success",
    "data": {}
}
```

If a client request does not match a pattern that the specification requires that it matches the server **SHOULD** respond with error code 400 and error "invalid_request". eg, something like
```javascript
{
    "status": "error",
    "error": "invalid_request",
    "error_description": "The client made an invalid request."
}
```
## Internal errors
At any time the server **MAY** respond to a request with response code 500 and error message "internal_server_error"

## Indicating protocol support

Upon request to the url
```
http://backend.endpoint/_supported_protocols_
```
Servers **MUST** respond with a response, which when unwrapped matches:
```javascript
exactly({
    "supported": array(string),
    "required": array(string)
})
```

`"supported"` **SHOULD** be an array of specification names (for example "BE01" for this specification) for specifications on which the client may rely on the server conforming to. **NOTE** that the *client* is not required to conform to the listed specification.

`"required"` **MUST** be an array of specification names (for example "BE01" for this specification) on which the client may rely on the server conforming but the server **will assume** that client the client conforms to.

A specification **MUST** only be listed in at most one of these arrays. Specifications **SHOULD** only be listed in required if the specification indicates at the top "can be required by servers". Clients may refuse to operate with servers who do not support or require some set of specifications however they **SHOULD** only do this when the specification indcates "can be required by clients".

Note that names are always two uppercase characters followed by two digits.

## General updates
When an update is issued that causes an error (eg. due to invalid data) the server **MUST NOT** have partialy applied attributes in the update. IE. updates are atomic.

## Metadata updates
Several objects have associated metadata objects for clients to use. This metadata **SHOULD** be interpreted as follows

```javascript
exact({
    "version": integer,
    "namespaces": {}
})
```
(this will be refered to as `metadata` within patterns)

The pattern `inital_metadata` is defined to be 
```javascript
{
    "version": 1,
    "namespaces": {}
}
```

`"version"` is a monotonically increasing counter that increments by one on each metadata update. Clients should choose an appropriate namespace to store their data in: all namespaces not prefixed by an underscore are reserved. Groups are given exlusive access to the namespace given by their group name (uppercase without a dash eg. "ML1").

Example metadata might be
```javascript
{
    "version": 12,
    "namespaces": {
        "ML1": {
            "model_store_dir": "_reserved/ML1/models"
        },
        "HCI3": {
            "display_name": "Microscopy analysis",
            "creation_date": "...",
            "created_by": "..."
        }
    }
}
```

When setting metadata the backend must ensure that it matches the `metadata` template and have a the version counter be exactly one greater than that within current metadata stored (this allows race-free read-modify-write). If there is not any metadata currently stored then no extra restriction is placed on the version field.

If the format of metadata inside a client request is not correct the server should responsd with code 400 and error "invalid_metadata"

If the version number is not correct the server should respond with "invalid_metadata_version" in this case the client should retry (starting again from read).

## OAuth
**Note that responses here do not exactly follow the JSON response format**
The URL
```
http://backend.endpoint/oauth/token
```
**MUST** be compliant with the following, reduced, specification of a token granting endpoint from OAuth2.

### Password grant
Clients **MUST** send all paramters urlencoded in the body of a `POST` request to the endpoint. Requests **MUST** be of the form
```
grant_type=password
username=<username>
password=<user password>
```

The server then tries to authenticate the user and, if successful, **SHOULD** respond with

```javascript
exact({
    "token_type": "bearer",
    "access_token": string,
    "refresh_token": string,
    "expires_in": integer
})
```
the `expires_in` field gives the validity period of the tokens, it applies to both the `access_token` and the `refresh_token`, it is specified in seconds and **SHOULD** be at least 6 hours.


If the credentials were not valid the server **MUST** respond with HTTP code 400 and response body
```javascript
exact({
    "error": "invalid_grant",
    "error_description": string
})
```

If the client made a malformed request, eg. missing parameters, then the server **MUST** respond with HTTP code 400 and body
```javascript
exact({
    "error": "invalid_request",
    "error_description": string
})
```

If the client indicates a `grant_type` options that the server does not support (not one of `"refresh_token"` or `"password"`) then the server **MUST** respond with HTTP code 400 and body
```javascript
exact({
    "error": "unsupported_grant_type",
    "error_description": string
})
```

### Refresh grant
Long running clients (eg. machine learning servers performing jobs that may take a long time to complete) may wish to get an access token with a longer validity time than the one they currently have to avoid having to cancel an operation or prompting the user for credentials. To meet this need backend servers **MUST** also support refresh token grants.

When making a refresh token grant clients **MUST** send all paramters urlencoded in the body of a `POST` request to the endpoint. Requests **MUST** be of the form
```
grant_type=refresh_token
refresh_token=<refresh token>
```
where `refresh_token` is a token that previously appeared in a server response and is still under its validity period.


The server **MUST** check that the token is still inside its validity period and if it is **SHOULD** respond with a value matching
```javascript
exact({
    "token_type": "bearer",
    "access_token": string,
    "refresh_token": string,
    "expires_in": integer
})
```
The response gives the new token. This response is subject to the same conditions as the response to a password grant. The server **MAY** reuse the `access_token` or `refresh_token` but if it does so it **MUST** ensure that the validity of the new tokens extend to match the new expiry time.

### Using tokens
After reciving a valid access token the client uses it to authenticate by setting its `Authorisation` HTTP header to exactly
```javascript
"Bearer " + access_token
```
in subsequent requests to the server.

### Unauthorised requests

If a client makes a request to a resource that it does not have access to then the server **SHOULD** response with HTTP code 401 and `"not_authorised"` error. Ie. A response matching
```javascript
{
    "status": "error",
    "error": "not_authorised",
    "error_description": string
}
```
## Logging
### storing
The server **MAY** provide a logging endpoint for all components to use.
Any authorised client may POST to
```
http://backend.endpoint/log
```
with request matching
```javascript
array({
    "component": string,
    "level": alternative(["info", "security", "warning", "error", "critical"]),
    "value": {}
})
```
If the server does not implement logging it **MUST** report an error "logging_not_enabled" and http error code 501.

The server must store alongside each message the username of the logged in user who posted the message, and a timestamp that the message was received at. The server **MAY** selectively drop log messages (below a certain priority level, or from users who do not have a given privilege, for example) it is recommended that they expose properties to control this (see section on properties). The server **MAY** also implement log roation/expiration.

If the request was valid and the server supports logging it should respond with an sucessful empty response.

### retrieving
Clients authorised as a user with the `admin` privilege can access
```
http://backend.endpoint/log?before=<ISO 8601 date time>&after=<ISO 8601 date time>&level=<level>
```
To retrieve log messages in the given date range and at log level `<level>` or above. Any parameter can be ommited in which case the given filter (before, after or log level) is not applied.

If the server does not implement logging it **MUST** report an error "logging_not_enabled" and http error code 501.

otherwise the serer response should match
```javascript
array({
    "component": string,
    "level": alternative(["info", "security", "warning", "error", "critical"]),
    "value": {},
    "username": string,
    "timestamp": string
})
```
`"timestamp"` should be an ISO 8601 formatted datetime. The array should be sorted newest to oldest.

## Properties
The server may wish to expose user/progam configurable properties. 

Clients authorised as a user with the `admin` privilege can access
```
http://backend.endpoint/properties
```
and the server **MUST** respond with a list of properties, with the unwrapped response matching
```javascript
array({
    "id": string,
    "display": optional({
        "category": string,
        "group": string,
        "display_name": string,
        "description": string
    }),
    "read_only": boolean,
    "type": alternative(["string", "int"]),
    "value": alternative([string, int])
})
```
properties providing a `"display"` key **SHOULD** be made available to the user as configuration options by the client. `"read_only"` properties should only be displayed and not given an edit option for.

To update propert(ies) the client POSTS to
```
http://backend.endpoint/properties?action=update
```
with body
```javascript
array({
    "id": string,
    "value": alternative([string, int])
})
```
The backend **SHOULD** only update properties given in body.

If a property that does not exist or is read only is specified the backend **SHOULD** respond with http response 400 and error "invalid_property" and set `"error_data"` to the property's name.

If an invalid property value is given the backend **SHOULD** respond with http response 400 and error "invalid_property_value" and set `"error_data"` to the property's name.

## Users
The backend server supports the concept of a users.
Users have at least:
- A username
- A password (this **SHOULD** be stored hashed and salted)
- A set of privileges (the set of available privileges should include at least `admin`)
- A set of projects and project access rights (project access rights should include, at least, `project_admin` and `regular`)
- A metadata object that can be accessed by all, and updated by the user, "public_user_metadata"
- A metadata object that can be accessed by all, and updated by users with the `admin` privilege, "public_admin_metadata"
- A metadata object that can be accessed by the user, and updated by the user, "private_user_metadata"
- A metadata object that can be accessed by users with the `admin` privilege, and updated by users with the `admin` privilege, "private_admin_metadata"

### Listing
Clients can access
```
http://backend.endpoint/users
```

If the client is authorised as a user with the `admin` privilege  the server **MUST** respond with a list of users, with the unwrapped response matching
```javascript
array({
    "username": string,
    "privileges": array(string),
    "projects": array({
        "project_name": string,
        "access_level": string
    }),
    "public_user_metadata": metadata,
    "private_user_metadata": metadata,
    "public_admin_metadata": metadata,
    "private_admin_metadata": metadata
})
```

If the client is authorised as a user without the `admin` privilege  the server **MUST** respond with a list of users, with the unwrapped response matching
```javascript
array({
    "username": string,
    "privileges": array(string),
    "projects": array({
        "project_name": string,
        "access_level": string
    }),
    "public_user_metadata": metadata,
    "private_user_metadata": not_present,
    "public_admin_metadata": metadata,
    "private_admin_metadata": not_present
})
```

### Specific user
Clients can access
```
http://backend.endpoint/users/<username>
```

If the client is authorised as a user with the `admin` privilege the server **MUST** respond with details of the particular user if they exist (unwrapped response):
```javascript
{
    "username": string,
    "privileges": array(string),
    "projects": array({
        "project_name": string,
        "access_level": string
    }),
    "public_user_metadata": metadata,
    "private_user_metadata": metadata,
    "public_admin_metadata": metadata,
    "private_admin_metadata": metadata
}
```

If the client is authorised as a user without the `admin` privilege the server **MUST** respond with details of the particular user if they exist (unwrapped response):
```javascript
array({
    "username": string,
    "privileges": array(string),
    "projects": array({
        "project_name": string,
        "access_level": string
    }),
    "public_user_metadata": metadata,
    "private_user_metadata": not_present,
    "public_admin_metadata": metadata,
    "private_admin_metadata": not_present
})
```

or if the user does not exist it **MUST** return HTTP response code 404 and a `"user_not_found"` error.

### current user
Any client may access details about their currently logged in user from
```
http://backend.endpoint/current_user
```
and the server **MUST** respond with details of the particular user if they exist (unwrapped response):
```javascript
{
    "username": string,
    "privileges": array(string),
    "projects": array({
        "project_name": string,
        "access_level": string
    }),
    "public_user_metadata": metadata,
    "private_user_metadata": metadata,
    "public_admin_metadata": metadata,
    "private_admin_metadata": not_present
}
```

### Creating
A client with the `admin` privilege may create a user by issuing a POST request to 
```
http://backend.endpoint/users/<username>?action=create
```
with body matching
```javascript
{
    "privileges": array(string),
    "password": string,
    "public_user_metadata": optional(metadata),
    "private_user_metadata": optional(metadata),
    "public_admin_metadata": optional(metadata),
    "private_admin_metadata": optional(metadata)
}
```
(note that there is no need to indicate the username inside the request body)
the metadata attributes **SHOULD** be assumed to be `inital_metadata` if not present in the request.

The server **MUST** either send a successful response without data,

or return an error "invalid_user" and response code 400. This may happen, for example, if the password does not meet some complexity requirements or if an invalid privilege was specified

or return an error "user_already_exists" and response code 400 if the user exists already.

### Updating
A client with the `admin` privilege may update a user by issuing a POST request to 
```
http://backend.endpoint/users/<username>?action=update
```
with body matching
```javascript
{
    "privileges": optional(array(string)),
    "password": optional(string),
    "public_user_metadata": optional(metadata),
    "private_user_metadata": optional(metadata),
    "public_admin_metadata": optional(metadata),
    "private_admin_metadata": optional(metadata)
}
```
(note that there is no need to indicate the username inside the request body)

The server **SHOULD** only update the attributes listed in the request.
Metadata **MUST** be processed in accordance with the "metadata" section of this specification.

The server **MUST** either send a successful response without data,

or return an error "invalid_user" and response code 400. This may happen, for example, if the password does not meet some complexity requirements or if an invalid privilege was specified

or return an error "user_not_found" and response code 404 if the user cannot be found.

or return a metadata error.

### Updating yourself
A client may update a subset of the details of the user it is logged in as by issuing a POST request to
```
http://backend.endpoint/current_user?action=update
```
with body matching
```javascript
{
    "password": optional({
        "old": string,
        "new": string
    }),
    "public_user_metadata": optional(metadata),
    "private_user_metadata": optional(metadata)
}
```
The server should only attempt to update the attributes specified in the request.

When updating the password the server **MUST** ensure that old password matches, if it does not the server **SHOULD** respond with response code 400 and error "invalid_password".

Metadata **MUST** be processed in accordance with the "metadata" section of this specification. Note that "public_admin_metadata" and "private_admin_metadata" **MUST** not be updated.

The server **MAY** return an error "invalid_user" and response code 400 if, for example, the password does not meet some complexity requirements.

or return a metadata error.

If the change was successful the server **MUST** respond with a sucessful empty response.

### Deleting
A client with the `admin` privilege may delete a user (who is not themself) by issuing a POST request to 
```
http://backend.endpoint/users/<username>?action=delete
```
The server **MUST** respond "user_not_found" and response code 404 if the user cannot be found.

or if the user is the current user, 400 and error message "invalid_user",

or if the user was deleted a successfull empty response.

## Projects
The backend server supports the concept of a project.
Projects have at least:
- A fixed name
- A set of users and project access rights (project access rights should include, at least, `project_admin` and `regular`)
- Public metadata object that is visible to all users but only editable by users with the `project_admin` access right, "public_metadata"
- Private metadata object that is visible to all users with at least the `regular` access right but only editable by users with the `project_admin` access right, "private_metadata".
- Private metadata object that is only visible and editable users with the `project_admin` access right, "admin_metadata".

### Listing
All clients can access
```
http://backend.endpoint/projects
```

and the server **MUST** respond with a list of projects, with the unwrapped response matching
```javascript
array({
    "project_name": string,
    "users": array({
        "username": string,
        "access_level": string
    }),
    "public_metadata": metadata,
    "private_metadata": optional(metadata),
    "admin_metadata": optional(metadata)
})
```
`"private_metadata"` **MUST** only be present when the client has at least `regular` access to the given projects.

`"admin_metadata"` **MUST** only be present when the client has at least `project_admin` access to the given projects.

### Specific project
Clients that have at least `regular` access to a given project can request
```
http://backend.endpoint/projects/<project_name>
```

and the server **MUST** respond with details of the particular project if it exists (unwrapped response):
```javascript
{
    "project_name": string,
    "users": array({
        "username": string,
        "access_level": string
    }),
    "public_metadata": metadata,
    "private_metadata": metadata,
    "admin_metadata": optional(metadata)
}
```
`"admin_metadata"` **MUST** only be present when the client has at least `project_admin` access to the given projects.

or if the project does not exist it **MUST** return HTTP response code 404 and a `"project_not_found"` error.

### Creating
A client with the `admin` privilege may create a project by issuing a POST request to 
```
http://backend.endpoint/projects/<project_name>?action=create
```
with body matching
```javascript
{
    "public_metadata": optional(metadata),
    "private_metadata": optional(metadata),
    "admin_metadata": optional(metadata)
}
```
(note that there is no need to indicate the project name inside the request body)
the metadata attributes **SHOULD** be assumed to be `inital_metdata` if not present in the request.

Metadata **MUST** be processed in accordance with the "metadata" section of this specification.

The server **MUST** either send a successful response without data,

or return an error "invalid_project" and response code 400,

or return an error "project_already_exists" and response code 400 if the user exists already.

or return a metadata error.

### Updating
A client with the `project_admin` access right may update a project by issuing a POST request to 
```
http://backend.endpoint/projects/<project_name>?action=update
```
with body matching
```javascript
{
    "public_metadata": optional(metadata),
    "private_metadata": optional(metadata),
    "admin_metadata": optional(metadata)
}
```
(note that there is no need to indicate the project_name inside the request body)
The server **SHOULD** only update the attributes listed in the request.
Metadata **MUST** be processed in accordance with the "metadata" section of this specification.

The server **MUST** either send a successful response without data,

or return an error "invalid_project" and response code 400,

or return an error "project_not_found" and response code 404 if the project cannot be found.

or return a metadata error.

### Deleting
A client with the `admin` privilege (**NOTE** different to `project_admin` access rights) may delete a project by issuing a POST request to 
```
http://backend.endpoint/projects/<project_name>?action=delete
```
The server **MUST** respond "project_not_found" and response code 404 if the project cannot be found.

or if the project was deleted a successfull empty response.

### Project grants
A client with the `project_admin` access right may change a users access rights to a project by issuing a POST request to 
```
http://backend.endpoint/projects/<project_name>?action=update_grant
```

with request matching
```javascript
{
    "username": string,
    "access_level": string
}
```
where access level is a valid access level or the value `"none"` to remove access.

The server **MUST** respond "project_not_found" and response code 404 if the project cannot be found.

The server **MUST** respond "user_not_found" and response code 404 if the user cannot be found.

The serve **MUST** respond with a successful empty response if the access level was updated.

## File access
**NOTE:** all file operations require that clients have at least `regular` access to the project.

### File paths
Each project has associated with a file store. Files are organised into directories. Valid File/directory names are `UTF-8` strings that do not contain "/" or "\". "." and ".." are **not** valid file/directory names.

Valid paths consist of valid file names seperated by a single "/".

If an invalid path is presented the server **SHOULD** with response code 400 and error "invalid_path".

The path "" (empty string) refers to the root directory of the project.

The path "_reserved" is reserved for a directory. All subpaths are all reserved. Groups are given exlusive access to the subdirectory given by their group name (uppercase without a dash) eg. "_reserved/ML1". Clients **SHOULD** hide the reserved directory from end-users and **MAY** hide the directory from listings (although allowing access with the direct path may be helpful).

a file path `<path>` inside project `<project_name>` is accessed under the URL
```
http://backend.endpoint/projects/<project_name>/files/<path>
```

Files also have an id associated with them, if the id of a file is known to be `<id>` then the file can also be accessed under the path
```
http://backend.endpoint/projects/<project_name>/files_by_id/<id>
```

If a request (that is not a create request) is made for a path/id that does not exist then the server **SHOULD** with response code 404 and error message "file_not_found".

### views
Files are accessed under a given view specifed by setting the query parameter `view=<view_name>`. All files support at least the `"meta"` view. If a view is not specified then the `"meta"` view assumed.

All no directory files support access to the raw bytes with the `"raw"` view.

### Meta view
If a meta request is made for the meta view of a given file. eg:
```
http://backend.endpoint/projects/<project_name>/files/<path>
```
the server *MUST* respond with a unwrapped response matching
```javascript
{
    "file_path": string,
    "file_name": string,
    "id": string,
    "supported_views": {},
    "type": string
}
```
`"file_path"` gives the full path to the file, `"file_name"` gives this path without any parent directories (eg the basename of the path).

`"supported_views"` is an object who's keys correspond to the supported views and values correspond to paramters supported by that view.

`"type"` is a file type. The type "generic" is for files who's type is not known and support only "meta" and "raw" views. The type "directory" is for directories.

### Meta view - directories
If the file is a directory and the query paramter "include_children" is included in the request then the meta view will additionally match:
```javascript
{
    "children": array({
        "file_path": string,
        "file_name": string,
        "id": string,
        "supported_views": {},
        "type": string
    })
}
```
where the "children" array contains the meta view for all the children of the directory. *NOTE:* the "include_children" option is not recursive, child directories will not list their children.

### Meta view - raw files
If files support the raw view then the meta view should match
```javascript
{
    "supported_views": {
        "raw": {
            "size": integer
        }
    }
}
```
where size give the size of the file in bytes.

### unsupported views
If the client makes a request using an unkown or unsupported view then the server **MUST** respond with code 400 and error 'unsupported_file_view'.

### raw file view
The "raw" file view (accssed by setting "view=raw" in the query string) provides access to the file as a stream of bytes.
The following paramters are accepeted:

| name   | description                                                                                                      | if not specified                     |
|--------|------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| offset | offset into the file, in bytes - the server will omit this number of bytes from the start of the response        | assume 0                             |
| length | total number of bytes to retrieve - server will truncate the response so that it is at most "length" bytes long. | data is not truncated before sending |

The server **MUST** respond with mimetype `application/octet-stream'`.

### uploading
files are uploaded by sending a POST request to the file URL either without the query paramter "action" or with it set to the value "upload".
The following addiontional paramters **MUST** be accepted by the server

| name      | description                                                                                                                                                | if not specified                                                                      |
|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| overwrite | allow file overwrites - do not cause an error if the file already exists.                                                                                  | exit with error code 400 and message "file_already_exists" if the file already exists |
| offset    | seek to the specified offset in the file before writing - if the file has to be extended to meek this request then the new space will be filled with zeros | data is written to the start of the file                                              |
| truncate  | after writing the size of the file is reduced so that the last byte in the file is the last byte written                                                   | file size never shrinks                                                               |

**NOTE:** A file can be truncated to a give size by issuing an empty POST request with paramters `overwrite=true&offset=<new_size>&truncate=true`

If the parent directory does not exist (or is not a directory) the server **MUST** respond with code 404 and error message "invalid_parent_directory".

If the write was successfull the server **MUST** respond with an empty successful response.

**NOTE: Chunked Uploads** for large files clients are recommended to, where possible, split the file into fixed size chunks (eg. 4Mib) and upload each chunk individually. This allows resuming interrupt uploads as well as progress reporting to the end user.

### creating directories
If a POST request is made to a non existent path with query paramter "action=mkdir" an attempt to make a new directory is made.

If there is already a file with this name the server responds with status code 400 and error message "file_already_exists".

If the parent directory does not exist (or is not a directory) then the server should respond with error code 404 and error message "invalid_parent_directory"

### deleting
If a POST request is made to a non existent path with query paramter "action=delete" an attempt to delete the file/directory is made.

Deletes on directories delete their contents and are recursive.

If an attempt to delete the root directory is made the server **MUST** respond with error code 400 and error message "invalid_operation".

If the file does not exist the server should respond with code 404 and message "file_not_found"

### Extra file types
The file type (and hence supported views) of a file are determined by some unspecifed process on the backend, this may include looking at the file extension and possibly the file contents. The server **SHOULD** recognise valid files that have the correct extention and file contents. If the client requires that a file type is correctly recognised it **SHOULD** ensure that it is uploaded under a file name with the correct extention.

## Tabular files
A file type that contains tabular data (eg. excel, csv) should reports its file type as tabular in its meta view:
```javascript
{
    "type": "tabular"
}
```
Additionally it should support the tabular view:
```javascript
{
    "supported_views": {
        "tabular": { 
            "columns": array(string),
            "rows": integer
        }
    }
}
```
Where columns gives all of the names of the columns. and rows the number of (non-header) rows.

### The tabular view
The tabular view supports the following parameters

| name     | description                                                                                                                                        | if not specified           |
|----------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| rowstart | first non-header row to be included in the output                                                                                                  | assume = 0                 |
| rowcount | maximum number of non header rows to include in the output                                                                                         | all matching rows included |
| cols     | comma separated list of all column indices (zero based indices into the "columns" field of the meta view) for columns to be included in the output | all columns included       |

The mimetype of a successful response *MUST* be "text/csv". The CSV response **always** includes the header row.

if rowstart is passed then the end of the file then no non-header rows are returned.

if cols contains invalid entries then the server **SHOULD** respond with code 400 and error 'invalid_request'.


## Zommable image files
A file type that contains images that can be rescaled and zoomed. The file type, as seen in the meta view, should be:
```javascript
{
    "type": "scalable_image"
}
```
Additionally it should support the following view:
```javascript
{
    "supported_views": {
        "scalable_image": { 
            "width": integer,
            "height": integer,
            "channels": array({
                "channel_name": string,
                "meta": metadata
            }),
            "meta": metadata
        }
    }
}
```
where width and height give the image dimensions (at the max scale level) and the meta attributes contain metadata extracted from the image. (under an appropriate namespace).

### The Scalable Image view
This view supports the following parameters

| name         | description                                                                                                                  | if not specified                                |
|--------------|------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|
| channel_name | image channel to operate on                                                                                                  | send response 400, with error "invalid_request" |
| zoom_level   | rescales the image before returning to client to be `requested_width / zoom_level` by `requested_height / zoom_level` pixels | assume = 1                                      |
| x_offset     | x offset, in unscaled pixels, of the region requested from the top left corner                                               | assume = 0                                      |
| y_offset     | y offset, in unscaled pixels, of the region requested from the top left corner                                               | assume = 0                                      |
| width        | width, in unscaled pixels, of the region requested                                                                           | set so that x_offset + width = image_width      |
| height       | height, in unscaled pixels, of the region requested                                                                          | set so that y_offset + height = image_height    |
    

The mimetype of a successful response *MUST* be "image/png".

If the region specified extends outside the image the server should fill in the remainder of the image with the color `#000000ff`.

x_offset, y_offset, width and height should all be a multiple of zoom_levl otherwise the server **MAY** respond with response code 400 and error 'invalid_request'.
