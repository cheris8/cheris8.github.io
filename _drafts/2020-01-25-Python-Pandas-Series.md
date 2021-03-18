


Series
Constructor
Series([data, index, dtype, name, copy, …])
One-dimensional ndarray with axis labels (including time series).
Attributes
Axes

Series.index
The index (axis labels) of the Series.
Series.array
The ExtensionArray of the data backing this Series or Index.
Series.values
Return Series as ndarray or ndarray-like depending on the dtype.
Series.dtype
Return the dtype object of the underlying data.
Series.shape
Return a tuple of the shape of the underlying data.
Series.nbytes
Return the number of bytes in the underlying data.
Series.ndim
Number of dimensions of the underlying data, by definition 1.
Series.size
Return the number of elements in the underlying data.
Series.T
Return the transpose, which is by definition self.
Series.memory_usage([index, deep])
Return the memory usage of the Series.
Series.hasnans
Return if I have any nans; enables various perf speedups.
Series.empty
Indicator whether DataFrame is empty.
Series.dtypes
Return the dtype object of the underlying data.
Series.name
Return the name of the Series.
Series.flags
Get the properties associated with this pandas object.
Series.set_flags(*[, copy, …])
Return a new object with updated flags.
Conversion
Series.astype(dtype[, copy, errors])
Cast a pandas object to a specified dtype dtype.
Series.convert_dtypes([infer_objects, …])
Convert columns to best possible dtypes using dtypes supporting pd.NA.
Series.infer_objects()
Attempt to infer better dtypes for object columns.
Series.copy([deep])
Make a copy of this object’s indices and data.
Series.bool()
Return the bool of a single element Series or DataFrame.
Series.to_numpy([dtype, copy, na_value])
A NumPy ndarray representing the values in this Series or Index.
Series.to_period([freq, copy])
Convert Series from DatetimeIndex to PeriodIndex.
Series.to_timestamp([freq, how, copy])
Cast to DatetimeIndex of Timestamps, at beginning of period.
Series.to_list()
Return a list of the values.
Series.__array__([dtype])
Return the values as a NumPy array.
Indexing, iteration
Series.get(key[, default])
Get item from object for given key (ex: DataFrame column).
Series.at
Access a single value for a row/column label pair.
Series.iat
Access a single value for a row/column pair by integer position.
Series.loc
Access a group of rows and columns by label(s) or a boolean array.
Series.iloc
Purely integer-location based indexing for selection by position.
Series.__iter__()
Return an iterator of the values.
Series.items()
Lazily iterate over (index, value) tuples.
Series.iteritems()
Lazily iterate over (index, value) tuples.
Series.keys()
Return alias for index.
Series.pop(item)
Return item and drops from series.
Series.item()
Return the first element of the underlying data as a Python scalar.
Series.xs(key[, axis, level, drop_level])
Return cross-section from the Series/DataFrame.
For more information on .at, .iat, .loc, and .iloc, see the indexing documentation.

