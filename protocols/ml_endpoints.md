# ML Group API
JSON API: http://jsonapi.org/format/

These API calls are to be made by BE groups. All data is to be sent in CSV format, see the CSV specification for more details.

### Create Model
- Input: algorithm, data file UUID, input columns, target columns.
- Output: training ID.
```javascript
baseurl/createmodel/
```
POST
```javascript
{
	meta: {
		TODO		
	}
}
```

### Get Prediction
- Input: model file UUID, target data UUID.
- Output: results.
```javascript
baseurl/predict/?model=model_uuid&target=target_uuid
```
GET
Response:
```javascript
{
	TODO
}
```

### Stop Training
- Input: Training ID.
- Output: model.
```javascript
baseurl/stop/?id=training_id
```
GET
Response:

### Check Model is Finished
- Input: Training ID.
- Output: boolean, model.
```javascript
baseurl/isfinished/?id=training_id
```
GET
Response:
TODO

### Request Suitable Models
- Input: data file uuid, input columns, target columns.
- Output: list of algorithms and descriptions.
```javascript
baseurl/requestmodels/data=data_uuid&input_columns=val1,val2,...,valn&target_columns=val1,val2,...,valn
```
GET
Response:
TODO
