<img src="https://user-images.githubusercontent.com/1223300/41346080-c2c910a0-6f05-11e8-89e9-71a72bb9543f.png" width="300">

# sklearn-pmml-model

[![CircleCI](https://circleci.com/gh/iamDecode/sklearn-pmml-model.svg?style=shield)](https://circleci.com/gh/iamDecode/sklearn-pmml-model)
[![codecov](https://codecov.io/gh/iamDecode/sklearn-pmml-model/branch/master/graph/badge.svg?token=CGbbgziGwn)](https://codecov.io/gh/iamDecode/sklearn-pmml-model)

A library to parse PMML models into Scikit-learn estimators.

## Installation

The easiest way is to use pip:

```
$ pip install sklearn-pmml-model
```

## Status
This library is very alpha, and currently only supports a small part of the [specification](http://dmg.org/pmml/v4-3/GeneralStructure.html):
- DataDictionary
  - DataField (continuous, categorical, ordinal)
    - Value
    - Interval
- TransformationDictionary
  - DerivedField
- TreeModel
  - Node
    - SimplePredicate (eq, neq, lt, le, gt, ge)
    
For a minimum viable beta we like to at least add `SimpleSetPredicate` and support for all dataTypes (date, time, dateTime etc).

## Example
A minimal working example is shown below:

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
import pandas as pd
import numpy as np
from sklearn_pmml_model.tree import PMMLTreeClassifier

# Prepare data
iris = load_iris()

# We only take the two corresponding features
X = pd.DataFrame(iris.data)
X.columns = np.array(iris.feature_names)
y = pd.Series(np.array(iris.target_names)[iris.target])
y.name = "Class"

Xtr, Xte, ytr, yte = train_test_split(X, y, test_size=0.33, random_state=123)

df = pd.concat([Xte, yte], axis=1)

clf = PMMLTreeClassifier(pmml="models/sklearn2pmml.pmml")
clf.predict(Xte)
clf.score(Xte, yte)
```

## Development

### Prerequisites

Tests can be run using Py.test. Grab a local copy of the source:

```
$ git clone http://github.com/iamDecode/sklearn-pmml-model
```

create a virtual environment:
```
$ python3 -m venv venv
```

And install the dependencies:

```
$ pip install -r requirements.txt
```

### Testing

You can execute tests with py.test by running:
```
$ pytest tests/
```

## Contributing

Feel free to make a contribution. Please read [CONTRIBUTING.md]() for details on the code of conduct, and the process for submitting pull requests.

## License

This project is licensed under the BSD 2-Clause License - see the [LICENSE.md](LICENSE.md) file for details.