Binary operator functions
Series.add(other[, level, fill_value, axis])
Return Addition of series and other, element-wise (binary operator add).
Series.sub(other[, level, fill_value, axis])
Return Subtraction of series and other, element-wise (binary operator sub).
Series.mul(other[, level, fill_value, axis])
Return Multiplication of series and other, element-wise (binary operator mul).
Series.div(other[, level, fill_value, axis])
Return Floating division of series and other, element-wise (binary operator truediv).
Series.truediv(other[, level, fill_value, axis])
Return Floating division of series and other, element-wise (binary operator truediv).
Series.floordiv(other[, level, fill_value, axis])
Return Integer division of series and other, element-wise (binary operator floordiv).
Series.mod(other[, level, fill_value, axis])
Return Modulo of series and other, element-wise (binary operator mod).
Series.pow(other[, level, fill_value, axis])
Return Exponential power of series and other, element-wise (binary operator pow).
Series.radd(other[, level, fill_value, axis])
Return Addition of series and other, element-wise (binary operator radd).
Series.rsub(other[, level, fill_value, axis])
Return Subtraction of series and other, element-wise (binary operator rsub).
Series.rmul(other[, level, fill_value, axis])
Return Multiplication of series and other, element-wise (binary operator rmul).
Series.rdiv(other[, level, fill_value, axis])
Return Floating division of series and other, element-wise (binary operator rtruediv).
Series.rtruediv(other[, level, fill_value, axis])
Return Floating division of series and other, element-wise (binary operator rtruediv).
Series.rfloordiv(other[, level, fill_value, …])
Return Integer division of series and other, element-wise (binary operator rfloordiv).
Series.rmod(other[, level, fill_value, axis])
Return Modulo of series and other, element-wise (binary operator rmod).
Series.rpow(other[, level, fill_value, axis])
Return Exponential power of series and other, element-wise (binary operator rpow).
Series.combine(other, func[, fill_value])
Combine the Series with a Series or scalar according to func.
Series.combine_first(other)
Combine Series values, choosing the calling Series’s values first.
Series.round([decimals])
Round each value in a Series to the given number of decimals.
Series.lt(other[, level, fill_value, axis])
Return Less than of series and other, element-wise (binary operator lt).
Series.gt(other[, level, fill_value, axis])
Return Greater than of series and other, element-wise (binary operator gt).
Series.le(other[, level, fill_value, axis])
Return Less than or equal to of series and other, element-wise (binary operator le).
Series.ge(other[, level, fill_value, axis])
Return Greater than or equal to of series and other, element-wise (binary operator ge).
Series.ne(other[, level, fill_value, axis])
Return Not equal to of series and other, element-wise (binary operator ne).
Series.eq(other[, level, fill_value, axis])
Return Equal to of series and other, element-wise (binary operator eq).
Series.product([axis, skipna, level, …])
Return the product of the values over the requested axis.
Series.dot(other)
Compute the dot product between the Series and the columns of other.
Function application, GroupBy & window
Series.apply(func[, convert_dtype, args])
Invoke function on values of Series.
Series.agg([func, axis])
Aggregate using one or more operations over the specified axis.
Series.aggregate([func, axis])
Aggregate using one or more operations over the specified axis.
Series.transform(func[, axis])
Call func on self producing a Series with transformed values.
Series.map(arg[, na_action])
Map values of Series according to input correspondence.
Series.groupby([by, axis, level, as_index, …])
Group Series using a mapper or by a Series of columns.
Series.rolling(window[, min_periods, …])
Provide rolling window calculations.
Series.expanding([min_periods, center, axis])
Provide expanding transformations.
Series.ewm([com, span, halflife, alpha, …])
Provide exponential weighted (EW) functions.
Series.pipe(func, *args, **kwargs)
Apply func(self, *args, **kwargs).
Computations / descriptive stats
Series.abs()
Return a Series/DataFrame with absolute numeric value of each element.
Series.all([axis, bool_only, skipna, level])
Return whether all elements are True, potentially over an axis.
Series.any([axis, bool_only, skipna, level])
Return whether any element is True, potentially over an axis.
Series.autocorr([lag])
Compute the lag-N autocorrelation.
Series.between(left, right[, inclusive])
Return boolean Series equivalent to left <= series <= right.
Series.clip([lower, upper, axis, inplace])
Trim values at input threshold(s).
Series.corr(other[, method, min_periods])
Compute correlation with other Series, excluding missing values.
Series.count([level])
Return number of non-NA/null observations in the Series.
Series.cov(other[, min_periods, ddof])
Compute covariance with Series, excluding missing values.
Series.cummax([axis, skipna])
Return cumulative maximum over a DataFrame or Series axis.
Series.cummin([axis, skipna])
Return cumulative minimum over a DataFrame or Series axis.
Series.cumprod([axis, skipna])
Return cumulative product over a DataFrame or Series axis.
Series.cumsum([axis, skipna])
Return cumulative sum over a DataFrame or Series axis.
Series.describe([percentiles, include, …])
Generate descriptive statistics.
Series.diff([periods])
First discrete difference of element.
Series.factorize([sort, na_sentinel])
Encode the object as an enumerated type or categorical variable.
Series.kurt([axis, skipna, level, numeric_only])
Return unbiased kurtosis over requested axis.
Series.mad([axis, skipna, level])
Return the mean absolute deviation of the values over the requested axis.
Series.max([axis, skipna, level, numeric_only])
Return the maximum of the values over the requested axis.
Series.mean([axis, skipna, level, numeric_only])
Return the mean of the values over the requested axis.
Series.median([axis, skipna, level, …])
Return the median of the values over the requested axis.
Series.min([axis, skipna, level, numeric_only])
Return the minimum of the values over the requested axis.
Series.mode([dropna])
Return the mode(s) of the Series.
Series.nlargest([n, keep])
Return the largest n elements.
Series.nsmallest([n, keep])
Return the smallest n elements.
Series.pct_change([periods, fill_method, …])
Percentage change between the current and a prior element.
Series.prod([axis, skipna, level, …])
Return the product of the values over the requested axis.
Series.quantile([q, interpolation])
Return value at the given quantile.
Series.rank([axis, method, numeric_only, …])
Compute numerical data ranks (1 through n) along axis.
Series.sem([axis, skipna, level, ddof, …])
Return unbiased standard error of the mean over requested axis.
Series.skew([axis, skipna, level, numeric_only])
Return unbiased skew over requested axis.
Series.std([axis, skipna, level, ddof, …])
Return sample standard deviation over requested axis.
Series.sum([axis, skipna, level, …])
Return the sum of the values over the requested axis.
Series.var([axis, skipna, level, ddof, …])
Return unbiased variance over requested axis.
Series.kurtosis([axis, skipna, level, …])
Return unbiased kurtosis over requested axis.
Series.unique()
Return unique values of Series object.
Series.nunique([dropna])
Return number of unique elements in the object.
Series.is_unique
Return boolean if values in the object are unique.
Series.is_monotonic
Return boolean if values in the object are monotonic_increasing.
Series.is_monotonic_increasing
Alias for is_monotonic.
Series.is_monotonic_decreasing
Return boolean if values in the object are monotonic_decreasing.
Series.value_counts([normalize, sort, …])
Return a Series containing counts of unique values.
Reindexing / selection / label manipulation
Series.align(other[, join, axis, level, …])
Align two objects on their axes with the specified join method.
Series.drop([labels, axis, index, columns, …])
Return Series with specified index labels removed.
Series.droplevel(level[, axis])
Return DataFrame with requested index / column level(s) removed.
Series.drop_duplicates([keep, inplace])
Return Series with duplicate values removed.
Series.duplicated([keep])
Indicate duplicate Series values.
Series.equals(other)
Test whether two objects contain the same elements.
Series.first(offset)
Select initial periods of time series data based on a date offset.
Series.head([n])
Return the first n rows.
Series.idxmax([axis, skipna])
Return the row label of the maximum value.
Series.idxmin([axis, skipna])
Return the row label of the minimum value.
Series.isin(values)
Whether elements in Series are contained in values.
Series.last(offset)
Select final periods of time series data based on a date offset.
Series.reindex([index])
Conform Series to new index with optional filling logic.
Series.reindex_like(other[, method, copy, …])
Return an object with matching indices as other object.
Series.rename([index, axis, copy, inplace, …])
Alter Series index labels or name.
Series.rename_axis([mapper, index, columns, …])
Set the name of the axis for the index or columns.
Series.reset_index([level, drop, name, inplace])
Generate a new DataFrame or Series with the index reset.
Series.sample([n, frac, replace, weights, …])
Return a random sample of items from an axis of object.
Series.set_axis(labels[, axis, inplace])
Assign desired index to given axis.
Series.take(indices[, axis, is_copy])
Return the elements in the given positional indices along an axis.
Series.tail([n])
Return the last n rows.
Series.truncate([before, after, axis, copy])
Truncate a Series or DataFrame before and after some index value.
Series.where(cond[, other, inplace, axis, …])
Replace values where the condition is False.
Series.mask(cond[, other, inplace, axis, …])
Replace values where the condition is True.
Series.add_prefix(prefix)
Prefix labels with string prefix.
Series.add_suffix(suffix)
Suffix labels with string suffix.
Series.filter([items, like, regex, axis])
Subset the dataframe rows or columns according to the specified index labels.
Missing data handling
Series.backfill([axis, inplace, limit, downcast])
Synonym for DataFrame.fillna() with method='bfill'.
Series.bfill([axis, inplace, limit, downcast])
Synonym for DataFrame.fillna() with method='bfill'.
Series.dropna([axis, inplace, how])
Return a new Series with missing values removed.
Series.ffill([axis, inplace, limit, downcast])
Synonym for DataFrame.fillna() with method='ffill'.
Series.fillna([value, method, axis, …])
Fill NA/NaN values using the specified method.
Series.interpolate([method, axis, limit, …])
Fill NaN values using an interpolation method.
Series.isna()
Detect missing values.
Series.isnull()
Detect missing values.
Series.notna()
Detect existing (non-missing) values.
Series.notnull()
Detect existing (non-missing) values.
Series.pad([axis, inplace, limit, downcast])
Synonym for DataFrame.fillna() with method='ffill'.
Series.replace([to_replace, value, inplace, …])
Replace values given in to_replace with value.
Reshaping, sorting
Series.argsort([axis, kind, order])
Return the integer indices that would sort the Series values.
Series.argmin([axis, skipna])
Return int position of the smallest value in the Series.
Series.argmax([axis, skipna])
Return int position of the largest value in the Series.
Series.reorder_levels(order)
Rearrange index levels using input order.
Series.sort_values([axis, ascending, …])
Sort by the values.
Series.sort_index([axis, level, ascending, …])
Sort Series by index labels.
Series.swaplevel([i, j, copy])
Swap levels i and j in a MultiIndex.
Series.unstack([level, fill_value])
Unstack, also known as pivot, Series with MultiIndex to produce DataFrame.
Series.explode([ignore_index])
Transform each element of a list-like to a row.
Series.searchsorted(value[, side, sorter])
Find indices where elements should be inserted to maintain order.
Series.ravel([order])
Return the flattened underlying data as an ndarray.
Series.repeat(repeats[, axis])
Repeat elements of a Series.
Series.squeeze([axis])
Squeeze 1 dimensional axis objects into scalars.
Series.view([dtype])
Create a new view of the Series.
Combining / comparing / joining / merging
Series.append(to_append[, ignore_index, …])
Concatenate two or more Series.
Series.compare(other[, align_axis, …])
Compare to another Series and show the differences.
Series.update(other)
Modify Series in place using values from passed Series.
Time Series-related
Series.asfreq(freq[, method, how, …])
Convert TimeSeries to specified frequency.
Series.asof(where[, subset])
Return the last row(s) without any NaNs before where.
Series.shift([periods, freq, axis, fill_value])
Shift index by desired number of periods with an optional time freq.
Series.first_valid_index()
Return index for first non-NA/null value.
Series.last_valid_index()
Return index for last non-NA/null value.
Series.resample(rule[, axis, closed, label, …])
Resample time-series data.
Series.tz_convert(tz[, axis, level, copy])
Convert tz-aware axis to target time zone.
Series.tz_localize(tz[, axis, level, copy, …])
Localize tz-naive index of a Series or DataFrame to target time zone.
Series.at_time(time[, asof, axis])
Select values at particular time of day (e.g., 9:30AM).
Series.between_time(start_time, end_time[, …])
Select values between particular times of the day (e.g., 9:00-9:30 AM).
Series.tshift([periods, freq, axis])
(DEPRECATED) Shift the time index, using the index’s frequency if available.
Series.slice_shift([periods, axis])
(DEPRECATED) Equivalent to shift without copying data.
Accessors
pandas provides dtype-specific methods under various accessors. These are separate namespaces within Series that only apply to specific data types.

