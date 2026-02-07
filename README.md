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

