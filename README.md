# swmf_validation

cdaweb is a python module that provides functions to download data from CDAWeb (https://cdaweb.sci.gsfc.nasa.gov/index.html/) using their REST API (https://cdaweb.gsfc.nasa.gov/WebServices/REST/).

## Getting started

### Prerequisites

- python 2.7
- spacepy

### Installing

```shell
python setup.py install
```

### Example

First, get a list of dataviews (at time of writing this can be skipped, since the only dataview is 'sp_phys'):

```python
import cdaweb
dataviews=cdaweb.get_dataviews()
```

Next, get a list of datasets in a dataview:

```python
import cdaweb
datasets=cdaweb.get_datasets('sp_phys')
```

datasets is a list of dictionaries, each containing metadata for each datset. For instance, datasets[0] contains:

```python
{'Id': 'UY_M0_PFRA',
 'Instrument': 'PFRA',
 'InstrumentType': 'Radio and Plasma Waves (space)',
 'Label': 'Ulysses PFRA 10 minute average data. - R MacDowall (NASA Goddard Spaceflight Center)',
 'Notes': 'https://cdaweb.sci.gsfc.nasa.gov/misc/NotesU.html#UY_M0_PFRA',
 'Observatory': 'ULYSSES',
 'ObservatoryGroup': 'Ulysses',
 'PiAffiliation': 'NASA Goddard Spaceflight Center',
 'PiName': 'R MacDowall',
 'TimeInterval': {'End': '2004-12-31T00:00:00.000Z',
                  'Start': '1990-10-29T00:00:00.000Z'}}
```

The 'Id' attribute of each dataset is used to access its data. For instance, datasets[0]['Id'] is 'UY_M0_PFRA'. We can get a list of its variables (and their information) with the get_variables function:

```python
import cdaweb
variables=get_variables('sp_phys','UY_M0_PFRA')
pprint(variables)
```

This prints a list of variables in the dataset:

```python
[{'LongDescription': 'PFR Average electric field intensity in 16 channels',
  'Name': 'Intensity',
  'ShortDescription': 'electric_field>AC'}]
```

To fetch data, call the get_cdf function with the requested date range and the names of the datasets and variables we want:

```python
import cdaweb
from datetime import datetime
data=get_cdf('sp_phys','UY_M0_PFRA',start_date=datetime(1990,10,29),end_date=datetime(1990,11,29),variables=['Intensity'])
```
