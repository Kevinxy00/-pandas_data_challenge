

```python
import pandas as pd
import numpy as np

csv_schools=r'C:\Users\K Y\pandas_data_challenge\PyCitySchools\raw_data\schools_complete.csv'
csv_students=r'C:\Users\K Y\pandas_data_challenge\PyCitySchools\raw_data\students_complete.csv'

```


```python
schoolsData = pd.read_csv(csv_schools)
students_df = pd.read_csv(csv_students)

#change name in schoolsData (output = schools_df) so it matches school in students_df
schoolsData = schoolsData.rename(columns = {'name':'school'})
schools_df = schoolsData.set_index('school', drop=True)
students_df.head(15)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Bryan Miranda</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>94</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Sheena Carter</td>
      <td>F</td>
      <td>11th</td>
      <td>Huang High School</td>
      <td>82</td>
      <td>80</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>Nicole Baker</td>
      <td>F</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>96</td>
      <td>69</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>Michael Roth</td>
      <td>M</td>
      <td>10th</td>
      <td>Huang High School</td>
      <td>95</td>
      <td>87</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>Matthew Greene</td>
      <td>M</td>
      <td>10th</td>
      <td>Huang High School</td>
      <td>96</td>
      <td>84</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>Andrew Alexander</td>
      <td>M</td>
      <td>10th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>70</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>Daniel Cooper</td>
      <td>M</td>
      <td>10th</td>
      <td>Huang High School</td>
      <td>78</td>
      <td>77</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>Brittney Walker</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>64</td>
      <td>79</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>William Long</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>71</td>
      <td>79</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>Tammy Hebert</td>
      <td>F</td>
      <td>10th</td>
      <td>Huang High School</td>
      <td>85</td>
      <td>67</td>
    </tr>
  </tbody>
</table>
</div>




```python
# ---District Summary ---
'''table of key district metrics including: Total Schools, Total Students, Total Budget, Average Math Score,
Average Reading Score, % Passing Math, % Passing Reading, Overall Passing Rate (Average of the above two)'''

schoolCt = schools_df['School ID'].count()
studentCt = students_df['name'].count()
SumTotalBudg = schools_df['budget'].sum()
AvgMath = students_df['math_score'].mean()
AvgRead = students_df['reading_score'].mean()
#variables only used to get percentages
MathPassNum = students_df[students_df.math_score >= 60].count()
ReadPassNum = students_df[students_df.reading_score >= 60].count()
#getting percentages
MathPassPerc = (MathPassNum / studentCt) * 100
ReadPassPerc = (ReadPassNum / studentCt) * 100
OverallPass = (MathPassPerc + ReadPassPerc) / 2

#Setting up the table, 
DistMetric_df = pd.DataFrame({'Total Schools':schoolCt, 'Total Students':studentCt, 'Total Budget':SumTotalBudg,\
    'Average Math Score':AvgMath, 'Average Reading Score':AvgRead, '% Passing Math':MathPassPerc,\
    '% Passing Reading':ReadPassPerc, 'Overall Passing Rate':OverallPass})
#resetting index to numbers,
DistMetric_df.reset_index(drop=True, inplace = True)
#rearranging the order,
DistMetric_df = DistMetric_df[['Total Schools', 'Total Students', 'Total Budget', 'Average Math Score',\
    'Average Reading Score', '% Passing Math', '% Passing Reading', 'Overall Passing Rate']]
#and getting first row of table
DistMetric_df.head(1)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>24649428</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>92.445749</td>
      <td>100.0</td>
      <td>96.222875</td>
    </tr>
  </tbody>
</table>
</div>




