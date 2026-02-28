# Overview 
Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from 

# The Questions 

Below are the questions I want to answer in my project:

1. What are the skills most in-demand for the top 3 popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand and High Paying) 

# Tools I Used 

For my deep dive in the analyst job market, I harnessed the power of several key tools: 

- Python: The backbone of my analysis, allowing me to analyze the data and find critical insights. I also used the following Python libraries:
    - Pandas Library: This was used to analyze data 
    - Matplotlib Library: To visualize the data 
    - Seaborn Library: Helped me create more advanced and detailed visuals



# The Analysis 
## 1 . What are the most demanded skills for the top 3 most popular data roles ?
To find the most demanded skills for the top 3 most popular data roles , I filtered out those positions which were the most popular and got the top 5 skills for these top 3 data roles. This shows which skills should i pay attention to depending on my target role.  

View my notebook with detailed steps here: 
[2_skills_demand.ipynb](3_Project\2_skills_demand.ipynb) 

### Visualize Data
``` python 
fig, ax = plt.subplots( len(job_titles) , 1)
sns.set_theme(style = 'ticks')



for i,job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot( data = df_plot , x = 'skill_percent' , y = 'job_skills' , ax = ax[i] , hue = 'skill_count' , palette ='dark:b_r' ,legend = False)
    ax[i].set_title(job_title)
    ax[i].set_ylabel("")
    ax[i].set_xlabel("")
    ax[i].set_xlim(0, 70)


    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text( v +2 , n , f'{v: .0f}%' , va= 'center')
    if i != len(job_titles) - 1: 
        ax[i].set_xticks([])



fig.suptitle('Counts of Top skill in Job Postings' , fontsize = 15)
fig.tight_layout() 
plt.show()
```

### Results 

![Visualisation of top skills for most demanded data roles](3_Project\Images\demanded_skills_for_data_roles.png)
 

### Insights 
- Python and SQL clearly form the core foundation across all roles.
  Python appears in a large share of postings, reaching about 70 percent in Data Scientist roles and more than half in Data Engineer roles. SQL shows a similar pattern, especially strong among Data Engineers and Data Analysts.

- Data Analysts show higher demand for tools such as Excel and Tableau, reflecting the emphasis on reporting, dashboards, and business insights. In contrast, Data Engineers frequently require tools like Spark and cloud platforms such as AWS and Azure.

- Data Scientists lean more toward statistical and analytical tools.
  Alongside Python and SQL, languages like R appear regularly in these roles, indicating a stronger focus on statistical analysis, experimentation, and modeling compared to other data positions.

- Visualization and communication remain important across roles.
  Tools like Tableau appear in a noticeable portion of both Analyst and Scientist postings, showing that the ability to present insights clearly is valued.


## 2. How are in-demand skills trending for Data Analysts?
To find the trend line for the in demand skills for Data Analyst I created a pivot table for the skill base and then added the month name column. 

View my notebook with detailed steps here: 
[3_skills_trend.ipynb](3_Project\3_skills_trend.ipynb) 


```python 
df_plot = df_DA_india_percent.iloc[: , :5]
sns.lineplot(data = df_plot , dashes = False , palette ='tab10' , legend = False)
sns.set_theme(style = 'ticks')
sns.despine()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals = 0))

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')

for i in range(5):
    plt.text( 11.2 , df_plot.iloc[-1, i], df_plot.columns[i])

```

### Results

![Visualisation of top skills for most demanded data roles](3_Project\Images\skills_trend.png)

### Insights 
-  SQL remains consistently dominant throughout the year. It stays above 50% for most months, with only small fluctuations, showing that database querying is a stable and essential requirement. 

- Python demand is steady with a slight mid-year peak. It rises around June before stabilizing again, suggesting that Python remains important but does not fluctuate as sharply as some other tools.

- Excel shows more variation compared to SQL and Python. There are noticeable dips and spikes over the year.

- Tableau and Power BI remain secondary but relevant tools. Their percentages are lower overall, but they show periodic increases, especially toward the later months, indicating steady demand for visualization skills even if they are not the primary requirement. 

## 3. What is the distribution of different salary roles and the salary difference between top demanded skills vs the highest paying skills.

For the first part I filtered out the top 6 data roles and plotted out a boxplot from median salary. Then for the second part I created 2 dataframes one for top 10 highest paying skills and another for top 10 in demand skills by grouping through the columns.

View my notebook with detailed steps here: 
[3_Salary_Analysis
.ipynb](3_Project\4_Salary_Analysis.ipynb) 
```python
#First Part
sns.boxplot( data = df_india_top6 , x = 'salary_year_avg' , y= 'job_title_short' , palette = 'light:g' , order = job_order)
sns.set_theme( style = 'ticks')

plt.title('Salary Distributions of different data roles in India')
plt.ylabel("")
plt.xlabel('Yearly Salary (USD$)')
plt.xlim( 0 , 300000)
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
plt.show()
```

### Results(1)
![Visualisation of top skills for most demanded data roles](3_Project\Images\median_salary_of_data_roles.png)


```Python
#Second part
fig , ax = plt.subplots(2 , 1)
sns.set_theme(style = 'ticks')

#for plotting the top 10 highest paying skills 
sns.barplot( data = df_DA_top_pay , x = 'median' , y = df_DA_top_pay.index , hue = 'median' , ax=ax[0] , legend = False , palette = 'dark:b_r')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x,pos : f'${int(x/1000)}K'))
ax[0].set_title('Top 10 highest paying skills for Data Analyst')
ax[0].set_xlabel("")
ax[0].set_ylabel("")
ax[0].set_xlim(0, 170000)

#for plotting the top 10 in-demand skills 
sns.barplot( data = df_DA_top_skills , x = 'median' , y = df_DA_top_skills.index , hue = 'median' , ax=ax[1] , legend = False , palette = 'dark:b_r')
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x,pos : f'${int(x/1000)}K'))
ax[1].set_title('Top 10 in-demand skills for Data Analyst')
ax[1].set_xlabel("")
ax[1].set_ylabel("")
ax[1].set_xlim(ax[0].get_xlim())
ax[1].set_xlim(0, 170000)
fig.tight_layout()
```

### Results(2)
![Visualisation of top skills for most demanded data roles](3_Project\Images\top10_demaned_skills_vs_top10_highest_paying_skills.png)

### Insights 


# What I Learned 

Throughout this project, I deepened my understanding of data analyst job market and enhanced my technical skills in Python in data manipulation and visualization. Here are a few specific things i learned: 
- Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- Data Cleaning Importance: I learned that thorough data cleaning and preparation are crucial before any analysis should be conducted, ensuring the accuracy and efficiency in deriving the insights from the data. 

# Challenges i Faced 
This project was not without its challenges, but it provided good learning opportunities:
- Data inconsistencies: Handling missing or inconcistent data entries requires careful consideration
- Virtual Environments: setting up virtual environments for a specific project requires a set procedure which was tricky for me 
- Balancing Breadth and Width: Deciding how deeply to dive into each anlysis while maintaining a broad overview required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion

