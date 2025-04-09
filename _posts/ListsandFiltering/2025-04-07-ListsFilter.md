---
layout: post
search_exclude: true
show_reading_time: false
permalink: /ListsFilter
title: Lists and Filtering 
categories: [ListsandFiltering]
---

# Lists and Searching Algorithms with Pandas

## Learning Objectives

By the end of this lesson, students will be able to:

- Understand fundamental list operations using both Python lists and Pandas Series/DataFrames
- Implement list procedures using proper syntax
- Apply traversal techniques (complete and partial) to both native lists and Pandas structures
- Use iteration statements to process data collections
- Implement common algorithms including:
  - Finding minimum/maximum values
  - Computing sums and averages
  - Performing searches and filters on datasets

---

## What Are Lists in Python and Pandas?

Lists in Python are ordered collections of items that allow us to store multiple related values under a single variable name. In data analysis, Pandas provides similar but more powerful structures called Series and DataFrames.

```python
# Example of a list in Python
student_scores = [95, 88, 76, 92, 84]

# Same data as a Pandas Series
import pandas as pd
scores_series = pd.Series([95, 88, 76, 92, 84], name="Scores")

# Example of a DataFrame - more complex data structure
student_data = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Emma'],
    'Score': [95, 88, 76, 92, 84],
    'Grade': ['A', 'B', 'C', 'A', 'B']
})
```

> **AAP-2.N.1**: The exam reference sheet provides basic operations on lists.

---

## Basic Operations

| Operation   | Python List                     | Pandas Series/DataFrame               |
|-------------|---------------------------------|---------------------------------------|
| **Create**  | `numbers = [1, 2, 3, 4, 5]`    | `series = pd.Series([1, 2, 3, 4, 5])` |
| **Access**  | `first = numbers[0]`           | `first = series[0]` or `df.iloc[0]`   |
| **Insert**  | `numbers.append(6)`            | `series = pd.concat([series, pd.Series([6])])` |
| **Remove**  | `numbers.remove(3)`            | `series = series[series != 3]`        |
| **Update**  | `numbers[1] = 10`              | `series[1] = 10` or `df.loc[1, 'column'] = 10` |
| **Length**  | `size = len(numbers)`          | `size = len(series)` or `df.shape[0]` |

> **AAP-2.N.2**: List procedures are implemented in accordance with the syntax rules of the programming language.

---

## Traversal with Python Lists and Pandas

Traversal is the process of accessing each element in a collection to perform operations.

> **AAP-2.O.1**: Traversing a list can be a complete traversal, where all elements are accessed, or a partial traversal, where only a portion of elements are accessed.

### Complete Traversal

```python
# Python List complete traversal
def print_all_list_elements(my_list):
    for item in my_list:
        print(item)

# Pandas Series complete traversal
def print_all_series_elements(my_series):
    for item in my_series:
        print(item)
        
# Pandas DataFrame complete traversal
def print_all_df_elements(df, column):
    for item in df[column]:
        print(item)
```

### Partial Traversal

```python
# Python List partial traversal (first half only)
def print_first_half_list(my_list):
    midpoint = len(my_list) // 2
    for i in range(midpoint):
        print(my_list[i])

# Pandas Series partial traversal
def print_first_half_series(my_series):
    midpoint = len(my_series) // 2
    for i in range(midpoint):
        print(my_series.iloc[i])
        
# Pandas DataFrame partial traversal
def print_first_half_df(df, column):
    midpoint = len(df) // 2
    for i in range(midpoint):
        print(df[column].iloc[i])
```

> **AAP-2.O.2**: Iteration statements can be used to traverse data collections.

---

## Iteration Techniques

### For Loop Traversal

```python
# Python List: Using index-based for loop
scores = [95, 88, 76, 92, 84]
for i in range(len(scores)):
    print(f"Student {i+1} score: {scores[i]}")

# Pandas Series: Using index-based traversal
scores_series = pd.Series([95, 88, 76, 92, 84])
for i in range(len(scores_series)):
    print(f"Student {i+1} score: {scores_series.iloc[i]}")

# Pandas DataFrame: Using iterrows()
for index, row in student_data.iterrows():
    print(f"Student {index+1}: {row['Name']} got {row['Score']}")
```

### While Loop Traversal

```python
# Python List with while loop
i = 0
while i < len(scores):
    print(f"Student {i+1} score: {scores[i]}")
    i += 1

# Pandas Series with while loop
i = 0
while i < len(scores_series):
    print(f"Student {i+1} score: {scores_series.iloc[i]}")
    i += 1
```

> **AAP-2.O.3**: The exam reference sheet provides pseudocode for loops.