```python
# ---School Summary---
'''merge schools_df with new variables: Average Math Score, Average Reading Score,
% Passing Math, % Passing Reading, Overall Passing Rate (Average of the above two)'''

#initializing groupby of students_df, getting student count and average scores for M and R
GroupStuSch = students_df.groupby('school')
StudentCt = GroupStuSch['Student ID'].count()
PerStuBudg = schools_df['budget'] / StudentCt
AvgScoresM = GroupStuSch['math_score'].mean()
AvgScoresR = GroupStuSch['reading_score'].mean()

#Getting the count of all passing students in both math and reading
SchoolPassM = students_df[students_df.math_score >= 60].groupby('school').count() 
SchoolPassR = students_df[students_df.reading_score >= 60].groupby('school').count()

#Getting pass % 
SchPerPassM = (SchoolPassM['math_score'] / StudentCt) * 100
SchPerPassR = (SchoolPassR['reading_score'] / StudentCt) * 100
SchOverall = (SchPerPassM + SchPerPassR) / 2

#concatenate schools_df
frames = [PerStuBudg, AvgScoresM, AvgScoresR, SchPerPassM, SchPerPassR, SchOverall]
SchScores= pd.concat(frames, axis=1)
SchScores.columns = ['Per Student Budget', 'Average Math Score', 'Average Reading Score', '% Passing Math', '% Passing Reading', 'Overall Passing Rate']
#merge with schools_df, clean up, and format
SchSummary = schools_df.join(SchScores)
del SchSummary['School ID']
SchSummary = SchSummary.rename(columns = {'type':'School Type', 'size':'Total Students', 'budget':'Total School Budget'})
SchSummaryFrmt = SchSummary.sort_index(axis=0) #sort school name (aka. the index) in alphabetical order  
SchSummaryFrmt = SchSummaryFrmt.style.set_properties(**{'text-align': 'right'})
SchSummaryFrmt

```




<style  type="text/css" >
    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col8 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col0 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col1 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col2 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col3 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col4 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col5 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col6 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col7 {
            text-align:  right;
        }    #T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col8 {
            text-align:  right;
        }</style>  
<table id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" >school</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row0" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col0" class="data row0 col0" >District</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col1" class="data row0 col1" >4976</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col2" class="data row0 col2" >3124928</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col3" class="data row0 col3" >628</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col4" class="data row0 col4" >77.0484</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col5" class="data row0 col5" >81.034</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col6" class="data row0 col6" >89.5297</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col7" class="data row0 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row0_col8" class="data row0 col8" >94.7649</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row1" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col0" class="data row1 col0" >Charter</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col1" class="data row1 col1" >1858</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col2" class="data row1 col2" >1081356</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col3" class="data row1 col3" >582</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col4" class="data row1 col4" >83.0619</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col5" class="data row1 col5" >83.9758</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col6" class="data row1 col6" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col7" class="data row1 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row1_col8" class="data row1 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row2" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col0" class="data row2 col0" >District</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col1" class="data row2 col1" >2949</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col2" class="data row2 col2" >1884411</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col3" class="data row2 col3" >639</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col4" class="data row2 col4" >76.7118</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col5" class="data row2 col5" >81.158</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col6" class="data row2 col6" >88.4368</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col7" class="data row2 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row2_col8" class="data row2 col8" >94.2184</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row3" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col0" class="data row3 col0" >District</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col1" class="data row3 col1" >2739</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col2" class="data row3 col2" >1763916</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col3" class="data row3 col3" >644</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col4" class="data row3 col4" >77.1026</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col5" class="data row3 col5" >80.7463</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col6" class="data row3 col6" >89.3027</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col7" class="data row3 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row3_col8" class="data row3 col8" >94.6513</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row4" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col1" class="data row4 col1" >1468</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col2" class="data row4 col2" >917500</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col3" class="data row4 col3" >625</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col4" class="data row4 col4" >83.3515</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col5" class="data row4 col5" >83.8168</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col6" class="data row4 col6" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col7" class="data row4 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row4_col8" class="data row4 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row5" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col0" class="data row5 col0" >District</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col1" class="data row5 col1" >4635</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col2" class="data row5 col2" >3022020</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col3" class="data row5 col3" >652</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col4" class="data row5 col4" >77.2898</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col5" class="data row5 col5" >80.9344</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col6" class="data row5 col6" >89.0831</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col7" class="data row5 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row5_col8" class="data row5 col8" >94.5415</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row6" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col0" class="data row6 col0" >Charter</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col1" class="data row6 col1" >427</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col2" class="data row6 col2" >248087</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col3" class="data row6 col3" >581</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col4" class="data row6 col4" >83.8033</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col5" class="data row6 col5" >83.815</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col6" class="data row6 col6" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col7" class="data row6 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row6_col8" class="data row6 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row7" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col0" class="data row7 col0" >District</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col1" class="data row7 col1" >2917</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col2" class="data row7 col2" >1910635</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col3" class="data row7 col3" >655</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col4" class="data row7 col4" >76.6294</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col5" class="data row7 col5" >81.1827</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col6" class="data row7 col6" >88.8584</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col7" class="data row7 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row7_col8" class="data row7 col8" >94.4292</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row8" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col0" class="data row8 col0" >District</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col1" class="data row8 col1" >4761</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col2" class="data row8 col2" >3094650</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col3" class="data row8 col3" >650</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col4" class="data row8 col4" >77.0725</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col5" class="data row8 col5" >80.9664</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col6" class="data row8 col6" >89.1829</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col7" class="data row8 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row8_col8" class="data row8 col8" >94.5915</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row9" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col0" class="data row9 col0" >Charter</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col1" class="data row9 col1" >962</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col2" class="data row9 col2" >585858</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col3" class="data row9 col3" >609</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col4" class="data row9 col4" >83.8399</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col5" class="data row9 col5" >84.0447</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col6" class="data row9 col6" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col7" class="data row9 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row9_col8" class="data row9 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row10" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col0" class="data row10 col0" >District</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col1" class="data row10 col1" >3999</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col2" class="data row10 col2" >2547363</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col3" class="data row10 col3" >637</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col4" class="data row10 col4" >76.8427</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col5" class="data row10 col5" >80.7447</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col6" class="data row10 col6" >88.5471</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col7" class="data row10 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row10_col8" class="data row10 col8" >94.2736</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row11" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col0" class="data row11 col0" >Charter</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col1" class="data row11 col1" >1761</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col2" class="data row11 col2" >1056600</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col3" class="data row11 col3" >600</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col4" class="data row11 col4" >83.3595</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col5" class="data row11 col5" >83.7257</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col6" class="data row11 col6" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col7" class="data row11 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row11_col8" class="data row11 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row12" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col0" class="data row12 col0" >Charter</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col1" class="data row12 col1" >1635</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col2" class="data row12 col2" >1043130</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col3" class="data row12 col3" >638</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col4" class="data row12 col4" >83.4183</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col5" class="data row12 col5" >83.8489</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col6" class="data row12 col6" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col7" class="data row12 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row12_col8" class="data row12 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row13" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col0" class="data row13 col0" >Charter</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col1" class="data row13 col1" >2283</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col2" class="data row13 col2" >1319574</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col3" class="data row13 col3" >578</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col4" class="data row13 col4" >83.2742</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col5" class="data row13 col5" >83.9895</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col6" class="data row13 col6" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col7" class="data row13 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row13_col8" class="data row13 col8" >100</td> 
    </tr>    <tr> 
        <th id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31level0_row14" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col0" class="data row14 col0" >Charter</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col1" class="data row14 col1" >1800</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col2" class="data row14 col2" >1049400</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col3" class="data row14 col3" >583</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col4" class="data row14 col4" >83.6822</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col5" class="data row14 col5" >83.955</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col6" class="data row14 col6" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col7" class="data row14 col7" >100</td> 
        <td id="T_5c9eb7c8_ca93_11e7_b320_fc45963dae31row14_col8" class="data row14 col8" >100</td> 
    </tr></tbody> 
