#!/usr/bin/env python
# coding: utf-8

# In[40]:


# SETUP
import pandas as pd
from pathlib import Path

# CSV LOAD IN
school_data_to_load = Path("/Users/pablofortich/Desktop/Pandas Challenge/PyCitySchools/Resources/schools_complete.csv")
student_data_to_load = Path("/Users/pablofortich/Desktop/Pandas Challenge/PyCitySchools/Resources/students_complete.csv")

# Read School and Student Data File 
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)

# Combine data  
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
school_data_complete.head()


# In[41]:


# DISTRICT SUMMARY
# Code for the number of unique schools
total_schools = school_data_complete['school_name'].nunique()

# Code to calculate number of students
total_students = school_data_complete['Student ID'].nunique()

# Code for the budget
total_budget = school_data['budget'].sum()

# Code to find the average math score
average_math_score = school_data_complete['math_score'].mean()

# Code to find the average reading score
average_reading_score = school_data_complete['reading_score'].mean()

# Code to find the percentage passing math
percentage_passing_math = (school_data_complete['math_score'] >= 70).mean() * 100

# Code to find the percentage passing reading
percentage_passing_reading = (school_data_complete['reading_score'] >= 70).mean() * 100

# Code the percentage overall passing
percentage_overall_passing = ((school_data_complete['math_score'] >= 70) & (school_data_complete['reading_score'] >= 70)).mean() * 100

# DataFrame that will hold the district summary data
district_summary = pd.DataFrame({
    'Total Schools': [total_schools],
    'Total Students': [total_students],
    'Total Budget': [total_budget],
    'Average Math Score': [average_math_score],
    'Average Reading Score': [average_reading_score],
    '% Passing Math': [percentage_passing_math],
    '% Passing Reading': [percentage_passing_reading],
    '% Overall Passing': [percentage_overall_passing]
})

# Formatting of the Total Budget with currency style
district_summary['Total Budget'] = district_summary['Total Budget'].map('${:,.2f}'.format)

# Display of the district summary
district_summary


# In[3]:


# SCHOOL SUMMARY
grouped_schools = school_data_complete.groupby('school_name')

# Code the total students per school
total_students_per_school = grouped_schools['Student ID'].count()

# School type for each school 
school_types = grouped_schools['type'].first()

# Code for the total school budget for each school
total_budget_per_school = grouped_schools['budget'].first()

# Code for the student budget PER
per_student_budget = total_budget_per_school / total_students_per_school

# Code for the average math score per school
average_math_score_per_school = grouped_schools['math_score'].mean()

# Code for the average reading score per school
average_reading_score_per_school = grouped_schools['reading_score'].mean()

# Code for the percentage passing math per school
percentage_passing_math_per_school = (school_data_complete[school_data_complete['math_score'] >= 70]
                                      .groupby('school_name')['Student ID'].count() / total_students_per_school) * 100

# Code for the the percentage passing reading per school
percentage_passing_reading_per_school = (school_data_complete[school_data_complete['reading_score'] >= 70]
                                         .groupby('school_name')['Student ID'].count() / total_students_per_school) * 100

# Code for the percentage overall passing per school
percentage_overall_passing_per_school = (school_data_complete[(school_data_complete['math_score'] >= 70)
                                                               & (school_data_complete['reading_score'] >= 70)]
                                         .groupby('school_name')['Student ID'].count() / total_students_per_school) * 100

# DataFrame to hold the school summary data
school_summary = pd.DataFrame({
    'School Type': school_types,
    'Total Students': total_students_per_school,
    'Total School Budget': total_budget_per_school,
    'Per Student Budget': per_student_budget,
    'Average Math Score': average_math_score_per_school,
    'Average Reading Score': average_reading_score_per_school,
    '% Passing Math': percentage_passing_math_per_school,
    '% Passing Reading': percentage_passing_reading_per_school,
    '% Overall Passing': percentage_overall_passing_per_school
})

# Format Total School Budget and Per Student Budget with currency style
school_summary['Total School Budget'] = school_summary['Total School Budget'].map('${:,.2f}'.format)
school_summary['Per Student Budget'] = school_summary['Per Student Budget'].map('${:,.2f}'.format)

# Display of the school summary
school_summary


# In[42]:


# HIGHEST-PERFORMING SCHOOLS
# Sort the school summary DataFrame by % Overall Passing in descending order
top_schools = school_summary.sort_values('% Overall Passing', ascending=False)