Data Type
Accessor
Datetime, Timedelta, Period
dt
String
str
Categorical
cat
Sparse
sparse
Datetimelike properties

Series.dt can be used to access the values of the series as datetimelike and return several properties. These can be accessed like Series.dt.<property>.

Datetime properties

Series.dt.date
Returns numpy array of python datetime.date objects (namely, the date part of Timestamps without timezone information).
Series.dt.time
Returns numpy array of datetime.time.
Series.dt.timetz
Returns numpy array of datetime.time also containing timezone information.
Series.dt.year
The year of the datetime.
Series.dt.month
The month as January=1, December=12.
Series.dt.day
The day of the datetime.
Series.dt.hour
The hours of the datetime.
Series.dt.minute
The minutes of the datetime.
Series.dt.second
The seconds of the datetime.
Series.dt.microsecond
The microseconds of the datetime.
Series.dt.nanosecond
The nanoseconds of the datetime.
Series.dt.week
(DEPRECATED) The week ordinal of the year.
Series.dt.weekofyear
(DEPRECATED) The week ordinal of the year.
Series.dt.dayofweek
The day of the week with Monday=0, Sunday=6.
Series.dt.day_of_week
The day of the week with Monday=0, Sunday=6.
Series.dt.weekday
The day of the week with Monday=0, Sunday=6.
Series.dt.dayofyear
The ordinal day of the year.
Series.dt.day_of_year
The ordinal day of the year.
Series.dt.quarter
The quarter of the date.
Series.dt.is_month_start
Indicates whether the date is the first day of the month.
Series.dt.is_month_end
Indicates whether the date is the last day of the month.
Series.dt.is_quarter_start
Indicator for whether the date is the first day of a quarter.
Series.dt.is_quarter_end
Indicator for whether the date is the last day of a quarter.
Series.dt.is_year_start
Indicate whether the date is the first day of a year.
Series.dt.is_year_end
Indicate whether the date is the last day of the year.
Series.dt.is_leap_year
Boolean indicator if the date belongs to a leap year.
Series.dt.daysinmonth
The number of days in the month.
Series.dt.days_in_month
The number of days in the month.
Series.dt.tz
Return timezone, if any.
Series.dt.freq
Return the frequency object for this PeriodArray.
Datetime methods