</table> 




```python
# **Top Performing Schools (By Passing Rate)**
'''Create a table that highlights the top 5 performing schools based on Overall Passing Rate. Include:
  * School Name * School Type * Total Students * Total School Budget * Per School Budget * Average Math Score
  * Average Reading Score * % Passing Math * % Passing Reading * Overall Passing Rate (Average of the above two)
'''
TopSchs = SchSummary.sort_values('Overall Passing Rate', ascending=False)
TopSchs.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>600.0</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Bottom Performing Schools (By Passing Rate)**
'''Create a table that highlights the top 5 performing schools based on Overall Passing Rate. Include:
  * School Name * School Type * Total Students * Total School Budget * Per School Budget * Average Math Score
  * Average Reading Score * % Passing Math * % Passing Reading * Overall Passing Rate (Average of the above two)
'''
BotSchs = SchSummary.sort_values('Overall Passing Rate')
BotSchs.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>88.436758</td>
      <td>100.0</td>
      <td>94.218379</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>88.547137</td>
      <td>100.0</td>
      <td>94.273568</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>88.858416</td>
      <td>100.0</td>
      <td>94.429208</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>89.083064</td>
      <td>100.0</td>
      <td>94.541532</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>89.182945</td>
      <td>100.0</td>
      <td>94.591472</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Math Scores by Grade**
'''Create a table that lists the average Math Score for students of each grade level 
(9th, 10th, 11th, 12th) at each school.'''

#create pivot table of average math scores for each school by grade
tableM = pd.pivot_table(students_df, values='math_score', index=['school'], columns=['grade'], aggfunc=np.mean)
#--FYI: pivot_table's aggfunc argument defaults to mean
#fixing the order of the grade so that '9th' isn't last
grad_order = ['9th', '10th', '11th', '12th']
tableM_frmt = tableM.reindex(columns = grad_order)

tableM_frmt


```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>grade</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Reading Scores by Grade**
'''Create a table that lists the average Reading Score for students of each grade level
(9th, 10th, 11th, 12th) at each school.'''

