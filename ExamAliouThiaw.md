![image bibliotheque polars](https://raw.githubusercontent.com/pola-rs/polars-static/master/logos/polars_github_logo_rect_dark_name.svg)

   **Aliou THIAW** 

 ##  **Table of Contents**

- Introduction:
- Presentation of Polars:
- What is Polars:
- Pratical example:
- Comparative analysis:
- Polars alternative:
- Use case:
- Conclusion:

# Introduction

Most Data Scientists/Analysts using Python are familiar with Pandas. And if you're into data science, you've probably spent a lot of time learning how to use them to manipulate your data. However, one of the main complaints about Pandas is its speed and inefficiency when dealing with large datasets. Fortunately, there is a new dataframe library that tries to address this main complaint about Pandas — Polars.

Polars is a DataFrame library written entirely in Rust. In this article, I will explain the basics of Polars and how it can be used instead of Pandas. In the following articles, I will dive into the details of the different Polars features.
# Presentation of Polars

The best way to understand **Polars** is that it is a better data frame library than Pandas. Here are some advantages of Polars over Pandas:

Polars does not use an index for the data frame. Eliminating the index makes it much easier to manipulate the dataframe (the index is mostly redundant in the Pandas dataframe anyway).
**Polars** represents data internally using Apache Arrow arrays, while Pandas stores data internally using NumPy arrays. Apache Arrow arrays are much more efficient in areas such as load time, memory usage, and compute.
Polars supports more parallel operations than Pandas. As **Polars** is written in Rust, it can run many operations in parallel.
**Polars** supports lazy evaluation. Based on your query, Polars will examine your queries, optimize them and look for ways to speed up the query or reduce memory usage. Pandas, on the other hand, only supports impatient evaluation, which immediately evaluates an expression as soon as it encounters one.

# What is Polars

Polars is a Python library for data manipulation and analysis, particularly designed for working with large and complex datasets. It provides a DataFrame object similar to Pandas, but with several additional features optimized for performance and memory efficiency.

Polars supports a wide range of data manipulation tasks, such as filtering, sorting, grouping, joining, aggregating, and pivoting data. It also supports a variety of data types, including numerical, categorical, date/time, and string data.

Polars is particularly well-suited for working with data in parallel, as it leverages multi-threading and multi-processing capabilities to speed up computation. It also supports distributed computing through the Ray framework, enabling distributed processing across multiple machines.

Overall, Polars provides a powerful and efficient tool for working with large and complex datasets in Python.

***Install and Import Polars***

To install polars, you can use the following command in your terminal or command prompt:

```python
pip install polars
```
Once you have installed polars, you can import it into your Python code using the following command:
```python
import polars as pl
``` 
This will allow you to use polars in your Python code. :+1:

# Pratical example
There are many datasets available for crime fiction analysis. Here are some examples:

**Kaggle**: Kaggle is a popular platform for data analysis. It offers many crime fiction datasets, including data on crime novels, crime TV shows, and crime movies. You can explore the data on Kaggle and download it to use in your projects.

**Goodreads**: Goodreads is a readers' reference website, which allows users to discover new books and share their opinions on the books they have read. The site has an extensive database of crime novels, which can be downloaded for analysis.

Project Gutenberg: Project Gutenberg is a free digital library of e-books. It offers a collection of classic detective books that can be downloaded for analysis.

**IMDb**: IMDb is an online database of movies, TV series, video games and entertainment. It offers many crime movie datasets, which can be used for analysis.

`nltk`: `nltk` is a Python library for natural language processing. It offers pre-processed text corpora, including crime novels, such as the Sherlock Holmes Corpus, which can be used to train text classification or generation models.
```python
pip install nltk
```

***Polars Expressions***

The following is an expression:
```python
pl.col("foo").sort().head(2)
```
The snippet above says:

1. Select column "foo"
2. Then sort the column (not in reversed order)
3. Then take the first two values of the sorted output

The power of expressions is that every expression produces a new expression, and that they can be piped together. You can run an expression by passing them to one of `Polars` execution contexts.

Here we run two expressions by running `df.select`:

```python
df.select([
    pl.col("foo").sort().head(2),
    pl.col("bar").filter(pl.col("foo") == 1).sum()
])
```

In this section we will go through some examples, but first let's create a dataset:

```python
import polars as pl
import numpy as np

np.random.seed(12)

df = pl.DataFrame(
    {
        "nrs": [1, 2, 3, None, 5],
        "names": ["foo", "ham", "spam", "egg", None],
        "random": np.random.rand(5),
        "groups": ["A", "A", "B", "C", "B"],
    }
)
print(df)
```
Result
```python
shape: (5, 4)
┌──────┬───────┬──────────┬────────┐
│ nrs  ┆ names ┆ random   ┆ groups │
│ ---  ┆ ---   ┆ ---      ┆ ---    │
│ i64  ┆ str   ┆ f64      ┆ str    │
╞══════╪═══════╪══════════╪════════╡
│ 1    ┆ foo   ┆ 0.154163 ┆ A      │
│ 2    ┆ ham   ┆ 0.74005  ┆ A      │
│ 3    ┆ spam  ┆ 0.263315 ┆ B      │
│ null ┆ egg   ┆ 0.533739 ┆ C      │
│ 5    ┆ null  ┆ 0.014575 ┆ B      │
└──────┴───────┴──────────┴────────┘
```
Count the number of rows in the group:

short form: 

```python
pl.count("party")
```
full form: 
```python
pl.col("party").count()
```
Get the first value of column in the group:

short form: 

```python
pl.first("first_element")
```

full form: 

```python
pl.col("last_name").first()
```
***Numpy interop***

`Polars` expressions support `NumPy`. See [here](https://numpy.org/doc/stable/reference/ufuncs.html#available-ufuncs) for a list of all supported numpy functions.

```python
import polars as pl
import numpy as np

df = pl.DataFrame({"a": [1, 2, 3], "b": [4, 5, 6]})

out = df.select(
    [
        np.log(pl.all()).suffix("_log"),
    ]
)
print(out)
```
Result

```python
shape: (3, 2)
┌──────────┬──────────┐
│ a_log    ┆ b_log    │
│ ---      ┆ ---      │
│ f64      ┆ f64      │
╞══════════╪══════════╡
│ 0.0      ┆ 1.386294 │
│ 0.693147 ┆ 1.609438 │
│ 1.098612 ┆ 1.791759 │
└──────────┴──────────┘
```

# Comparative analysis











