# Display the top 5 rows
top_schools.head()


# In[43]:


# LOWEST PERRFORMING SCHOOLS
# Sort the school summary DataFrame by % Overall Passing in ascending order
bottom_schools = school_summary.sort_values('% Overall Passing')

# Display the top 5 rows
bottom_schools.head()


# In[44]:


# MATH SCORES BY GRADE
#  Average math scores by grade and school pivot table used
math_scores_by_grade = pd.pivot_table(student_data, values='math_score', index='school_name', columns='grade', aggfunc='mean')

# Reorder columns to display grades in the correct order
math_scores_by_grade = math_scores_by_grade[['9th', '10th', '11th', '12th']]

# Avg math scores by grade and school
math_scores_by_grade


# In[45]:


# READING SCORES BY GRADE
# Average reading scores by grade and school. Pivot table used
reading_scores_by_grade = pd.pivot_table(student_data, values='reading_score', index='school_name', columns='grade', aggfunc='mean')

# Reorder columns to display grades in the correct order
reading_scores_by_grade = reading_scores_by_grade[['9th', '10th', '11th', '12th']]

# Avg reading scores by grade and school
reading_scores_by_grade


# In[46]:


# SCORES BY SCHOOL SPENDING
# Data Frames 

school_spending_df = pd.DataFrame({
    'Average Math Score': average_math_score_per_school,
    'Average Reading Score': average_reading_score_per_school,
    '% Passing Math': percentage_passing_math_per_school,
    '% Passing Reading': percentage_passing_reading_per_school,
    '% Overall Passing': percentage_overall_passing_per_school
})

# Bins and labels for school spending
spending_bins = [0, 585, 630, 645, 680]
labels = ["<$585", "$585-630", "$630-645", "$645-680"]

# Categorizationn of spending based on the bins
school_spending_df['Spending Ranges (Per Student)'] = pd.cut(per_student_budget, spending_bins, labels=labels)

#  c mean scores per spending range
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Math Score"].mean()
spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Reading Score"].mean()
spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Math"].mean()
spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Reading"].mean()
overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Overall Passing"].mean()

# DataFrame for spending_summary
spending_summary = pd.DataFrame({
    'Average Math Score': spending_math_scores,
    'Average Reading Score': spending_reading_scores,
    '% Passing Math': spending_passing_math,
    '% Passing Reading': spending_passing_reading,
    '% Overall Passing': overall_passing_spending
})

# Display of the spending summary
spending_summary


# In[47]:


# SCORES BY SCHOOL SIZE
# DataFrame with the columns
per_school_summary = pd.DataFrame({
    'Average Math Score': average_math_score_per_school,
    'Average Reading Score': average_reading_score_per_school,
    '% Passing Math': percentage_passing_math_per_school,
    '% Passing Reading': percentage_passing_reading_per_school,
    '% Overall Passing': percentage_overall_passing_per_school,
    'Total Students': total_students_per_school  
})

# Bins and labels for school size
size_bins = [0, 1000, 2000, 5000]
labels = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]

# Categorize school size based on the bins
per_school_summary['School Size'] = pd.cut(per_school_summary['Total Students'], size_bins, labels=labels)

# Group by school size and calculate mean scores
size_summary = per_school_summary.groupby('School Size').agg({
    'Average Math Score': 'mean',
    'Average Reading Score': 'mean',
    '% Passing Math': 'mean',
    '% Passing Reading': 'mean',
    '% Overall Passing': 'mean'
})

# Display the size summary
size_summary



# In[48]:


# SCORE BY SCHOOL TYPE

# DataFrame with columns
per_school_summary = pd.DataFrame({
    'Average Math Score': average_math_score_per_school,
    'Average Reading Score': average_reading_score_per_school,
    '% Passing Math': percentage_passing_math_per_school,
    '% Passing Reading': percentage_passing_reading_per_school,
    '% Overall Passing': percentage_overall_passing_per_school,
    'Total Students': total_students_per_school,  
    'School Type': school_types  
})

# Group by school type and calculate mean scores
type_summary = per_school_summary.groupby('School Type').agg({
    'Average Math Score': 'mean',
    'Average Reading Score': 'mean',
    '% Passing Math': 'mean',
    '% Passing Reading': 'mean',
    '% Overall Passing': 'mean'
})

# Display the type summary
type_summary




# In[ ]:




