# Postman test runner
Simple postman test runner to overcome simplicity of postman for repeating test on endpoints

- Simplify test creation (simply write a JSON)
- Repeat tests for each endpoint

You need to start your postman collection with the provided one as template. It includes code on the pre-script and test tabs of the collection

Then it uses the external data feature of postman to define the test iterations


# Tests capabilities
This is meant for simple tests, for more advanced scenarios you have to use postman capabilities.
It provides support for
- Modify request parameters
  - path params
  - query params
- Check the http response code
- Check response values


This is a sample for tests.
The postman external data format requires it to be an array, we will define all our tests on the "test" field of the first object.

```[ {
  "defaultTest": {
      "httpCode": [200, 204],
      "validateData": {}
  },
  "tests":{
        "echo test": [
          {
	    "description":"Test echo endpoint with one param",
            "queryParams": {
	      "foo": "bar"
	    },
            "httpCode": 200,
            "validateData": {
              "args.foo": "bar"
            }
          }
	]

}]
```

## Default tests

If you define on test on the field `defaultTests` they will be invoked for every tests

```
  "defaultTest": {
      "httpCode": [200, 204],
      "validateData": {}
  }
```


## Check endpoints

To identify the tests for every endpoint we use the same name as in the postman requests as fields under the `test` field.
Since this is an array, every object defined will become a request to the endpoint


```
 { 
 
   "tests" : {
      "first endpoint": [
         { // data for first test },
         { // data for second test }
      ]
   }
}
```

## Validate http response code

We can check against one or several possible return values

```
 { 
 
   "tests" : {
      "first endpoint": [
         { 
           "httpCode": 200
         }
      ],
      "second endpoint": [
         { 
           "httpCode": [200, 204]
         }
      ]
   }
}
```

## Request parameters

In postman we define params both in the query or the path, for example:

`http://server:port/:mypathvariable?param1=value`

to substitute params on the path we use the `pathParams` field with our desired values


```
  "pathParams" : {
     "mypathvariable" : "newpath"
  }
```

And for query params:

```
  "queryParams" : {
      "param1" : "new-value"
}
````

## Validate responses

Write data to validate under the `validateData` field

The most simple case checks for equal value, but there are other possibilities:
- check for regular expression if value starts with "/"
- check for null
- check for complex object (matches completely)
- check for element on an array
- check for array length


```
{
    "validateData": {
        "simple": "value1",
	"simple": "/value\d",
        "checkNull": null,
        "nested.field": "value2",
        "array": [{
             "field": 1234
        }],
	"array.0.field": 1234,
	"array.length": 1
    }
}
```
