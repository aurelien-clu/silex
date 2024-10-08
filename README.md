# Silex

Add more 🔥 to [Apache Spark](https://spark.apache.org/)!

[![CI](https://img.shields.io/github/workflow/status/aurelien-clu/silex/CI?style=flat-square)](https://github.com/aurelien-clu/silex/actions)
[![Pypi](https://img.shields.io/pypi/v/spark-silex?style=flat-square)](https://pypi.org/project/spark-silex/)
[![License](https://img.shields.io/github/license/aurelien-clu/silex?style=flat-square)](https://github.com/aurelien-clu/silex/blob/main/LICENSE)
[![Python](https://img.shields.io/badge/Python_3.8|3.9|3.10-Python?style=flat-square&logo=Python)](https://www.python.org/downloads/release/python-380/)
![Apache Spark](https://img.shields.io/static/v1?style=flat-square&message=Apache+Spark+%20+3&color=E25A1C&logo=Apache+Spark&logoColor=FFFFFF&label=)

[![Manager: Poetry](https://img.shields.io/badge/Manager-Poetry-blue?style=flat-square)](https://python-poetry.org/)
[![Test: BDD](https://img.shields.io/badge/Test-BDD-critical?style=flat-square)](https://github.com/behave/behave)
[![Test: Doctest](https://img.shields.io/badge/Test-Doctest-success?style=flat-square)](https://docs.python.org/3/library/doctest.html)
[![Code style: Black](https://img.shields.io/badge/Codestyle-Black-black?style=flat-square)](https://github.com/psf/black)
[![Imports: isort](https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat-square&labelColor=ef8336)](https://pycqa.github.io/isort/)
[![Linter: Flake8](https://img.shields.io/badge/Linter-Flake8-black?style=flat-square)](https://github.com/PyCQA/flake8)
[![try/except style: tryceratops](https://img.shields.io/badge/try%2Fexcept%20style-tryceratops%20%F0%9F%A6%96%E2%9C%A8-black?style=flat-square)](https://github.com/guilatrova/tryceratops)
[![Typing: MyPy](https://img.shields.io/badge/Typing-MyPy-blue?style=flat-square)](https://github.com/python/mypy)
[![Security: Bandit](https://img.shields.io/badge/Security-Bandit-critical?style=flat-square)](https://github.com/PyCQA/bandit)
[![Git: Pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&style=flat-square&logoColor=white)](https://pre-commit.com/)
[![Git: Conventional](https://img.shields.io/badge/Git-conventional-ff69b4?style=flat-square)](https://www.conventionalcommits.org)
[![Version: Semantic](https://img.shields.io/badge/Version-Semantic-black?style=flat-square)](https://semver.org/)

## TLDR

Silex is a Data Engineering library to extend PySpark.

You don't need another class, just use PySpark as usual and you have new functions to your DataFrames!

```shell
pip install spark-silex
```

```python
import silex
from pyspark.sql import DataFrame

# extends your DataFrame with silex functions!
# if for some reason you don't want to do that, check 'Without extending Dataframes' README section below
silex.extend_dataframes()

df: DataFrame = ...  # your regular Spark DataFrame
df: DataFrame = df.drop_col_if_na()  # new function! and still a regular Spark Dataframe!
# scroll for more information!
```

### TODO

- [ ] IDE auto-completions
- [ ] show-off coverage
- [ ] more documentation and examples
- [ ] more functions

### Available functions

```python
# assertions => raises an Exception if not met /!\
def expect_column(self, col: str) -> DataFrame: ...
def expect_columns(self, cols: Union[str, List[str]]) -> DataFrame: ...

def expect_distinct_values_equal_set(self, cols: Union[str, List[str]], values: Collection[Any]) -> DataFrame: ...
def expect_distinct_values_in_set(self, cols: Union[str, List[str]], values: Collection[Any]) -> DataFrame: ...

def expect_min_value_between(self, cols: Union[str, List[str]], min: Any, max: Any) -> DataFrame: ...
def expect_avg_value_between(self, cols: Union[str, List[str]], min: Any, max: Any) -> DataFrame: ...
def expect_max_value_between(self, cols: Union[str, List[str]], min: Any, max: Any) -> DataFrame: ...

def expect_unique_id(self, cols: Union[str, List[str]]) -> DataFrame: ...

# boolean checks
def has_column(self, col: str) -> bool: ...
def has_columns(self, cols: Union[str, List[str]]) -> bool: ...

def has_distinct_values_equal_set(self, cols: Union[str, List[str]], values: Collection[Any]) -> bool: ...
def has_distinct_values_in_set(self, cols: Union[str, List[str]], values: Collection[Any]) -> bool: ...

def has_min_value_between(self, cols: Union[str, List[str]], min: Any, max: Any) -> bool: ...
def has_avg_value_between(self, cols: Union[str, List[str]], min: Any, max: Any) -> bool: ...
def has_max_value_between(self, cols: Union[str, List[str]], min: Any, max: Any) -> bool: ...

def has_unique_id(self, cols: Union[str, List[str]]) -> bool: ...

# dates
def with_date_column(self, col: str, fmt: str, new_col: Optional[str] = None) -> DataFrame: ...

# drop
def drop_col_if_na(self, max: int) -> DataFrame: ...
def drop_col_if_not_distinct(self, min: int) -> DataFrame: ...

# filters
def filter_on_range(self, col: str, from_: Any, to: Any, ...) -> DataFrame: ...

# joins
def join_closest_date(self, other: DataFrame, ...) -> DataFrame: ...
```

## Getting started

### Pre-requisites

- Python 3.8 or above
- Spark 3 or above

### Installation

```shell
pip install spark-silex
```

### Usage

[examples/](examples/)

#### By extending DataFrames! ⚡

```python
import silex
from pyspark.sql import DataFrame, SparkSession

# extends your DataFrame with silex functions!
# if for some reason you don't want to do that, check next example
silex.extend_dataframes()

spark = SparkSession.builder.getOrCreate()

data = [
    (0, "01/29/2022", "a", 1.0),
    (1, "01/30/2022", "b", 2.0),
    (2, "01/31/2022", "c", 3.0),
]
df: DataFrame = spark.createDataFrame(data, schema=["id", "date_US", "text", "value"])
df.show()
# +---+----------+----+-----+
# | id|   date_US|text|value|
# +---+----------+----+-----+
# |  0|01/29/2022|   a|  1.0|
# |  1|01/30/2022|   b|  2.0|
# |  2|01/31/2022|   c|  3.0|
# +---+----------+----+-----+

df = df.with_date_column(col="date_US", new_col="date", fmt="MM/dd/yyyy")
df.show()
df.printSchema()
# +---+----------+----+-----+----------+
# | id|   date_US|text|value|      date|
# +---+----------+----+-----+----------+
# |  0|01/29/2022|   a|  1.0|2022-01-29|
# |  1|01/30/2022|   b|  2.0|2022-01-30|
# |  2|01/31/2022|   c|  3.0|2022-01-31|
# +---+----------+----+-----+----------+
#
# root
#  |-- id: long (nullable = true)
#  |-- date_US: string (nullable = true)
#  |-- text: string (nullable = true)
#  |-- value: double (nullable = true)
#  |-- date: date (nullable = true)

```

#### Without extending Dataframes 🌧️

```python
from silex.fn.date import with_date_column

# same as before except you need to provide the DataFrame
df = with_date_column(df=df, col="date_US", new_col="date", fmt="MM/dd/yyyy")
```

## Contributing

```shell
# install poetry and python 3.8, using pyenv for instance

cd silex
poetry env use path/to/python3.8  # e.g. ~/.pyenv/versions/3.8.12/bin/python
poetry shell
poetry install
pre-commit install

make help
# or open Makefile to learn about available commands for development
```
