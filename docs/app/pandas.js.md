wq/pandas.js
============

[wq/pandas.js]

**wq/pandas.js** is a tool to load and parse CSV files generated by Pandas dataframes.  wq/pandas.js extends d3's built in CSV parser with a more robust way to handle the complex CSV headers Pandas DataFrames can produce.  wq/pandas.js is primarily intended for use in conjunction with [Django REST Pandas].

wq/pandas.js exports two functions: `pandas.parse()` and `pandas.get()`.  For similarity with `d3.csv()`, these are also added to the `d3` object as `d3.pandas.parse()` and `d3.pandas()`, respectively. 

## parse()

`pandas.parse()` parses a CSV string with the following structure:

```bash
,value,value,value
site,SITE1,SITE2,SITE3
parameter,PARAM1,PARAM1,PARAM2
date,,,
2014-01-01,0.5,0.5,0.2
2014-01-02,0.1,0.5,0.2
```

Into an array of datasets with the following structure:
```javascript
[
  {
    'site': 'SITE1',
    'parameter': 'PARAM1',
    'list': [
      {'date': '2014-01-01', 'value': 0.5},
      {'date': '2014-01-02', 'value': 0.1}
    ]
  }
  // etc for SITE2/PARAM1 and SITE3/PARAM2...
]
```

`pandas.parse()` also supports multi-valued datasets, e.g.:

```bash
,val1,val2
site,SITE1,SITE1,
parameter,PARAM1,PARAM1
date,,
2014-01-01,0.6,0.3
```
Which will be parsed into:

```javascript
[
  {
    'site': 'SITE1',
    'parameter': 'PARAM1',
    'list': [
      {'date': '2014-01-01', 'val1': 0.6, 'val2': 0.3}
    ]
  }
]
```

## get()

`pandas.get()` is a simple wrapper around `d3.xhr()` to make it easy to load and parse pandas data from an external file or REST service.

### Example Usage

```javascript
define(['d3', 'wq/pandas'], function(d3, pandas) {

pandas.get('/data.csv' render);

function render(error, data) {
    d3.select('svg')
       .selectAll('rect')
       .data(data)
       // ...
}

});

```

[wq/chart.js] provides some example charts that work well with the output of `pandas.parse()`.

[wq/pandas.js]: https://github.com/wq/wq.app/blob/master/js/wq/pandas.js
[wq/chart.js]: http://wq.io/docs/chart-js
[Django REST Pandas]: https://github.com/wq/django-rest-pandas