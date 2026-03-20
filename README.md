# Overview
This data analytics project explores the current job market for data analyst roles, offering actionable insights for job seekers and career planners. Leveraging a comprehensive dataset from [Luke Barousse's Python Course](https://lukebarousse.com/python), this analysis investigates salary trends, in-demand skills, and the intersection between compensation and market demand.

The dataset includes detailed information on job titles, salaries, locations, and required skills, providing a robust foundation for answering key questions such as:

- Which skills are most frequently requested by employers?
- How do salaries vary across different skills and locations?
- What skills offer the best balance between high demand and high compensation?

Through a series of Python-powered analytical scripts, this project transforms raw job posting data into meaningful insights—helping data professionals identify the most promising opportunities and strategically position themselves in a competitive market.

## Research Questions
This project seeks to answer the following key questions about the data analytics job market:

1. What are the most in-demand skills for the top three most popular data roles?
Identifying the technical competencies employers prioritize most frequently across different data positions.
2. How are skill requirements trending for Data Analysts over time?
Tracking the evolution of demanded skills to reveal emerging technologies and declining ones.
3. What is the relationship between skills, job roles, and salary levels for Data Analysts?
Analyzing how different skills and positions correlate with compensation.
4. Which skills offer the optimal balance between high demand and high salary?
Pinpointing the most strategic skills for Data Analysts to learn—those that are both frequently requested and highly compensated.

## Tools I Used
For this deep dive into the data analyst job market, I leveraged a powerful stack of modern data analytics tools:

| **Tool/Library** | **Purpose** |
|:-----------------|:------------|
| **Python** | The core programming language powering my entire analysis—from data manipulation to insight generation. |
| **Pandas** | Used for data cleaning, transformation, and exploratory analysis. |
| **Matplotlib** | Created foundational visualizations to illustrate key trends and patterns. |
| **Seaborn** | Enabled more sophisticated and aesthetically pleasing statistical visualizations. |
| **Jupyter Notebooks** | Provided an interactive environment to run Python scripts while seamlessly integrating code, visualizations, and documentation. |
| **Visual Studio Code** | Served as my primary integrated development environment (IDE) for writing and executing Python scripts. |
| **Git & GitHub** | Essential for version control, code sharing, and project collaboration—ensuring transparency and reproducibility throughout the analysis lifecycle. |

## Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.
### Import & Cleanup Data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.
```python
# Import Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt

# Load Data
dataset = load_dataset("lukebarousse/data_jobs")
df = dataset["train"].to_pandas()

# Data Cleanup
df["job_posted_date"] = pd.to_datetime(df["job_posted_date"])
df["job_skills"] = df["job_skills"].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
### Filter US Jobs
```python
# Filter for only United States in the job_country
df_US = df[df["job_country"] == "United States"]
```

## The Analysis
Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

### 1. What are the most in-demand skills for the top three most popular data roles?
To identify the most sought-after skills for leading data roles, I isolated the top three most frequently posted positions and extracted their five most common required skills. This targeted analysis reveals the key competencies needed for each career path, enabling strategic skill development based on specific role aspirations.

View my notebook with detailed steps here:[ 2_Skill_Demand.](3_Project/2_Skill_Demand.ipynb)

Data Visualization
```python
# Plotting the data
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style="ticks")

# Iterate through the job_titles
for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc["job_title_short"] == job_title].head(5)  # Filter the job_title_short of the df_skills_count to only show job titles for the top 3 roles by the top 5 skills
    sns.barplot(data=df_plot, x="skill_percent", y="job_skills", ax=ax[i], hue="skill_percent", palette="dark:b_r")    
    ax[i].set_xlabel("")
    ax[i].set_xlim(0, 78)
    ax[i].set_ylabel("")
    ax[i].legend().set_visible(False)

    # Loop through and set titles for the data using text
    for n, v in enumerate(df_plot["skill_percent"]):
        ax[i].text(v + 1, n, f"{v:.0f}%", va="center")

    # remove the x label for the top 2 charts
    if i != len(job_titles) -1:
        ax[i].set_xticks([])

