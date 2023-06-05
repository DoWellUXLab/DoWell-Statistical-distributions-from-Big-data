# DoWell-Statistical-distributions-from-bigdata


Targeted population is an API that can be used to fetch data from a database. Normal, Poisson, Binomial and bernoulli distribution can be selected as the filteration method.

##### Request method: POST
##### Request URL: 
http://100032.pythonanywhere.com/api/targeted_population/

```
"database_details": {
    'database_name': 'mongodb', # database type
    'collection': 'some_collection', # collection name
    'database': 'some_data_base', # database name
    'fields':['a_field_name'] # column name or field name
}
```

The database name and collection name of the targeted data. The fields are those fields or column name on which the distribution logic will be applied.

```
"time_input" : {
    'column_name': 'Date', # column name for the date
    'split':'week', # split for poisson distribution
    'period': 'life_time', # the period from which data will be taken
    'start_point': '2021/01/08', # start point for custom period
    'end_point': '2021/01/25', # end point for custom period
}
```

For time input

Period can be `'custom'` or `'last_1_day'` or `'last_30_days'` or `'last_90_days'` or `'last_180_days'` or `'last_1_year'` or `'life_time'` and if `'custome'` is given then need to specify `start_point` and `end_point`

`'split'` value is required for poisson distribution. Split value can be `'week'`, `'hour'`, `'day'`, `'month'`.

### Stage input list

```
"stage_input_list": [
{
        'd': 1,
        'm_or_A_selction': 'population_average',
        'm_or_A_value': 300,
        'error': 10,
        'r': 100,
        'start_point': 0,
        'end_point': 700,
        'a': 2,
    },
]
```

statge_input_list is a list of stages
### For Norml distribution a stage can have

```
{
        'd': 1,
        'm_or_A_selction': 'population_average',
        'm_or_A_value': 300,
        'error': 10,
        'r': 100,
        'start_point': 0,
        'end_point': 700,
        'a': 2,
 }
 ```

- `datatype` can be `0` to `7`.
- `'m_or_A_selction'` can be `'maximum_point'` or `'population_average'`
- `'m_or_A_value'` is value of the `maximum_point'` or `'population_average'`
- `error` is the error allowed in percentage
- `r` is range
- `start_point` is the starting value of that stage
- `end_point` is the maximum value.
- `a` is the number of item that can be taken in a range

```
"distribution_input": {
    'normal': 1,
    'poisson':0,
    'binomial':0,
    'bernoulli':1
}
```

Give `1` if expecting the output for the respective distribution Give `0` if not expecting output for that distribution
Binomial distribution

If we are expecting binomial distribution result we need to provide the binomial distribution specific inputs.

```
"binomial" : {
    'number_of_variable':13,
    'split_choice': 'simple',
    'split_decision': "Eliminate",
    'user_choice_value': 43,
    'function': ">",
    'marginal_error': "0",
    'error': 20,
}
```

`'split_choice'` can be `'simple'` or `'calculated'`
`'split_decision'` can be `'Eliminate'` or `'Check_Accuracy'`
`'function'` can be `'>'` , `'='` , `'<'`

### For Bernoulli distribution

Bernoulli specific distribution is required while expecting result from Bernoulli distribution.

```
'bernoulli': {
    'error_size': 0.167,
    'test_number': 7,
    'selection_start_point': 500,
    'items_to_be_selected': 600,
}
```

### Curl example
```
curl --location 'http://100032.pythonanywhere.com/api/targeted_population/' \
--data '{
    "database_details": {
        "database_name": "mongodb",
        "collection": "test",
        "database": "Bangalore",
        "fields": [
            "LENGTH"
        ]
    },
    "distribution_input": {
        "normal": 1,
        "poisson": 1,
        "binomial": 0,
        "bernoulli": 0
    },
    "number_of_variable": 1,
    "stages": [
    ],
    "time_input": {
        "column_name": "Date",
        "split": "week",
        "period": "life_time",
        "start_point": "2021/01/08",
        "end_point": "2021/01/25"
    },
    "binomial": {},
    "bernoulli": {}
}'
```

### python example
```
import json
import requests

#url = 'http://127.0.0.1:5000/api/targeted_population/'
## production api url
url = 'http://100032.pythonanywhere.com/api/targeted_population/'


database_details = {
    'database_name': 'mongodb',
    'collection': 'test',
    'database': 'Bangalore',
    'fields':["LENGTH"]
}


# number of variables for sampling rule
number_of_variables = 1

# for first stage it's mandatory to have d=5
# period can be 'custom' or 'last_1_day' or 'last_30_days' or 'last_90_days' or 'last_180_days' or 'last_1_year' or 'life_time'
# if custom is given then need to specify start_point and end_point
# for others datatpe 'm_or_A_selction' can be 'maximum_point' or 'population_average'
# the the value of that selection in 'm_or_A_value'
# error is the error allowed in percentage


time_input = {
    'column_name': 'Date',
    'split': 'week',
    'period': 'life_time',
    'start_point': '2021/01/08',
    'end_point': '2021/01/25',
}

stage_input_list = [
    {
        'data_type': 1,
        'm_or_A_selction': 'maximum_point',
        'm_or_A_value': 500,
        'error': 40,
        'r': 100,
        'start_point': 0,
        'end_point': 700,
        'a': 2,
    }
]

# distribution input
distribution_input={
    'normal': 1,
    'poisson':1,
    'binomial':1,
    'bernoulli':0
    
}

bernoulli = {
    'error_size': 0.167,
    'test_number': 7,
    'selection_start_point': 500,
    'items_to_be_selected': 600,
}


binomial = {
    'number_of_variable':13,
    'split_choice': 'simple',
    'split_decision': "Eliminate",
    'user_choice_value': 43,
    'function': ">",
    'marginal_error': "0",
    'error': 20,
}




request_data = {
    'database_details': database_details,
    'distribution_input': distribution_input,
    'number_of_variable': number_of_variables,
    'stages': stage_input_list,
    'time_input': time_input,
    'binomial': binomial,
    'bernoulli': bernoulli,
}



headers = {'content-type': 'application/json'}

response = requests.post(url, json=request_data,headers=headers)

print(response.text)
```

### Output format
```
{
   "normal":{
      "is_error":false,
      "data":[
         [
         ]
      ],
      "sampling_rule":{
         "LENGTH":{
            "sampling_status":false,
            "sampling_status_text":"sample size is not adequate, univariate, 1<=1*10"
         }
      }
   },
   "poisson":{
      "is_error":false,
      "data":[]
    },
    "binomial":{},
    "bernoulli":{}
    
```
The `data` is a list for each fields

**** When `is_error` is `true` for a distribution, that's means something went wrong on applying that distribution
