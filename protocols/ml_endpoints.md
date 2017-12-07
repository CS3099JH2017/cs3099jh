# ML Group API
JSON API: http://jsonapi.org/format/

These API calls are to be made by BE groups. All data files to be sent in CSV format, see the CSV specification for more details.

## Training
`baseurl/trainning/{model_id}`
### Create Model
- Input: algorithm, data file UUID, model UUID, input columns, output columns.
- Output: none.
POST
```javascript
{
  "data": {
    "algorithm": "naive-bayes",
    "training_data": "bccf2c80-8ebb-4bec-a15c-6f699f36e765",
    "input_columns": [
      0
    ],
    "output_columns": [
      0
    ]
  }
}
```
### Status
- Input: model UUID.
- Output: boolean, model.
GET
```javascript
{
  "data": {
    "percent_trained": 0
  }
}
```
### Stop Training
- Input: model UUID.
- Output: none.
DELETE

## Model
`baseurl/model/{model_id}`
### Get Prediction
- Input: model file UUID, input data UUID.
- Output: results.
```javascript
/prediction?input_data=input_uuid
```
GET
```javascript
{
  "data": {
    "results": [
      [
        null
      ]
    ]
  }
}
```

## Default
`baseurl/`
### Request Suitable Models
- Input: data file uuid, input columns, output columns.
- Output: list of algorithms and descriptions.
```javascript
/suggest?data_id=data_uuid&input_columns=val1,val2,...,valn&output_columns=val1,val2,...,valn
```
GET
```javascript
{
  "data": {
    "algorithms": [
      {
        "name": "naive-bayes",
        "score": 0.3
      }
    ]
  }
}
```