fig.suptitle("What is the Likelihood of Skills Requested in US job postings", fontsize=15)
plt.tight_layout()  # Fix the overlap
plt.show()
```

Results

![Skills Demand Plot](3_Project/images/likelihood_of_skills_demand_in_us_jobs.png)

*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

**Insights:**

- Data Analysts face a diverse but focused skill landscape, with SQL dominating at 51%—reinforcing its position as the non-negotiable foundation for the role. Excel follows closely at 41%, highlighting its continued relevance for business-facing analytics, while visualization tools like Tableau (28%) and programming languages like Python (27%) show nearly equal importance, suggesting employers value a hybrid of traditional and modern analytical tools.

- Data Engineers command a cloud-centric skillset, with SQL (68%) and Python (65%) as near-ubiquitous requirements, reflecting the need for strong database and scripting capabilities. The prominent presence of AWS (43%), Azure (32%), and Spark (32%) underscores the industry's shift toward cloud-native data engineering and distributed computing, making cloud platform expertise essential for this role.

- Data Scientists prioritize Python above all (72%) , establishing it as the undisputed lingua franca of advanced analytics. SQL (51%) remains critical for data extraction, while R (44%) maintains a strong foothold, suggesting a continued appreciation for statistical computing. The lower but meaningful demand for SAS and Tableau (both 24%) indicates that while specialized tools matter, foundational programming and database skills remain paramount.

### 2. How are skill requirements trending for Data Analysts over time?
To analyze skill trends for Data Analysts throughout 2023, I filtered for data analyst roles and aggregated required skills by posting month. This longitudinal analysis tracks the monthly prevalence of the top five skills, revealing their popularity trajectories over the year.

View my notebook with detailed steps here: [3_Skills_Trend](3_Project/3_Skills_Trend.ipynb)

Data Visualization
```python
df_plot = df_DA_US_percent.iloc[:, :5]

sns.lineplot(data=df_plot, dashes=False, palette="tab10")
sns.set_theme(style="ticks")
sns.despine()

plt.title("Trending Top Skills for Data Analysts in the US")
plt.ylabel("Likelihood in Job Posting")
plt.xlabel("2023")
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])

plt.show()
```

Results

![Skills Trend Plot](/3_Project/images/Trending_Top_Skills_for_DA_in%20the%20US.png)

*Bar graph visualizing the trending top skills for data analysts in the US in 2023.*

**Insights:**

- SQL maintains dominance but shows a gradual decline throughout 2023, dropping from 64% in January to 48% by December—a 16 percentage point decrease. Despite this downward trend, SQL remains the most requested skill every month, reinforcing its foundational importance for Data Analysts even as the skill landscape diversifies.

- Tableau experiences remarkable growth mid-year, surging from 25% in January to a peak of 45% in July—an 80% increase in demand. This dramatic rise suggests a mid-2023 shift toward prioritizing data visualization expertise, though it moderates slightly in subsequent months while remaining elevated above年初 levels.

- Python and Excel show remarkable stability throughout the year, with Python consistently hovering between 33-39% and Excel maintaining a steady 41-49% range. This consistency indicates these tools have become baseline expectations for Data Analysts, while Power BI shows modest but steady growth—climbing from 18% to 25% by year-end—suggesting increasing adoption of Microsoft's visualization platform.

### 3. How well do jobs and skills pay for Data Analysts?
To pinpoint the most lucrative roles and skills, I filtered for jobs located in the United States and analyzed median salary distributions across key data positions—Data Scientist, Data Engineer, and Data Analyst. This approach established a baseline understanding of which roles command the highest compensation before diving into skill-level salary analysis.


View my notebook with detailed steps here: [4_Salary_Analysis](/3_Project/4_Salary_Analysis.ipynb)

Data Visualization 
```python
sns.boxplot(data=df_US_top6, x="salary_year_avg", y="job_title_short", order=job_order)

