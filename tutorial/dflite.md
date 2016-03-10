---
title: DFLite - Easy DataFrames in Python
layout: page
---

The data frame was a concept I first came across in R, where it is a fundemental component of data analysis. Never having done much data analysis in Python, I came across a situation where I needed a data frame but didn't know about the `pandas` implementation, so I went about writing my own `DataFrame` class. When I realized the `pandas` version existed, I immediately switched all my code over only to find that for my application, **the pandas DataFrame was over 2 times slower** than my lightweight `DataFrame`. So I spent some time making sure the interfaces were the same (at least in what I was doing) and made some nice 'this is what you would expect' modifications for personal use in the future.

In general, the `pandas` data frame performs quite well, especially with large datasets. I'm sure that this class has quite a few holes in it, but the idea of a lightweight `DataFrame` for Python is worth pursuing in the future. You can find the [source code for `dflite`](https://github.com/paleolimbot/dflite) on GitHub, including a copy of this notebook. Here's some general usage.

## Importing

Importing is easy, the only dependency is `numpy`.


```python
import dflite as df
```

## Creating a DataFrame

Usually all I want to do is create a `DataFrame` from a CSV file, but in code there's a couple of other ways to construct the class. The `DataFrame.from_records()` method is probably the most useful, creating a `DataFrame` from an iterable grouped by record (items coming out of a Postgres database via `psycopg2` are a good example). This is more or less equivalent to the `pandas` method of the same name. For now, we'll demo the class with the small CSV included in the directory.


```python
data = df.read_csv("test.csv")
data.head()
```




<table><tr><td></td><td><strong>Time (UTC)</strong></td><td><strong>Latitude</strong></td><td><strong>Longitude</strong></td></tr>
<tr><td><strong>0</strong></td><td>2016-03-02 17:50:18</td><td>45.10303743</td><td>-64.29103034</td></tr>
<tr><td><strong>1</strong></td><td>2016-03-02 17:50:19</td><td>45.10291441</td><td>-64.29095464</td></tr>
<tr><td><strong>2</strong></td><td>2016-03-02 17:50:20</td><td>45.10279595</td><td>-64.29089237</td></tr>
<tr><td><strong>3</strong></td><td>2016-03-02 17:50:21</td><td>45.1026838</td><td>-64.29084603</td></tr>
<tr><td><strong>4</strong></td><td>2016-03-02 17:50:22</td><td>45.10259138</td><td>-64.29080328</td></tr>
<tr><td><strong>5</strong></td><td>2016-03-02 17:50:23</td><td>45.10251977</td><td>-64.29080362</td></tr></table>



The `read_csv()` function is pretty much the same as the `pandas` version, at least for simple usage. You can also pass in a file-like object and a `driver=` parameter. Currently only `csv` files are supported, but they're the most common, so hey.

The `tail()` method works much the same as the `head()` method.


```python
data.tail(3)
```




<table><tr><td></td><td><strong>Time (UTC)</strong></td><td><strong>Latitude</strong></td><td><strong>Longitude</strong></td></tr>
<tr><td><strong>0</strong></td><td>2016-03-02 17:50:54</td><td>45.09937809</td><td>-64.29696471</td></tr>
<tr><td><strong>1</strong></td><td>2016-03-02 17:50:55</td><td>45.09924766</td><td>-64.2972626</td></tr>
<tr><td><strong>2</strong></td><td>2016-03-02 17:50:56</td><td>45.09911724</td><td>-64.29754859</td></tr></table>