Series.dt.to_period(*args, **kwargs)
Cast to PeriodArray/Index at a particular frequency.
Series.dt.to_pydatetime()
Return the data as an array of native Python datetime objects.
Series.dt.tz_localize(*args, **kwargs)
Localize tz-naive Datetime Array/Index to tz-aware Datetime Array/Index.
Series.dt.tz_convert(*args, **kwargs)
Convert tz-aware Datetime Array/Index from one time zone to another.
Series.dt.normalize(*args, **kwargs)
Convert times to midnight.
Series.dt.strftime(*args, **kwargs)
Convert to Index using specified date_format.
Series.dt.round(*args, **kwargs)
Perform round operation on the data to the specified freq.
Series.dt.floor(*args, **kwargs)
Perform floor operation on the data to the specified freq.
Series.dt.ceil(*args, **kwargs)
Perform ceil operation on the data to the specified freq.
Series.dt.month_name(*args, **kwargs)
Return the month names of the DateTimeIndex with specified locale.
Series.dt.day_name(*args, **kwargs)
Return the day names of the DateTimeIndex with specified locale.
Period properties

Series.dt.qyear
Series.dt.start_time
Series.dt.end_time
Timedelta properties

Series.dt.days
Number of days for each element.
Series.dt.seconds
Number of seconds (>= 0 and less than 1 day) for each element.
Series.dt.microseconds
Number of microseconds (>= 0 and less than 1 second) for each element.
Series.dt.nanoseconds
Number of nanoseconds (>= 0 and less than 1 microsecond) for each element.
Series.dt.components
Return a Dataframe of the components of the Timedeltas.
Timedelta methods