ax =plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}K"))

plt.title("Salary Distribution in the United States")
plt.ylabel("")
plt.xlabel("Yearly Salary ($USD)")
plt.xlim(0, 600000)

plt.show()
```


Results

![Salary Distribution for Data Jobs](/3_Project/images/salary_distribution_in_us.png)

*Box plot visualizing the salary distributions for the top 6 data job titles.*


Insights

- Senior roles command significantly higher compensation, with Senior Data Scientists and Senior Data Engineers earning median salaries well above $150K, while their non-senior counterparts cluster closer to the $100K–$120K range. The upward shift underscores the substantial salary progression associated with experience and seniority in data roles.

- Data Engineers and Data Scientists show comparable earning potential, with both roles exhibiting similar median salary ranges and upper quartiles extending toward the $200K mark. This parity suggests that technical specialization in either engineering or advanced analytics yields similarly lucrative career outcomes.

- Data Analysts have the widest salary distribution among all roles, with lower quartiles starting near $70K and upper outliers reaching beyond $150K. This broad spread reflects the diversity of Data Analyst positions—ranging from entry-level business analytics roles to highly specialized analytical positions that can command salaries competitive with more senior data roles.


### Highest Paid & Most Demanded Skills for Data Analysts
To refine my analysis, I narrowed the focus exclusively to Data Analyst roles and examined both the highest-paying skills and the most in-demand skills. The findings are presented through two distinct bar charts—one highlighting top salaries, the other showcasing demand frequency.


Data Visualization
```python
fig, ax = plt.subplots(2, 1)

# Set theme
sns.set_theme(style="ticks")

# Top 10 Highest Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x="median", y=df_DA_top_pay.index, ax=ax[0], hue="median", palette="dark:b_r", legend=False)
# ax[0].legend().remove() # remove the legend

# df_DA_top_pay[::-1].plot(kind="barh", y="median", ax=ax[0], legend=False) # df_Da_top_pay[::-1] inverts the df
# ax[0].invert_yaxis() # invert the plot
ax[0].set_title("Top 10 Highest Paid Skills for Data Analysts")
ax[0].set_ylabel("")
ax[0].set_xlabel("")
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}K"))


# Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data=df_DA_skills, x="median", y=df_DA_skills.index, ax=ax[1], hue="median", palette="light:b", legend=False)
# ax[0].legend().remove() # remove the legend

# df_DA_skills[::-1].plot(kind="barh", y="median", ax=ax[1], legend=False)
ax[1].set_xlim(ax[0].get_xlim())
ax[1].set_title("Top 10 Most In-Demand Skills for Data Analysts")
ax[1].set_ylabel("")
ax[1].set_xlabel("Median Salary (USD)")
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}K"))

fig.tight_layout()
plt.show()
```

Results

Here's the breakdown of the highest-paid & most in-demand skills for data analyst in the US:

![Highest Paid & Most Demanded Skills](/3_Project/images/highest_paid_most_demanded_skills.png)


*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US.*

Insights:

- There is a striking disconnect between the highest-paid and most in-demand skills for Data Analysts. Skills like dplyr, bitbucket, and gitlab command top salaries exceeding $150K, yet they do not appear among the most frequently requested skills. This suggests that niche or specialized tools can yield premium compensation despite having lower overall market demand.

- Python and Tableau demonstrate strong value as foundational skills with broad demand. Both appear in the top 10 most in-demand skills while also ranking among the higher-paid categories, indicating they offer a favorable balance of opportunity and earning potential for Data Analysts seeking versatile career paths.

- Traditional office tools like Excel, PowerPoint, and Word show high demand but rank at the bottom for salary potential. While these skills remain frequently requested—particularly Excel—they consistently fall in the lowest compensation tiers, suggesting they are considered baseline expectations rather than differentiators for higher-paying roles.


### 4. What are the most optimal skills to learn for Data Analysts?