#create pivot table of average reading scores for each school by grade
tableR = pd.pivot_table(students_df, values= 'reading_score', index='school', columns='grade')
#format so '9th' column isn't at the end, using previous list [grad_order]
tableR_frmt = tableR.reindex(columns = grad_order)
tableR_frmt

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>grade</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Scores by School Spending**
'''Create a table that breaks down school performances based on average Spending Ranges (Per Student). 
Use 4 reasonable bins to group school spending. Include in the table each of the following:
* Average Math Score * Average Reading Score * % Passing Math * % Passing Reading * Overall Passing Rate (Average of the above two)'''


SchSpendSumma = SchSummary[['Average Math Score', 'Average Reading Score', '% Passing Math',\
                            '% Passing Reading', 'Overall Passing Rate', 'Per Student Budget']].copy()


#creating bins and applying to new dataframe
binsSpPerStu = [0, 600, 625, 650, 675]
groupNameBin = ['<600', '600-625', '625-650', '650-675']

SchSpendSumma['Spending Ranges (Per Student)'] = pd.cut(SchSummary['Per Student Budget'], binsSpPerStu, right=False, labels=groupNameBin)


tableASPS = pd.pivot_table(SchSpendSumma, values = ['Average Math Score', 'Average Reading Score',\
    '% Passing Math', '% Passing Reading', 'Overall Passing Rate'], index = 'Spending Ranges (Per Student)' )
column_order = ['Average Math Score', 'Average Reading Score', '% Passing Math', '% Passing Reading',\
            'Overall Passing Rate']
tableASPS = tableASPS.reindex(columns = column_order)
tableASPS
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Ranges (Per Student)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;600</th>
      <td>83.455399</td>
      <td>83.933814</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>600-625</th>
      <td>83.599686</td>
      <td>83.885211</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>625-650</th>
      <td>79.079225</td>
      <td>81.891436</td>
      <td>92.636050</td>
      <td>100.0</td>
      <td>96.318025</td>
    </tr>
    <tr>
      <th>650-675</th>
      <td>76.997210</td>
      <td>81.027843</td>
      <td>89.041475</td>
      <td>100.0</td>
      <td>94.520737</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Scores by School Size**
'''Repeat the above breakdown, but this time group schools based 
on a reasonable approximation of school size (Small, Medium, Large).'''
#Min/Max Values for SchSummary['Total Students']: max == 4976, min == 427
binsSchSize = [0, 1500, 3000, 5000]
groupNames = ['Small (<1500)', 'Medium (1500-3000)', 'Large (3000-5000)']

SchSizeSumma = SchSummary[['Total Students', 'Average Math Score', 'Average Reading Score', '% Passing Math',\
                            '% Passing Reading', 'Overall Passing Rate']].copy()

SchSizeSumma['School Size'] = pd.cut(SchSpendSumma['Total Students'], binsSchSize, right=False, labels=groupNames)

table_SchSize = pd.pivot_table(SchSizeSumma, values=['Average Math Score', 'Average Reading Score', '% Passing Math',\
                            '% Passing Reading', 'Overall Passing Rate'], index='School Size')

table_SchSize = table_SchSize.reindex(columns=column_order)
table_SchSize
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small (&lt;1500)</th>
      <td>83.664898</td>
      <td>83.892148</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Medium (1500-3000)</th>
      <td>80.904987</td>
      <td>82.822740</td>
      <td>95.824730</td>
      <td>100.0</td>
      <td>97.912365</td>
    </tr>
    <tr>
      <th>Large (3000-5000)</th>
      <td>77.063340</td>
      <td>80.919864</td>
      <td>89.085722</td>
      <td>100.0</td>
      <td>94.542861</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Scores by School Type**
#* Repeat the above breakdown, but this time group schools based on school type (Charter vs. District).

#binsType = ['Charter', 'District']

SchTypeSumma = SchSummary[['School Type', 'Average Math Score', 'Average Reading Score', '% Passing Math',\
                            '% Passing Reading', 'Overall Passing Rate']].copy()
table_SchType = pd.pivot_table(SchTypeSumma, values = ['Average Math Score', 'Average Reading Score', '% Passing Math',\
                            '% Passing Reading', 'Overall Passing Rate'],\
                              index='School Type')
table_SchType = table_SchType.reindex(columns=column_order)
table_SchType
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>88.991533</td>
      <td>100.0</td>
      <td>94.495766</td>
    </tr>
  </tbody>
</table>
</div>