Series.dt.to_pytimedelta()
Return an array of native datetime.timedelta objects.
Series.dt.total_seconds(*args, **kwargs)
Return total duration of each element expressed in seconds.
String handling

Series.str can be used to access the values of the series as strings and apply several methods to it. These can be accessed like Series.str.<function/property>.

Series.str.capitalize()
Convert strings in the Series/Index to be capitalized.
Series.str.casefold()
Convert strings in the Series/Index to be casefolded.
Series.str.cat([others, sep, na_rep, join])
Concatenate strings in the Series/Index with given separator.
Series.str.center(width[, fillchar])
Pad left and right side of strings in the Series/Index.
Series.str.contains(pat[, case, flags, na, …])
Test if pattern or regex is contained within a string of a Series or Index.
Series.str.count(pat[, flags])
Count occurrences of pattern in each string of the Series/Index.
Series.str.decode(encoding[, errors])
Decode character string in the Series/Index using indicated encoding.
Series.str.encode(encoding[, errors])
Encode character string in the Series/Index using indicated encoding.
Series.str.endswith(pat[, na])
Test if the end of each string element matches a pattern.
Series.str.extract(pat[, flags, expand])
Extract capture groups in the regex pat as columns in a DataFrame.
Series.str.extractall(pat[, flags])
Extract capture groups in the regex pat as columns in DataFrame.
Series.str.find(sub[, start, end])
Return lowest indexes in each strings in the Series/Index.
Series.str.findall(pat[, flags])
Find all occurrences of pattern or regular expression in the Series/Index.
Series.str.get(i)
Extract element from each component at specified position.
Series.str.index(sub[, start, end])
Return lowest indexes in each string in Series/Index.
Series.str.join(sep)
Join lists contained as elements in the Series/Index with passed delimiter.
Series.str.len()
Compute the length of each element in the Series/Index.
Series.str.ljust(width[, fillchar])
Pad right side of strings in the Series/Index.
Series.str.lower()
Convert strings in the Series/Index to lowercase.
Series.str.lstrip([to_strip])
Remove leading characters.
Series.str.match(pat[, case, flags, na])
Determine if each string starts with a match of a regular expression.
Series.str.normalize(form)
Return the Unicode normal form for the strings in the Series/Index.
Series.str.pad(width[, side, fillchar])
Pad strings in the Series/Index up to width.
Series.str.partition([sep, expand])
Split the string at the first occurrence of sep.
Series.str.repeat(repeats)
Duplicate each string in the Series or Index.
Series.str.replace(pat, repl[, n, case, …])
Replace each occurrence of pattern/regex in the Series/Index.
Series.str.rfind(sub[, start, end])
Return highest indexes in each strings in the Series/Index.
Series.str.rindex(sub[, start, end])
Return highest indexes in each string in Series/Index.
Series.str.rjust(width[, fillchar])
Pad left side of strings in the Series/Index.
Series.str.rpartition([sep, expand])
Split the string at the last occurrence of sep.
Series.str.rstrip([to_strip])
Remove trailing characters.
Series.str.slice([start, stop, step])
Slice substrings from each element in the Series or Index.
Series.str.slice_replace([start, stop, repl])
Replace a positional slice of a string with another value.
Series.str.split([pat, n, expand])
Split strings around given separator/delimiter.
Series.str.rsplit([pat, n, expand])
Split strings around given separator/delimiter.
Series.str.startswith(pat[, na])
Test if the start of each string element matches a pattern.
Series.str.strip([to_strip])
Remove leading and trailing characters.
Series.str.swapcase()
Convert strings in the Series/Index to be swapcased.
Series.str.title()
Convert strings in the Series/Index to titlecase.
Series.str.translate(table)
Map all characters in the string through the given mapping table.
Series.str.upper()
Convert strings in the Series/Index to uppercase.
Series.str.wrap(width, **kwargs)
Wrap strings in Series/Index at specified line width.
Series.str.zfill(width)
Pad strings in the Series/Index by prepending ‘0’ characters.
Series.str.isalnum()
Check whether all characters in each string are alphanumeric.
Series.str.isalpha()
Check whether all characters in each string are alphabetic.
Series.str.isdigit()
Check whether all characters in each string are digits.
Series.str.isspace()
Check whether all characters in each string are alphanumeric.
Series.str.islower()
Check whether all characters in each string are lowercase.
Series.str.isupper()
Check whether all characters in each string are uppercase.
Series.str.istitle()
Check whether all characters in each string are titlecase.
Series.str.isnumeric()
Check whether all characters in each string are numeric.
Series.str.isdecimal()
Check whether all characters in each string are decimal.
Series.str.get_dummies([sep])
Return DataFrame of dummy/indicator variables for Series.
Categorical accessor

