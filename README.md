# The Analysis:
## 1. What are the most demanded skill for the top 3 most popular data roles?

To find the most demanded skill for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these tiop 3 roles. This query highlights the most popular job tille and their top skills, showing which skills I should pay attention to depending on the role I am targeting.

View my notebook with detailed steps here:
[2_Skills_Count.ipynb](3_Project/2_Skills_Count.ipynb)

### Code for Visualizing Data

```python
fig, ax = plt.subplots(len(job_titles),1)
sns.set_theme(style='ticks', rc={'figure.figsize':(10,10)})

for i, job_title in enumerate(job_titles):
   df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
   sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r', )
   ax[i].set_title(job_title)
   ax[i].set_ylabel('')
   ax[i].set_xlabel('')
   ax[i].get_legend().remove()
   ax[i].set_xlim(0,78)

   for n, v in enumerate(df_plot['skill_percent']):
      ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

   if i != len(job_titles) - 1:
      ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requsted in U.S. Job Postings', fontsize=20)
fig.tight_layout()
plt.show()
```

### Results

![likelihood skills data role](3_Project/images/likelihood_skills_data_role.png)


### Insights

- As an analyst myself it was interesting to look at the top skills and see that 3 of top 5 are tools/software that I am proficient with as is.
- I found it interesting how important python is for the 2 more advanced roles, but stil makes its way in there for analysts. I can see that perecentage increasing as time goes on making it important to be learning python now if you don't know it already.
- The last thing I found interesting is how analysts seem to be expected to know more general skills and overall just be proficient in those. But the more advanced roles, especially engineers seem to require more specialized tools.

## 2. How are in-demand skills trending for Data Analysts?

To look at this I created a new data frame to look at just Data Analyst jobs in the United States. The plan was to make a Line Chart to plot the change over the course of the year. So, I made a new column for the Month No. Then exploded the skills column. So, that I could get the count of the mentions for each skill. Then created a totals row to sum those counts for each month up. This allowed me to sort on that Total row to find the top 5 skills. From there the Total row was deleted and I created a new index that was the short form of each month and deleted the Month No. column. This helps give the visualization a better look. Then the data was plotted.

View my notebook with detailed steps here:
[3_Skills_Trend.ipynb](3_Project/3_Skills_Trend.ipynb)

### Visualize Data

```python
df_plot = df_DA_US_percent.iloc[:, :5]

sns.lineplot(data=df_plot, dashes=False, palette="tab10")
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the U.S.')
plt.xlabel('2023')
plt.ylabel('Likelihood in Job Posting')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax=plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])

plt.show()
```

### Results

![trending skills data analysts](3_Project\images\trending_skills_data_analysts.png)

### Insights

- Overall SQL is the most in-demand skill and did appear to have a slight decline throughout the year, but with it having a signficantly higher likelihood than the other top 5. It is safe to say SQL is here to stay and is easily the most in demand skill for Data Analysts.
- Overall the other top 5 seem to be pretty steady when looking at demand. Excel being the only one with some weird spikes either way, but based on those increase and decreases it is pretty steady.
- On key thing I did notice is that Python does seem to be on a relatively steady incline. So, something to keep and eye out for in the coming years. Python very well may be a standard thing employers are looking for in Analysts.

## 3. How well do jobs and skills pay for Data Analysts?

To gather this data for plotting I first created a dataframe that was specific to the United States and then dropped any rows where the 'salary_year_avg' column had NaN values. Then I created a list of the top 6 job titles with the most job postings from that data frame that we could then use to filter and create a top 6 data frame. From there I created a job order list that we could pass into the seaborn plot. Then I plotted the data and formatted the visualization.

View my notebook with detailed steps here:
[4_Salary_Analysis.ipynb](3_Project/4_Salary_Analysis.ipynb)

### Visualize Data

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y="job_title_short", order=job_order)

plt.title('Salary Distributions in the United States')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
plt.xlim(0, 600000)
ticks_x=plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### Results
![median salaries for top 6 roles](3_Project\images\top6_median_salaries.png)

### Insights

- The first thing that caught my eye with this was that all the Senior roles were not at the not at the top. While it does make sense as the Engineer and Scientist roles may require more specialized skills equalling higher pay. Just something to note if you are an Analyst and thinking about becoming a Senior Analyst. Maybe you would be better off trying to be an Engineer or Scientist
- Another thing is some of the more outlying findings for the Data Scientist. There is a job posting that pays almost $600k per year and that is not a senior role. Would be interesting to see what company that is and what that roles entails.
- The final thing is some of the outliers on the lower end of the pay scale. 2 of the Senior Roles(Sr. Data Analyst & Sr. Data Engineer) have some job postings where the person being hired would be relatively underpaid compared to what plenty of other roles are hiring for.

## 4. 