---

## Common Algorithms with Python Lists and Pandas

> **AAP-2.O.4**: Knowledge of existing algorithms that use iteration can help in constructing new algorithms.

### Finding Minimum/Maximum Values

```python
# Python List approach
def find_min_max_list(numbers):
    if len(numbers) == 0:
        return None, None
    
    minimum = maximum = numbers[0]
    for num in numbers:
        if num < minimum:
            minimum = num
        if num > maximum:
            maximum = num
    return minimum, maximum

# Pandas approach
def find_min_max_pandas(series):
    if len(series) == 0:
        return None, None
    return series.min(), series.max()

# Example usage
scores = [95, 88, 76, 92, 84]
min_score, max_score = find_min_max_list(scores)
print(f"Min: {min_score}, Max: {max_score}")

scores_series = pd.Series(scores)
min_score, max_score = find_min_max_pandas(scores_series)
print(f"Min: {min_score}, Max: {max_score}")

# With DataFrame
min_score = student_data['Score'].min()
max_score = student_data['Score'].max() 
print(f"Min: {min_score}, Max: {max_score}")
```

### Computing Sum and Average

```python
# Python List approach
def compute_sum_avg_list(numbers):
    if len(numbers) == 0:
        return 0, 0
        
    total = 0
    for num in numbers:
        total += num
    average = total / len(numbers)
    return total, average

# Pandas approach
def compute_sum_avg_pandas(series):
    if len(series) == 0:
        return 0, 0
    return series.sum(), series.mean()

# Example with DataFrame
total_score = student_data['Score'].sum()
avg_score = student_data['Score'].mean()
print(f"Total: {total_score}, Average: {avg_score}")
```

---

## Searching Algorithms

### Linear Search in Python Lists

> **AAP-2.O.5**: Linear or sequential search algorithms check each element of a list, in order, until the desired value is found or all elements in the list have been checked.

```python
def linear_search(values, target):
    for i in range(len(values)):
        if values[i] == target:
            return i  # Return the index where target was found
    return -1  # Return -1 if target not found
```

### Searching in Pandas

```python
# Finding exact matches in a Series
def find_in_series(series, value):
    matches = series[series == value]
    if len(matches) > 0:
        return matches.index.tolist()
    return []

# Finding in a DataFrame
def find_in_dataframe(df, column, value):
    matches = df[df[column] == value]
    if len(matches.index) > 0:
        return matches.index.tolist()
    return []

# Example usage
student_data = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Emma'],
    'Score': [95, 88, 76, 92, 84],
    'Grade': ['A', 'B', 'C', 'A', 'B']
})

# Find students with grade 'A'
a_students = student_data[student_data['Grade'] == 'A']
print(a_students)

# Find students with scores above 90
high_scorers = student_data[student_data['Score'] > 90]
print(high_scorers)
```

---

## Working with Data - Step by Step

### Pattern: Filtering Data

```python
# Python List approach
def get_passing_scores_list(scores, passing_threshold=70):
    passing = []
    for score in scores:
        if score >= passing_threshold:
            passing.append(score)
    return passing

# Pandas approach
def get_passing_scores_pandas(scores_series, passing_threshold=70):
    return scores_series[scores_series >= passing_threshold]

# Example with DataFrame
passing_students = student_data[student_data['Score'] >= 70]
print(passing_students)
```

### Pattern: Transforming Data

```python
# Python List approach
def apply_curve_list(scores, curve=5):
    curved_scores = []
    for score in scores:
        curved_scores.append(min(100, score + curve))
    return curved_scores

# Pandas approach
def apply_curve_pandas(scores_series, curve=5):
    return scores_series.apply(lambda x: min(100, x + curve))

# Example with DataFrame
student_data['Curved Score'] = student_data['Score'].apply(lambda x: min(100, x + 5))
print(student_data)
```

### Pattern: Grouping and Aggregating

```python
# Grouping by grade and calculating average score
grade_stats = student_data.groupby('Grade')['Score'].agg(['mean', 'min', 'max', 'count'])
print(grade_stats)
```

---

## PopCorn Hacks (In-Class Exercises)

### PopCorn Hack 1: Find Students with Scores in a Range

```python
# Complete the function to find all students with scores between min_score and max_score
def find_students_in_range(df, min_score, max_score):
    # Your code here
    pass

# Test with: find_students_in_range(student_data, 80, 90)
```

### PopCorn Hack 2: Calculate Letter Grades

```python
# Complete the function to add a 'Letter' column based on numerical scores
def add_letter_grades(df):
    # Your code here
    # A: 90-100, B: 80-89, C: 70-79, D: 60-69, F: below 60
    pass

# Test with: add_letter_grades(student_data)
```