Categorical-dtype specific methods and attributes are available under the Series.cat accessor.

Series.cat.categories
The categories of this categorical.
Series.cat.ordered
Whether the categories have an ordered relationship.
Series.cat.codes
Return Series of codes as well as the index.
Series.cat.rename_categories(*args, **kwargs)
Rename categories.
Series.cat.reorder_categories(*args, **kwargs)
Reorder categories as specified in new_categories.
Series.cat.add_categories(*args, **kwargs)
Add new categories.
Series.cat.remove_categories(*args, **kwargs)
Remove the specified categories.
Series.cat.remove_unused_categories(*args, …)
Remove categories which are not used.
Series.cat.set_categories(*args, **kwargs)
Set the categories to the specified new_categories.
Series.cat.as_ordered(*args, **kwargs)
Set the Categorical to be ordered.
Series.cat.as_unordered(*args, **kwargs)
Set the Categorical to be unordered.
Sparse accessor

Sparse-dtype specific methods and attributes are provided under the Series.sparse accessor.

Series.sparse.npoints
The number of non- fill_value points.
Series.sparse.density
The percent of non- fill_value points, as decimal.
Series.sparse.fill_value
Elements in data that are fill_value are not stored.
Series.sparse.sp_values
An ndarray containing the non- fill_value values.
Series.sparse.from_coo(A[, dense_index])
Create a Series with sparse values from a scipy.sparse.coo_matrix.
Series.sparse.to_coo([row_levels, …])
Create a scipy.sparse.coo_matrix from a Series with MultiIndex.
Flags

