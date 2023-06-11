# Call Center Analysis | Pwc Switzerland Power BI Virtual Case Experience
![Cover for github](https://user-images.githubusercontent.com/100661121/233259968-0c733411-1ce8-467b-ad94-a66a09e58bd8.png)


# Problem Statement

- **Problem:** The manager at PhoneNow (a big telecom company) is looking for transparency and insight into the Call Center dataset to gain an accurate overview of long-term trends in customer and agent behaviour.
- **Objective:** The purpose of this analysis is to create a dashboard in Power BI for Call Center Manager that reflects all relevant Key Performance Indicators (KPIs) and metrics to:
    - Self-exploratory call trends
    - Overview the agent’s performance and behaviors
    - Overview the customer satisfaction
    - Contain many metrics and plots related to a single area of business for discussing with higher manager and generating further analysis.
    - Allows for minimal interaction
- **Possible KPIs** include (but not limited to):
    - Overall customer satisfaction
    - Overall calls answered/abandoned
    - Calls by time
    - Average speed of answer
    - Agents performance quadrant -> average handle time(talk duration) vs calls answered

# Data Sourcing

The Dataset used for this analysis was provided by [Pwc Switzerland](https://www.pwc.ch/en/careers-with-pwc/students/virtual-case-experience.html) and available at here: [Call Center Dataset](https://github.com/calmk/Call-Center-Trends-PWC-Virtual-Case-Experience/blob/main/01%20Call-Center-Dataset.xlsx)

# Data Preparation

The dataset was loaded into Microsoft Power BI Desktop for transformation in Power Query and modeling.

### Metadata

The tabulation below shows the metadata of `Call Center` dataset:

| File name |01 Call-Center-Dataset  |
| --- | --- |
| Format | .xlsx |
| Size | 249KB |
| Fields | 10 |
| Entities | 5000 |
| Time | January 1, 2021 - March 31, 2021 |

The tabulation below shows the `Call Center` table with its fields names and their description:

| Field Name | Description | Data Type |
| --- | --- | --- |
| Call Id | Represents every unique observation in the dataset | Text  |
| Agent | Describes the name of the agent | Text |
| Date | Describes the date of the call | Date |
| Time | Represents the time of the call | Date/Time |
| Topic | Describes the topic of the caller | Text |
| Answered (Y/N) | Describes if the call was Answered or not | Text |
| Resolved | Describes if the problem was Resolved or not | Text |
| Speed of answer in seconds | Represents the speed of answer in seconds | Decimal number |
| AvgTalkDuration | Represents the average talk duration of call | Time |
| Satisfaction rating | Represents the satisfaction rating of the agent during the call | Decimal number |

### Data Cleaning

Data Cleaning for the dataset was done in Power Query as follows:

- Unnecessary columns were removed
- Each of the columns in the table were validated to have the correct data type
- Unnecessary rows were removed

### Data Transformation

To ensure the comprehensive of satisfaction of customers, a additional column named `Satisfaction Likert` was created for referencing using the M-formula: 

`Table.AddColumn(#"Added Custom", "Satisfaction likert", each if [Satisfaction rating] = 1 then "Very poor" else if [Satisfaction rating] = 2 then "Poor" else if [Satisfaction rating] = 3 then "Average" else if [Satisfaction rating] = 4 then "Good" else "Very good")`

Here is a breakdown of what the formula does:

For the dataset, we want to transform the satisfaction rating from number to text based on Likert scale with the condition if `Satisfaction rating = 1`, it will display the rating was `“Very poor”`, respectively for each value of `Satisfaction rating` .

# Data Modeling

After the dataset was cleaned and transformed, it was ready to be modeled, but the dataset is just included one table, so the Data Modeling is nothing much to modify

# Data Visualization
![233076182-dedd0efc-5704-4886-b4d0-b80a841e9773](https://github.com/Pranjali-d/PWC_Virtual_case_Exp/assets/49934575/0e8a843b-ca16-4b23-a012-1a4a09a1c83f)




Data visualization for the datasets was done in Microsoft Power BI Desktop and design in PowerPoint, the dashboard includes:

- One main dashboard
- Six tooltip pages

### Dashboard type
Dashboard by level of detail: **Tactical dashboard**

Dashboard by use-case: **Exploratory**

Target audience: **Team lead & Manager** (non-technical users)

### Key Performance Indicators and metrics:

**About Calls and Agents:** 

- Overall calls answered/abandoned
- Calls recieve by time, day 
- Average speed of answer, handle duration
- Resolved rate by Agents, Topics
- Agent’s performance quadrant -> average handle time (talk duration) vs calls answered

**About Customer satisfaction:**

- Overall customer satisfaction
- Customer satisfaction distribution by Agents, Topics
### Measures

Measure used in visualization are:

- **Calculated measures:**

  - Number of answered = `Calculate(distinctcount('Call Center'[Call Id]),Filter('Call Center','Call Center'[Answered (Y/N)]="Y"))`
  - Abandoned Rate = `DIVIDE(COUNT('Call Center'[Call Id]) - [Number of Answer], COUNT('Call Center'[Call Id]))`
  - Number of resolved = `Calculate(distinctcount('Call Center'[Call Id]),Filter('Call Center','Call Center'[Resolved]="Y"))`
  - Average satisfaction rating = `Average('Call Center'[Satisfaction rating])`
  - Average Speed of answer = `Average('Call Center'[Average Speed of anser in seconds])`
  - Operation hour DAX = `FORMAT('Call Center'[Time], "hh:mm")`
  - duration = `MINUTE('Call Center'[AvgTalkDuration])*60 + SECOND('Call Center'[AvgTalkDuration])`
- **Format measures:**

  - Welcome text = `VAR Hour = HOUR(NOW())
  VAR Greeting = 
  SWITCH(
      TRUE(),
      Hour >= 0 && Hour < 5, "Good Night",
      Hour >= 5 && Hour < 12, "Good Morning",
      Hour >= 12 && Hour < 18, "Good Afternoon",
      Hour >= 18 && Hour < 24, "Good Evening"
  )
  RETURN
  Greeting & " " & "Manager!"`
  - Show filter =

      `-- Agents
      IF(
          ISFILTERED('Call Center'[Agent]),
          VAR Agents = VALUES('Call Center'[Agent])
          VAR Agentscombined = CONCATENATEX(Agents, 'Call Center'[Agent], UNICHAR(10))
          RETURN Agentscombined & UNICHAR(10)
      )&
      --Topics
      IF(
          ISFILTERED('Call Center'[Topic]),
          VAR Topics = VALUES('Call Center'[Topic])
          VAR Topicscombined = CONCATENATEX(Topics, 'Call Center'[Topic], UNICHAR(10))
          RETURN Topicscombined & UNICHAR(10)
      )`

  - Show header filter =

      `-- Header of agent
      IF( 
          ISFILTERED('Call Center'[Agent]),
          "Agent: " & REPT(UNICHAR(10), COUNTROWS(VALUES('Call Center'[Agent])))
      ) &
      -- Header of agent
      IF( 
          ISFILTERED('Call Center'[Topic]),
          "Topic: " & REPT(UNICHAR(10), COUNTROWS(VALUES('Call Center'[Topic])))
      )`

### Format using

**Font:** SF Pro Display

**Color:** Datacamp palette


# Analysis and Insights
The purpose of this dashboard is served as self-exploratory for managers, but I still note some highlighted point that I recognize below:

********************About Call trends:********************

- Customers tend to call more between 5:00 pm - 5:30pm at 250 calls received with the abandoned rate is 18.40% (approximately to the average abandoned rate) and distributed mainly in the middle of month
- The highest abadoned rate is 28.03% between 1:00pm - 1:30pm
- Customers have more problem with Streaming service
- The resolved rate is at high rate (89,94%)

********************About performance of agents:********************

- The agent satisfies customers most is Becky with 12.02% of “Very good” rating
- The agent has a highest resolved rate is Jim and he is effective with solving prolem related to “Contract related” and “Admin Support”

********************About the customer satisfaction:********************

- The average customer satisfaction is at acceptable rate with 3.40, mainly comes from “Average” (30.04%) and “Good” (29.11%) rating
- The correlation of call answered and call resolved is strongly positive which resulted in a increase of the customer satisfaction rate

# Shareable Link
You can interact and have fun with the dashboard here:

[Microsoft PowerBI](https://app.powerbi.com/view?r=eyJrIjoiZmNiNWNiZGMtZTM4MS00ZDI3LTlhNTUtODMwOTZmZDExOGI5IiwidCI6ImRmODY3OWNkLWE4MGUtNDVkOC05OWFjLWM4M2VkN2ZmOTVhMCJ9p) 