### PopCorn Hack 3: Find the Mode in a Series

```python
# Complete the function to find the most common value in a series
def find_mode(series):
    # Your code here
    pass

# Test with: find_mode(pd.Series([1, 2, 2, 3, 4, 2, 5]))
```

---

## SQL with SQLite3 and Pandas

Pandas can also interact with SQL databases, which is another way to work with structured data:

```python
import sqlite3
import pandas as pd

# Create an in-memory SQLite database
conn = sqlite3.connect(':memory:')

# Create a table and insert data
student_data.to_sql('students', conn, index=False)

# Query the database with SQL
query = "SELECT * FROM students WHERE Score > 80"
high_scorers = pd.read_sql_query(query, conn)
print(high_scorers)

# More complex query with aggregation
query = """
SELECT Grade, AVG(Score) as AvgScore, COUNT(*) as Count
FROM students
GROUP BY Grade
ORDER BY AvgScore DESC
"""
grade_stats = pd.read_sql_query(query, conn)
print(grade_stats)
```

---
## MCQ Questions1. Which of the following is the correct way to create a Pandas Series from a list?
A. series = pd.DataFrame([1, 2, 3])
B. series = pd.Series([1, 2, 3])
C. series = [1, 2, 3]
D. series = pd.Series({1, 2, 3})
<details> <summary>💡 Answer</summary> B. `series = pd.Series([1, 2, 3])` </details>
2. What is the purpose of a traversal in the context of lists and Pandas structures?
A. To delete all elements in a collection
B. To access each element in a collection
C. To sort the collection alphabetically
D. To filter out duplicate values
<details> <summary>💡 Answer</summary> B. To access each element in a collection </details>
3. What Python keyword is most commonly used in a complete traversal of a list?
A. map
B. def
C. for
D. try
<details> <summary>💡 Answer</summary> C. `for` </details>
4. How can you find all rows in a DataFrame where the value in the "Score" column is greater than 90?
A. df[Score > 90]
B. df['Score'] > 90
C. df[df['Score'] > 90]
D. df.where('Score' > 90)
<details> <summary>💡 Answer</summary> C. `df[df['Score'] > 90]` </details>
5. Which of the following is the correct method to find the average of a Pandas Series?
A. series.mean()
B. mean(series)
C. average(series)
D. series.avg()
<details> <summary>💡 Answer</summary> A. `series.mean()` </details>
6. In a DataFrame, what does df.iloc[0] return?
A. The first column of the DataFrame
B. The first row of the DataFrame
C. The column names
D. All rows except the first one
<details> <summary>💡 Answer</summary> B. The first row of the DataFrame </details>
7. What does the following code return if the value is not found?
def linear_search(values, target):
    for i in range(len(values)):
        if values[i] == target:
            return i
    return -1
A. It returns 0
B. It raises an error
C. It returns -1
D. It returns None
<details> <summary>💡 Answer</summary> C. It returns -1 </details>
8. Which Pandas method allows you to add a new column to a DataFrame by transforming an existing one?
A. .add()
B. .transform()
C. .apply()
D. .insert()
<details> <summary>💡 Answer</summary> C. `.apply()` </details>
9. What is the result of this code?
numbers = [1, 2, 3]
numbers.append(4)
print(numbers)
A. [1, 2, 3]
B. [1, 2, 3, 4]
C. 1, 2, 3, 4
D. [4, 1, 2, 3]
<details> <summary>💡 Answer</summary> B. `[1, 2, 3, 4]` </details>
10. What is a benefit of using Pandas DataFrames over native Python lists?
A. They execute faster than lists
B. They allow heterogeneous types within the same column
C. They provide tabular data structures with built-in methods for filtering, aggregation, and transformation
D. They use less memory
<details> <summary>💡 Answer</summary> C. They provide tabular data structures with built-in methods for filtering, aggregation, and transformation </details>


## Homework Assignment: Data Analysis

For this homework, you'll work with a dataset of student performance and implement various list algorithms using both Python lists and Pandas.

**Dataset**: A CSV file related to your Pilot City project. For example, our project is Fire Predictions using satellite data, so we would find a satellite reading CSV file and use Pandas to process it. CSV is the most compatible with Pandas and is therefore the most recommended. Others will also work.

**Tasks**:

1. Load the data using Pandas:
   ```python
   import pandas as pd
   datas = pd.read_csv('data_title.csv')
   ```