Flags refer to attributes of the pandas object. Properties of the dataset (like the date is was recorded, the URL it was accessed from, etc.) should be stored in Series.attrs.

Flags(obj, *, allows_duplicate_labels)
Flags that apply to pandas objects.
Metadata

Series.attrs is a dictionary for storing global metadata for this Series.

Warning
Series.attrs is considered experimental and may change without warning.
Series.attrs
Dictionary of global attributes of this dataset.
Plotting
Series.plot is both a callable method and a namespace attribute for specific plotting methods of the form Series.plot.<kind>.

Series.plot([kind, ax, figsize, ….])
Series plotting accessor and method
Series.plot.area([x, y])
Draw a stacked area plot.
Series.plot.bar([x, y])
Vertical bar plot.
Series.plot.barh([x, y])
Make a horizontal bar plot.
Series.plot.box([by])
Make a box plot of the DataFrame columns.
Series.plot.density([bw_method, ind])
Generate Kernel Density Estimate plot using Gaussian kernels.
Series.plot.hist([by, bins])
Draw one histogram of the DataFrame’s columns.
Series.plot.kde([bw_method, ind])
Generate Kernel Density Estimate plot using Gaussian kernels.
Series.plot.line([x, y])
Plot Series or DataFrame as lines.
Series.plot.pie(**kwargs)
Generate a pie plot.
Series.hist([by, ax, grid, xlabelsize, …])
Draw histogram of the input series using matplotlib.
Serialization / IO / conversion
Series.to_pickle(path[, compression, …])
Pickle (serialize) object to file.
Series.to_csv([path_or_buf, sep, na_rep, …])
Write object to a comma-separated values (csv) file.
Series.to_dict([into])
Convert Series to {label -> value} dict or dict-like object.
Series.to_excel(excel_writer[, sheet_name, …])
Write object to an Excel sheet.
Series.to_frame([name])
Convert Series to DataFrame.
Series.to_xarray()
Return an xarray object from the pandas object.
Series.to_hdf(path_or_buf, key[, mode, …])
Write the contained data to an HDF5 file using HDFStore.
Series.to_sql(name, con[, schema, …])
Write records stored in a DataFrame to a SQL database.
Series.to_json([path_or_buf, orient, …])
Convert the object to a JSON string.
Series.to_string([buf, na_rep, …])
Render a string representation of the Series.
Series.to_clipboard([excel, sep])
Copy object to the system clipboard.
Series.to_latex([buf, columns, col_space, …])
Render object to a LaTeX tabular, longtable, or nested table/tabular.
Series.to_markdown([buf, mode, index, …])
Print Series in Markdown-friendly format.