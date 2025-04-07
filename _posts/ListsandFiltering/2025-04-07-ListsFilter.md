---
layout: post
search_exclude: true
show_reading_time: false
permalink: /ListsFilter
title: Lists and Filtering 
categories: [ListsandFiltering]
---

## 1. Basic Python List and Pandas Structures

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

Breakdown:
- Line 1-2: Creates a standard Python list called `student_scores` with 5 integers representing test scores
- Line 4-5: Imports the pandas library and creates a Series (pandas' 1D array) with the same data, plus a name attribute "Scores"
- Line 7-11: Creates a DataFrame (pandas' 2D table) with 3 columns:
  - 'Name': A list of 5 student names as strings
  - 'Score': The same list of scores from before
  - 'Grade': A list of letter grades corresponding to each student

## 2. Complete Traversal Functions

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

Breakdown:
- Line 1-4: Defines a function that takes a list as input and uses a simple for loop to print each element
- Line 6-9: Defines a function that works similarly but takes a pandas Series as input
- Line 11-14: Defines a function that takes a DataFrame and column name as inputs, then prints each value in that specific column

## 3. Partial Traversal Functions

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

Breakdown:
- Line 1-5: Defines a function that:
  - Calculates the midpoint of the list using integer division (`//`)
  - Uses a for loop with `range(midpoint)` to iterate through only the first half of indices
  - Prints each element in the first half
- Line 7-11: Similar function for pandas Series, using `.iloc[i]` to access elements by integer position
- Line 13-17: Function for DataFrame columns that:
  - Calculates midpoint based on DataFrame length
  - Uses `df[column].iloc[i]` to access specific elements in the specified column

## 4. For Loop Traversal Examples

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

Breakdown:
- Line 1-4: Demonstrates index-based traversal of a list:
  - Creates a list of scores
  - Loops through indices using `range(len(scores))`
  - Prints each score with a formatted string showing student number (index+1)
- Line 6-9: Similar approach with a pandas Series:
  - Creates a Series with the same data
  - Uses `.iloc[i]` to access elements by integer position
- Line 11-13: Shows DataFrame traversal using `.iterrows()`:
  - This method returns both the index and the entire row as a Series
  - The loop unpacks these into `index` and `row` variables
  - Accesses specific columns of each row using dictionary-style notation `row['Name']`

## 5. While Loop Traversal Examples

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

Breakdown:
- Line 1-5: Shows while loop traversal for a list:
  - Initializes a counter `i` to 0
  - Loop continues as long as `i` is less than the list length
  - Prints each score with its corresponding student number
  - Increments `i` to move to the next element
- Line 7-11: Same approach with a pandas Series:
  - Uses the same counter and condition
  - Uses `.iloc[i]` to access elements by position
  - Remember to increment `i` each iteration to avoid infinite loops

## 6. Finding Min/Max Values

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

Breakdown:
- Line 1-12: Defines a function to find min/max values in a list:
  - First checks if the list is empty and returns None values if so
  - Initializes both minimum and maximum to the first element
  - Traverses the list, updating minimum or maximum whenever a smaller/larger value is found
  - Returns both values as a tuple
- Line 14-18: Simplified pandas version that:
  - Checks for empty Series
  - Uses built-in `.min()` and `.max()` methods
- Line 20-24: Example using the list approach with sample data
- Line 26-28: Same example using the pandas approach
- Line 30-33: Shows how to find min/max directly on a DataFrame column

## 7. Computing Sum and Average

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

Breakdown:
- Line 1-10: Defines a function to calculate sum and average of a list:
  - Handles empty list case
  - Initializes total to 0
  - Loops through each number, adding it to the total
  - Calculates average by dividing total by list length
  - Returns both values as a tuple
- Line 12-16: Pandas version using built-in methods:
  - Handles empty Series case
  - Uses `.sum()` and `.mean()` methods
- Line 18-21: Example showing how to calculate directly on a DataFrame column

## 8. Linear Search in Python Lists

```python
def linear_search(values, target):
    for i in range(len(values)):
        if values[i] == target:
            return i  # Return the index where target was found
    return -1  # Return -1 if target not found
```

Breakdown:
- Line 1-5: Defines a linear search function that:
  - Takes a list of values and a target value to search for
  - Iterates through each index of the list
  - Compares each element with the target
  - Returns the index immediately when a match is found
  - Returns -1 if no match is found after checking all elements

## 9. Searching in Pandas

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

Breakdown:
- Line 1-6: Defines a function to find matches in a Series:
  - Uses boolean indexing `series[series == value]` to filter
  - Returns a list of indices for the matching values
- Line 8-13: Similar function for DataFrames:
  - Takes a DataFrame, column name, and value
  - Uses boolean indexing `df[df[column] == value]` to filter
  - Returns a list of indices for rows with matching values
- Line 15-20: Creates sample student data
- Line 22-24: Example using boolean indexing to find students with grade 'A'
- Line 26-28: Example using comparison operator to find students with scores above 90

## 10. Filtering Data

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

Breakdown:
- Line 1-7: Defines a function to filter passing scores from a list:
  - Creates an empty list to store passing scores
  - Loops through each score
  - Appends to the result list only if the score meets the threshold
- Line 9-11: Pandas version using boolean indexing:
  - Uses the comparison operator to create a boolean mask
  - Returns only the values that meet the condition
- Line 13-15: Example showing how to filter a DataFrame by a threshold value

## 11. Transforming Data

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

Breakdown:
- Line 1-6: Defines a function to apply a curve to scores in a list:
  - Creates an empty list for curved scores
  - Loops through each score, adding the curve value
  - Uses `min(100, score + curve)` to cap scores at 100
  - Appends each curved score to the result list
- Line 8-10: Pandas version using `.apply()`:
  - Takes a lambda function that adds the curve and caps at 100
  - Applies this function to each element of the Series
- Line 12-14: Example of adding a new column to a DataFrame:
  - Creates 'Curved Score' column using `.apply()` with a lambda function
  - Prints the updated DataFrame

## 12. Grouping and Aggregating

```python
# Grouping by grade and calculating average score
grade_stats = student_data.groupby('Grade')['Score'].agg(['mean', 'min', 'max', 'count'])
print(grade_stats)
```

Breakdown:
- Line 1-3: Demonstrates grouping and aggregation in pandas:
  - Uses `.groupby('Grade')` to group the data by the 'Grade' column
  - Selects the 'Score' column from each group
  - Calls `.agg()` with a list of functions to compute:
    - 'mean': average score
    - 'min': minimum score
    - 'max': maximum score
    - 'count': number of students
  - The result is a DataFrame with grades as index and each statistic as a column

## 13. SQL with SQLite3 and Pandas

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

Breakdown:
- Line 1-2: Imports sqlite3 for database operations and pandas
- Line 4-5: Creates an in-memory SQLite database connection
- Line 7-8: Converts the student_data DataFrame to an SQL table called 'students'
- Line 10-13: Executes a simple SQL query to find students with scores > 80:
  - Uses `pd.read_sql_query()` to run the query and get results as a DataFrame
- Line 15-22: Demonstrates a more complex SQL query:
  - Uses multi-line string for better readability
  - Groups by Grade
  - Calculates average score and count for each grade
  - Orders results by average score in descending order
  - Returns results as a DataFrame

## 14. Data Visualization Examples

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Set up the plotting style
plt.style.use('ggplot')
sns.set_palette("pastel")

# Example: Score distribution by subject
plt.figure(figsize=(12, 6))
sns.boxplot(data=student_data[['Math Score', 'Reading Score', 'Writing Score']])
plt.title('Score Distribution by Subject')
plt.ylabel('Score')
plt.show()

# Example: Average score by test preparation status
prep_effect = student_data.groupby('Test Preparation')['Average Score'].mean().reset_index()
sns.barplot(x='Test Preparation', y='Average Score', data=prep_effect)
plt.title('Effect of Test Preparation on Average Score')
plt.show()
```

Breakdown:
- Line 1-2: Imports visualization libraries matplotlib and seaborn
- Line 4-6: Sets up plotting style:
  - Uses 'ggplot' style from matplotlib
  - Sets a "pastel" color palette from seaborn
- Line 8-13: Creates a boxplot visualization:
  - Sets figure size to 12x6 inches
  - Creates a boxplot showing distribution of scores across three subjects
  - Adds title and y-axis label
  - Displays the plot
- Line 15-19: Creates a bar chart visualization:
  - Groups data by 'Test Preparation' and calculates mean 'Average Score'
  - Uses `.reset_index()` to convert groupby result to regular DataFrame
  - Creates a bar plot comparing average scores between preparation groups
  - Adds a title and displays the plot

These code snippets demonstrate a progression from basic Python lists to more powerful pandas data structures, along with various techniques for data manipulation, analysis, and visualization.