2. Implement the following algorithms:  
   - Find fire incidents with the highest and lowest overall average temperature recorded.  
   - Calculate the difference between the maximum and minimum temperatures for each fire incident.  
   - Identify all fire incidents where the temperature exceeded the average temperature across all incidents.  
   - Group fire incidents by vegetation type and weather conditions, then calculate the average temperature and wind speed for each group.  

3. Answer the following analytical questions:
   - **Is there a correlation between vegetation type and fire intensity?**  
     Use Pandas to calculate the correlation between vegetation type and fire intensity. Visualize the results using a bar chart or scatter plot.

   - **Which weather condition is associated with the highest average fire intensity?**  
     Compute the average fire intensity for each weather condition and identify the condition with the highest average.

   - **What percentage of fire incidents recorded temperatures above 100°F?**  
     Filter the dataset to find fire incidents with temperatures above 100°F, then calculate the percentage of such incidents relative to the total.

4. Create a SQLite database:
   - **Store the fire incident data in a table**  
     Use SQLite to create a database and store the fire incident data in a table named `fire_incidents`.

   - **Write SQL queries to extract insights about fire incidents**  
     Examples:
     - Find the average temperature and wind speed for each vegetation type.
     - Identify fire incidents where the temperature exceeded 120°F and wind speed was above 15 mph.
     - Group fire incidents by weather condition and calculate the average fire intensity for each group.

   - **Compare the SQL approach with the direct Pandas approach**  
     Discuss the advantages and disadvantages of using SQL versus Pandas for data analysis, focusing on performance, readability, and ease of use.

**Submission Format**: Jupyter Notebook with code, explanations, and visualizations.

<span style="color:red; font-size: 1.5em;">**DUE DATE**: **[ONE DAY FROM TODAY WEDNESDAY 10:00 PM]**</span>

---

## Extension: Data Visualization (0.02 Extra Credit Using Seaborn or matplotlib)

After analyzing the fire data, visualize your findings:

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Set up the plotting style
plt.style.use('ggplot')
sns.set_palette("pastel")

# Example: Temperature distribution by vegetation type
plt.figure(figsize=(12, 6))
sns.boxplot(x='Vegetation Type', y='Temperature', data=fire_data)
plt.title('Temperature Distribution by Vegetation Type')
plt.ylabel('Temperature (°F)')
plt.xlabel('Vegetation Type')
plt.xticks(rotation=45)
plt.show()

# Example: Average fire intensity by weather condition
weather_effect = fire_data.groupby('Weather Condition')['Fire Intensity'].mean().reset_index()
sns.barplot(x='Weather Condition', y='Fire Intensity', data=weather_effect)
plt.title('Effect of Weather Condition on Fire Intensity')
plt.ylabel('Average Fire Intensity')
plt.xlabel('Weather Condition')
plt.xticks(rotation=45)
plt.show()
```
---

## Dataset Design and Issue Reference

Before diving into data analysis, it's crucial to ensure that your datasets are well-structured and designed for the intended operations. Poorly designed datasets can lead to inefficiencies, errors, and difficulties in testing.

For example, the following issue highlights problems with improperly designed columns in a dataset:  
[GitHub Issue #34: Example Results - Dataset Design Problems](https://github.com/SanPranav/QcommVNE_Frontend/issues/34)

We **highly recommend** reviewing and refining your datasets before creating a final cumulative set for testing. This step ensures that your data is clean, consistent, and optimized for analysis, reducing the risk of encountering issues during testing and implementation.

### Example: Filtered Dataset

Below is an example of a filtered dataset based on the criteria mentioned in the issue above. This dataset excludes problematic columns, removes unrealistic fire sizes, and focuses on meaningful attributes for analysis. This filtered dataset can now be used for further analysis, such as identifying trends in fire occurrences, calculating average fire sizes, or visualizing fire locations on a map. For more details, refer to the issue linked above.

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
file_path = "/home/pranav/nighthawk/Pranav_2025/Pranav_2025/assets/data/National_USFS_Fire_Occurrence_Point_(Feature_Layer).csv"
df = pd.read_csv(file_path, low_memory=False)

# Drop problematic columns (hardcoded column names due to dtype warning)
drop_columns = ['column_name_5', 'column_name_11', 'column_name_12', 'column_name_13', 'column_name_18', 'column_name_19']
df = df.drop(columns=[col for col in drop_columns if col in df.columns])

# Rename columns for clarity
df.rename(columns={'TOTALACRES': 'fire_size'}, inplace=True)

# Remove unrealistic fire sizes (e.g., negative values, extreme outliers)
if 'fire_size' in df.columns:
    df = df[df['fire_size'] > 0]  # Remove negative or zero fire sizes
    df = df[df['fire_size'] < df['fire_size'].quantile(0.99)]  # Remove extreme outliers

# Convert FIREYEAR to integer
df['FIREYEAR'] = df['FIREYEAR'].fillna(0).astype(int)

# Create a summary DataFrame
summary_df = df.groupby('FIREYEAR')['fire_size'].agg(['count', 'mean', 'sum']).reset_index()
summary_df.columns = ['Year', 'Fire Count', 'Avg Fire Size', 'Total Burned Area']

# Display the first few rows
summary_df.head()

# Plot Fire Count Over Years
plt.figure(figsize=(12, 6))
sns.barplot(x=summary_df['Year'], y=summary_df['Fire Count'], palette='viridis')
plt.title("Total Fire Occurrences Per Year")
plt.xlabel("Year")
plt.ylabel("Number of Fires")
plt.xticks(rotation=45)
plt.show()

# Plot Total Burned Area Over Years
plt.figure(figsize=(12, 6))
sns.lineplot(x=summary_df['Year'], y=summary_df['Total Burned Area'], marker='o', color='red')
plt.title("Total Burned Area Per Year")
plt.xlabel("Year")
plt.ylabel("Acres Burned")
plt.grid()
plt.show()

# Fire Size Distribution Boxplot
plt.figure(figsize=(10, 6))
sns.boxplot(x=df['fire_size'], showfliers=False, color='blue')
plt.title("Fire Size Distribution")
plt.xlabel("Fire Size (Acres)")
plt.show()
```
---
## Multiple Choice