Notice here how the indicies are 0, 1, and 2, where they *should* be the last few indicies of the `DataFrame`. The `pandas.DataFrame` supports (I would argue is slightly obsessed with) thd idea of **index**es for rows/columns. For rows I almost never have a reason to access them by anything other than an integer (certainly not a string), and because of this I don't bother with them. Since `tail()` is basically just checking the `DataFrame`, I didn't bother to re-number the rows (`tail()` is actually just shorthand for `data.iloc[(len(data)-nrows):len(data)])`, so it's really its own `DataFrame` object).

## Columns

Column names can be accessed and set just like the `pandas` version:


```python
data.columns
```




    ['Time (UTC)', 'Latitude', 'Longitude']




```python
data.columns = ("col1", "col2", "col3")
data.head()
```




<table><tr><td></td><td><strong>col1</strong></td><td><strong>col2</strong></td><td><strong>col3</strong></td></tr>
<tr><td><strong>0</strong></td><td>2016-03-02 17:50:18</td><td>45.10303743</td><td>-64.29103034</td></tr>
<tr><td><strong>1</strong></td><td>2016-03-02 17:50:19</td><td>45.10291441</td><td>-64.29095464</td></tr>
<tr><td><strong>2</strong></td><td>2016-03-02 17:50:20</td><td>45.10279595</td><td>-64.29089237</td></tr>
<tr><td><strong>3</strong></td><td>2016-03-02 17:50:21</td><td>45.1026838</td><td>-64.29084603</td></tr>
<tr><td><strong>4</strong></td><td>2016-03-02 17:50:22</td><td>45.10259138</td><td>-64.29080328</td></tr>
<tr><td><strong>5</strong></td><td>2016-03-02 17:50:23</td><td>45.10251977</td><td>-64.29080362</td></tr></table>



Column values can be added and removed in a similar way


```python
data["col1"]
```




    array(['2016-03-02 17:50:18', '2016-03-02 17:50:19', '2016-03-02 17:50:20',
           '2016-03-02 17:50:21', '2016-03-02 17:50:22', '2016-03-02 17:50:23',
           '2016-03-02 17:50:24', '2016-03-02 17:50:25', '2016-03-02 17:50:26',
           '2016-03-02 17:50:27', '2016-03-02 17:50:28', '2016-03-02 17:50:29',
           '2016-03-02 17:50:30', '2016-03-02 17:50:31', '2016-03-02 17:50:32',
           '2016-03-02 17:50:33', '2016-03-02 17:50:34', '2016-03-02 17:50:35',
           '2016-03-02 17:50:36', '2016-03-02 17:50:37', '2016-03-02 17:50:38',
           '2016-03-02 17:50:39', '2016-03-02 17:50:40', '2016-03-02 17:50:41',
           '2016-03-02 17:50:42', '2016-03-02 17:50:43', '2016-03-02 17:50:44',
           '2016-03-02 17:50:45', '2016-03-02 17:50:46', '2016-03-02 17:50:47',
           '2016-03-02 17:50:48', '2016-03-02 17:50:49', '2016-03-02 17:50:50',
           '2016-03-02 17:50:51', '2016-03-02 17:50:52', '2016-03-02 17:50:53',
           '2016-03-02 17:50:54', '2016-03-02 17:50:55', '2016-03-02 17:50:56'], 
          dtype='<U19')



Columns can also be accessed by index (this isn't possible in the `pandas` version, and I'm not quite sure why)


```python
data[0]
```




    array(['2016-03-02 17:50:18', '2016-03-02 17:50:19', '2016-03-02 17:50:20',
           '2016-03-02 17:50:21', '2016-03-02 17:50:22', '2016-03-02 17:50:23',
           '2016-03-02 17:50:24', '2016-03-02 17:50:25', '2016-03-02 17:50:26',
           '2016-03-02 17:50:27', '2016-03-02 17:50:28', '2016-03-02 17:50:29',
           '2016-03-02 17:50:30', '2016-03-02 17:50:31', '2016-03-02 17:50:32',
           '2016-03-02 17:50:33', '2016-03-02 17:50:34', '2016-03-02 17:50:35',
           '2016-03-02 17:50:36', '2016-03-02 17:50:37', '2016-03-02 17:50:38',
           '2016-03-02 17:50:39', '2016-03-02 17:50:40', '2016-03-02 17:50:41',
           '2016-03-02 17:50:42', '2016-03-02 17:50:43', '2016-03-02 17:50:44',
           '2016-03-02 17:50:45', '2016-03-02 17:50:46', '2016-03-02 17:50:47',
           '2016-03-02 17:50:48', '2016-03-02 17:50:49', '2016-03-02 17:50:50',
           '2016-03-02 17:50:51', '2016-03-02 17:50:52', '2016-03-02 17:50:53',
           '2016-03-02 17:50:54', '2016-03-02 17:50:55', '2016-03-02 17:50:56'], 
          dtype='<U19')




```python
data["newcol"] = 10
data.head()
```




<table><tr><td></td><td><strong>col1</strong></td><td><strong>col2</strong></td><td><strong>col3</strong></td><td><strong>newcol</strong></td></tr>
<tr><td><strong>0</strong></td><td>2016-03-02 17:50:18</td><td>45.10303743</td><td>-64.29103034</td><td>10</td></tr>
<tr><td><strong>1</strong></td><td>2016-03-02 17:50:19</td><td>45.10291441</td><td>-64.29095464</td><td>10</td></tr>
<tr><td><strong>2</strong></td><td>2016-03-02 17:50:20</td><td>45.10279595</td><td>-64.29089237</td><td>10</td></tr>
<tr><td><strong>3</strong></td><td>2016-03-02 17:50:21</td><td>45.1026838</td><td>-64.29084603</td><td>10</td></tr>
<tr><td><strong>4</strong></td><td>2016-03-02 17:50:22</td><td>45.10259138</td><td>-64.29080328</td><td>10</td></tr>
<tr><td><strong>5</strong></td><td>2016-03-02 17:50:23</td><td>45.10251977</td><td>-64.29080362</td><td>10</td></tr></table>




```python
data["newcol"] = data["newcol"] + 4
data.head()
```




<table><tr><td></td><td><strong>col1</strong></td><td><strong>col2</strong></td><td><strong>col3</strong></td><td><strong>newcol</strong></td></tr>
<tr><td><strong>0</strong></td><td>2016-03-02 17:50:18</td><td>45.10303743</td><td>-64.29103034</td><td>14</td></tr>
<tr><td><strong>1</strong></td><td>2016-03-02 17:50:19</td><td>45.10291441</td><td>-64.29095464</td><td>14</td></tr>
<tr><td><strong>2</strong></td><td>2016-03-02 17:50:20</td><td>45.10279595</td><td>-64.29089237</td><td>14</td></tr>
<tr><td><strong>3</strong></td><td>2016-03-02 17:50:21</td><td>45.1026838</td><td>-64.29084603</td><td>14</td></tr>
<tr><td><strong>4</strong></td><td>2016-03-02 17:50:22</td><td>45.10259138</td><td>-64.29080328</td><td>14</td></tr>
<tr><td><strong>5</strong></td><td>2016-03-02 17:50:23</td><td>45.10251977</td><td>-64.29080362</td><td>14</td></tr></table>




```python
del data["newcol"]
data.head()
```




<table><tr><td></td><td><strong>col1</strong></td><td><strong>col2</strong></td><td><strong>col3</strong></td></tr>
<tr><td><strong>0</strong></td><td>2016-03-02 17:50:18</td><td>45.10303743</td><td>-64.29103034</td></tr>
<tr><td><strong>1</strong></td><td>2016-03-02 17:50:19</td><td>45.10291441</td><td>-64.29095464</td></tr>
<tr><td><strong>2</strong></td><td>2016-03-02 17:50:20</td><td>45.10279595</td><td>-64.29089237</td></tr>
<tr><td><strong>3</strong></td><td>2016-03-02 17:50:21</td><td>45.1026838</td><td>-64.29084603</td></tr>
<tr><td><strong>4</strong></td><td>2016-03-02 17:50:22</td><td>45.10259138</td><td>-64.29080328</td></tr>
<tr><td><strong>5</strong></td><td>2016-03-02 17:50:23</td><td>45.10251977</td><td>-64.29080362</td></tr></table>



## Rows

In `pandas`, rows are accessed through the `iloc` attribute, so after considerable changing of code, so does mine. Here, `data.iloc[3]` will give the fourth row (as a `dict` ish object), and `data.iloc[3, :]` will give a `DataFrame` with only one row. The `pandas` version also has a `loc[]` option where names can be specified, but in this implementation `iloc` and `loc` are identical, and so you can pass more or less anything between the brackets and get a sensible result. 


```python
row = data.iloc[3]
row
```




<table><tr><td><strong>col1</strong></td><td><strong>col2</strong></td><td><strong>col3</strong></td></tr><tr><td>2016-03-02 17:50:21</td><td>45.1026838</td><td>-64.29084603</td></tr></table>



Of course, I've made the nice `_repr_html_()` method so it displas nicely but each row is actually a `_DFRow` object, which is a subclass of `dict` that keeps its values in order. This means you can index it by column name or by index.


```python
row["col1"]
```




    '2016-03-02 17:50:21'




```python
row[0]
```




    '2016-03-02 17:50:21'



Iterating through rows is done using the `itertuples()` method, which returns an iterator that iterates through the rows in the same way as the `pandas` version. Because `pandas` returns its row with the `0`th item as the row number (or row *index*, if you believe in that kind of thing), this method does as well.


```python
for row in data.head().itertuples():
    print(row[0], row["col2"], row[1])
```

    0 45.10303743 2016-03-02 17:50:18
    1 45.10291441 2016-03-02 17:50:19
    2 45.10279595 2016-03-02 17:50:20
    3 45.1026838 2016-03-02 17:50:21
    4 45.10259138 2016-03-02 17:50:22
    5 45.10251977 2016-03-02 17:50:23


## Subsetting

Each column is a NumPy `ndarray` object, so it can be indexed like any other `ndarray` object (i.e. by a `list` of desired rows, by an `ndarray` of logicals, by a single index, or by a `slice`). Some of this notation is available in the `iloc` method as well, which returns a single value (if two `int`s are passed), a `_DFRow` (if only a single integer is passed), or a subsetted `DataFrame` (if some combination of slices/ints/lists is passed). See the following examples:


```python
data.iloc[1:3]
```




<table><tr><td></td><td><strong>col1</strong></td><td><strong>col2</strong></td><td><strong>col3</strong></td></tr>
<tr><td><strong>0</strong></td><td>2016-03-02 17:50:19</td><td>45.10291441</td><td>-64.29095464</td></tr>
<tr><td><strong>1</strong></td><td>2016-03-02 17:50:20</td><td>45.10279595</td><td>-64.29089237</td></tr></table>




```python
data.iloc[1:5, 0:2]
```




<table><tr><td></td><td><strong>col1</strong></td><td><strong>col2</strong></td></tr>
<tr><td><strong>0</strong></td><td>2016-03-02 17:50:19</td><td>45.10291441</td></tr>
<tr><td><strong>1</strong></td><td>2016-03-02 17:50:20</td><td>45.10279595</td></tr>
<tr><td><strong>2</strong></td><td>2016-03-02 17:50:21</td><td>45.1026838</td></tr>
<tr><td><strong>3</strong></td><td>2016-03-02 17:50:22</td><td>45.10259138</td></tr></table>




```python
data.iloc[[2, 5, 5, 33], ("col1", "col3")]
```




<table><tr><td></td><td><strong>col1</strong></td><td><strong>col3</strong></td></tr>
<tr><td><strong>0</strong></td><td>2016-03-02 17:50:20</td><td>-64.29089237</td></tr>
<tr><td><strong>1</strong></td><td>2016-03-02 17:50:23</td><td>-64.29080362</td></tr>
<tr><td><strong>2</strong></td><td>2016-03-02 17:50:23</td><td>-64.29080362</td></tr>
<tr><td><strong>3</strong></td><td>2016-03-02 17:50:51</td><td>-64.29613321</td></tr></table>



Notice again how our original row number aren't preserved. You can work around this by making a column with your original row numbers. I get how this could be annoying, but including it was too complicated and wasn't necessary for what I was doing.


```python
data["original_rows"] = list(range(len(data)))
data.iloc[[2, 5, 5, 33], ("original_rows", "col1", "col3")]
```




<table><tr><td></td><td><strong>original_rows</strong></td><td><strong>col1</strong></td><td><strong>col3</strong></td></tr>
<tr><td><strong>0</strong></td><td>2</td><td>2016-03-02 17:50:20</td><td>-64.29089237</td></tr>
<tr><td><strong>1</strong></td><td>5</td><td>2016-03-02 17:50:23</td><td>-64.29080362</td></tr>
<tr><td><strong>2</strong></td><td>5</td><td>2016-03-02 17:50:23</td><td>-64.29080362</td></tr>
<tr><td><strong>3</strong></td><td>33</td><td>2016-03-02 17:50:51</td><td>-64.29613321</td></tr></table>



All of the nice indexing things we can do with NumPy are also available in the 'rows' part of the index:


```python
data.iloc[data["col2"] > 45.1022]
```




<table><tr><td></td><td><strong>col1</strong></td><td><strong>col2</strong></td><td><strong>col3</strong></td><td><strong>original_rows</strong></td></tr>
<tr><td><strong>0</strong></td><td>2016-03-02 17:50:18</td><td>45.10303743</td><td>-64.29103034</td><td>0</td></tr>
<tr><td><strong>1</strong></td><td>2016-03-02 17:50:19</td><td>45.10291441</td><td>-64.29095464</td><td>1</td></tr>
<tr><td><strong>2</strong></td><td>2016-03-02 17:50:20</td><td>45.10279595</td><td>-64.29089237</td><td>2</td></tr>
<tr><td><strong>3</strong></td><td>2016-03-02 17:50:21</td><td>45.1026838</td><td>-64.29084603</td><td>3</td></tr>
<tr><td><strong>4</strong></td><td>2016-03-02 17:50:22</td><td>45.10259138</td><td>-64.29080328</td><td>4</td></tr>
<tr><td><strong>5</strong></td><td>2016-03-02 17:50:23</td><td>45.10251977</td><td>-64.29080362</td><td>5</td></tr>
<tr><td><strong>6</strong></td><td>2016-03-02 17:50:24</td><td>45.10245523</td><td>-64.29083152</td><td>6</td></tr>
<tr><td><strong>7</strong></td><td>2016-03-02 17:50:25</td><td>45.10240112</td><td>-64.29086638</td><td>7</td></tr>
<tr><td><strong>8</strong></td><td>2016-03-02 17:50:26</td><td>45.10233343</td><td>-64.2909347</td><td>8</td></tr>
<tr><td><strong>9</strong></td><td>2016-03-02 17:50:27</td><td>45.10227411</td><td>-64.29102036</td><td>9</td></tr>
<tr><td><strong>10</strong></td><td>2016-03-02 17:50:28</td><td>45.1022154</td><td>-64.29111654</td><td>10</td></tr></table>



## Exporting

Writing the `DataFrame` to a CSV is probably the easiest way to export, although TSV is also supported. The `to_csv()` method works more or less like the `pandas` version, and can take a file-like object as well as a filename.

## Performance

As I mentioned earlier, running the `pandas.DataFrame` in production code that used quite a lot of `DataFrame`s was quite slow. I have a feeling that there's a lot of overhead involved with the convenience of multiple indexing and built-in plotting support that slows the class down when there isn't a need for it. There's also probably a lot of work to be done on this class that can add convenience without comprimising performance, but I'll leave that up to some folks with a bit more spare time than I do. Cheers!
