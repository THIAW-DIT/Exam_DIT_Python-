![image bibliotheque polars](https://raw.githubusercontent.com/pola-rs/polars-static/master/logos/polars_github_logo_rect_dark_name.svg)

   
 ##  **Table of Contents**

- [Introduction](#introduction):

- [Presentation of Polars](#presentation-of-polars):

- [What is Polars](#what-is-polars):

- [Pratical example](#pratical-example):

- [Comparative analysis](#comparative-analysis):

- [Polars alternative](#polars-alternative):

- [ Use case](#use-case):

- [Conclusion](#conclusion):

- [Reference](#reference)

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

> *So far you have taken note* :writing_hand:

# Comparative analysis

This section explores the main aspects of how the Polars package differs from Pandas regarding syntax and execution time:

* Reading Data

* Selecting and Filtering Data

* Creating New Columns
 
* Grouping and Aggregation

* Missing Data

**Reading Data**

Reading a CSV file in Polars will feel familiar because you can use the `.read_csv()` method like in Pandas:

```python
# Pandas
pd.read_csv('example.csv')

# Polars
pl.read_csv('example.csv')
```
The resulting execution times to read the sample dataset in Pandas and Polars are shown below:

![time of excute](https://miro.medium.com/v2/resize:fit:640/format:webp/0*yXjcvIcyJK7OxZOU.png)

**Selecting and Filtering Data**

The first major difference between Pandas and Polars is that Polars does not use an index. Instead, each row is indexed by its integer position in the DataFrame.

Although the same Pandas code will run with Polars, it is not the best practice. In Polars, you should use the .select() method to select data.

```python
# Pandas
df[['col1', 'col2']] 

# The above code will run with Polars as well, 
# but the correct way in Polars is:
df.select(pl.col(['col1', 'col2']))
```
The resulting execution times to select data in Pandas and Polars are shown below.

![second img](https://miro.medium.com/v2/resize:fit:640/format:webp/0*rNvAISXVQNXxYPxP.png)

While you would use the `.query()` method in Pandas to filter data, you need to use the `.filter(`) method in Polars.

```python
# Pandas
df.query('col1 > 5')

# Polars
df.filter(pl.col('col') > 5)
```

The resulting execution times to filter data in Pandas and Polars are shown below.

![my third img](https://miro.medium.com/v2/resize:fit:640/format:webp/0*0NjTuXNhJVReBkea.png)

In contrast to Pandas, Polars can run operations in .select() and .filter() in parallel.

**Creating New Columns**

Creating a new column in Polars also differs from what you might be used to in Pandas. 
In Polars, you need to use the `.with_column()` or the `.with_columns()` method depending on how many columns you want to create.

```python
# Pandas
df_pd["new_col"] = df_pd["col"] * 10

# Polars
df.with_columns([(pl.col("col") * 10).alias("new_col")])

# Polars for multiple columns
# df.with_columns([(pl.col("col") * 10).alias("new_col"), ...])
```

**Grouping and Aggregation**

Grouping and aggregation are slightly different between Pandas and Polars syntax-wise, but both use the `.groupby()` and `.agg()` methods.

```python
# Pandas
df_pd.groupby('col1')['col2'].agg('mean')

# Polars
# df.groupby('col1').agg([pl.col('col2').mean()]) # As suggested in Polars docs
df.groupby('col1').agg([pl.mean('col2')]) # Shorter
```
**Missing Data**

Another major difference between Pandas and Polars is that Pandas uses `NaN` values to indicate missing values, while Polars uses `null`

Thus, instead of the `.fillna()` method in Pandas, you should use the `.fill_null()` method in Polars.

```python
# Pandas
df['col2'].fillna(-999)

# Polars
# df_pd.with_column(pl.col('col2').fill_null(pl.lit(-999))) # As suggested in Polars docs
df_pd.with_column(pl.col('col2').fill_null(-999)) # Shorter
```
***Conclusion***

The main advantage of Polars over Pandas is its speed. If you need to do a lot of data processing on large datasets, you should definitely try Polars.

The main advantage of Polars over Pandas is its speed.

# Polars alternative

The advantage of using polars over pandas is that nowadays we collect a lot of raw and uncleaned data for our data analysis. So before performing any analysis, we need to clean the data. For cleaning, we have to perform a lot of operations on data. Polars provides us with a fast and efficient framework for cleaning and operating on large data. By using polars we can easily clean a large raw data.

Polars is faster and more efficient than pandas. One of the significant problems with pandas is their slow speed and inefficiencies when dealing with larger datasets. The major difference between polars and pandas is speed.

Let us understand the speed difference between pandas and polars in reading a CSV file.

```python
#Data Loading
start = perf_counter()
df_pl = pl.read_csv("tmdb_5000_credits.csv")#not lazy mode
end = perf_counter()
print(f"Spent {round(end-start,2)}s.")
```

The above code gives the following output

```plaintext
Spent 0.4 s 
```

```python
start = perf_counter()
df_pd = pd.read_csv("tmdb_5000_credits.csv")
end = perf_counter()
print(f"Spent {round(end-start,2)}s.")
```

The above code gives the following output:

```plaintext
Spent 0.85s
```
# Use case

**Data reading**

You could do the same in polars via

```python
import polars as pl
import numpy as np
df = pl.read_csv("/content/drive/MyDrive/train.csv") 
df.head(5)
```
Check duplicate data

```python
df.filter(df.is_duplicated())

df.is_duplicated()
```

Data description

```python
print(df.describe())
```
Check for missing data

```python
print(df.null_count())
```
Replace missing data

```python
df.fill_null(99)

df.fill_null(strategy="forward")

df.fill_null(strategy="max")
shape: (4, 2)
┌─────┬──────┐
│ a   ┆ b    │
│ --- ┆ ---  │
│ i64 ┆ f64  │
╞═════╪══════╡
│ 1   ┆ 0.5  │
│ 2   ┆ 4.0  │
│ 4   ┆ 13.0 │
│ 4   ┆ 13.0 │
└─────┴──────┘

df.fill_null(strategy="zero")
shape: (4, 2)
┌─────┬──────┐
│ a   ┆ b    │
│ --- ┆ ---  │
│ i64 ┆ f64  │
╞═════╪══════╡
│ 1   ┆ 0.5  │
│ 2   ┆ 4.0  │
│ 0   ┆ 0.0  │
│ 4   ┆ 13.0 │
└─────┴──────┘
```
Convert categorical variables to dummy/indicator variables.

```python
df.to_dummies()
```

# Conclusion

In conclusion, whodunits in Python are an essential tool for data scientists and data analysts looking to explore and analyze complex data. Polars offer a clear and concise syntax for creating high-quality, interactive charts that effectively visualize data and detect hidden trends and patterns.

Polars are also easy to use and customize, offering a wide range of features to customize charts, including changing the visual appearance of charts, changing labels and colors, and creating layered charts. .

Finally, polars is a rapidly growing data visualization library, with frequent updates and enhancements, making it a solid choice for long-term data visualization projects.

# Reference

- https://pola-rs.github.io/polars/py-polars/html/reference/dataframe/api/polars.DataFrame.describe.html
- https://calmcode.io/polars/read-csv.html
- https://www.kaggle.com/datasets/hesh97/titanicdataset-traincsv
- https://docusaurus.io/fr/docs/markdown-features/links
- https://pola-rs.github.io/polars-book/user-guide/notebooks/introduction_polars-py.html
- https://towardsdatascience.com/pandas-vs-polars-a-syntax-and-speed-comparison-5aa54e27497e#0602
- https://anamikayadav.hashnode.dev/polars-python-library-a-fast-and-efficient-alternative-to-pandas


# Author: Aliou THIAW  
![myimage](map.png)







