1. **What is the primary difference between a Python list and a Pandas Series?**  
    A. Lists can only store numbers, while Series can store strings  
    B. Pandas Series have labeled indices, while lists use integer positions  
    C. Lists use loops, Series use if statements  
    D. Pandas Series cannot be modified after creation  
    :white_check_mark: **Correct Answer**: B  
    **Explanation**: Lists use integer-based indices, while Pandas Series support labeled indexing with more functionality.

2. **Which of the following creates a DataFrame in Pandas?**  
    A. `df = pd.Series({'Name': 'Alice', 'Score': 95})`  
    B. `df = pd.List({'Name': 'Alice', 'Score': 95})`  
    C. `df = pd.DataFrame({'Name': ['Alice'], 'Score': [95]})`  
    D. `df = pd.Table({'Alice': 95})`  
    :white_check_mark: **Correct Answer**: C  
    **Explanation**: Pandas DataFrames are created using `pd.DataFrame()` with dictionary values as lists.

3. **What will `df.shape[0]` return when `df` is a DataFrame?**  
    A. Number of columns  
    B. Number of rows  
    C. Total number of cells  
    D. First value of the DataFrame  
    :white_check_mark: **Correct Answer**: B  
    **Explanation**: `df.shape` returns a tuple `(rows, columns)`, so `df.shape[0]` gives the row count.

4. **Which operation would you use to remove the value 3 from a Pandas Series?**  
    A. `series.remove(3)`  
    B. `series = series[series != 3]`  
    C. `series.pop(3)`  
    D. `del series[3]`  
    :white_check_mark: **Correct Answer**: B  
    **Explanation**: Filtering with `series[series != 3]` returns a new Series without the value 3.

![Image](https://github.com/user-attachments/assets/d43dc36e-4519-4b7a-83b8-4c1e221b33cf)
![Image](https://github.com/user-attachments/assets/f4d002c4-9c3b-48b1-901b-b9357fb7293a)

## College Board Connections

| Code         | Description                                                                 |
|--------------|-----------------------------------------------------------------------------|
| **AAP-2.N.1** | The exam reference sheet provides basic operations on lists.               |
| **AAP-2.N.2** | List procedures are implemented in accordance with the syntax rules.       |
| **AAP-2.O.1** | Traversing a list can be a complete traversal or a partial traversal.      |
| **AAP-2.O.2** | Iteration statements can be used to traverse a list.                       |
| **AAP-2.O.3** | The exam reference sheet provides pseudocode for loops.                   |
| **AAP-2.O.4** | Knowledge of existing algorithms that use iteration can help in constructing new algorithms. |
| **AAP-2.O.5** | Linear or sequential search algorithms check each element until the desired value is found or all elements have been checked. |

---

## Additional Resources

- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [SQLite with Python Tutorial](https://docs.python.org/3/library/sqlite3.html)
- [Kaggle: Student Performance Dataset](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams)
- [DataCamp: Pandas Cheat Sheet](https://www.datacamp.com/community/blog/python-pandas-cheat-sheet)
- [W3Schools: Python Pandas Tutorial](https://www.w3schools.com/python/pandas/default.asp)