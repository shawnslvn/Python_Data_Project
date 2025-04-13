# The Analysis:
## 1. What are the most demanded skill for the top 3 most popular data roles?

To find the most demanded skill for the top 3 most popular data roles. I filtered out thos positions by which ones were the most popular, and got the top 5 skills for these tiop 3 roles. This query highlights the most popular job tille and their top skills, showing which skills I should pay attention to depending on the role I am targeting.

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