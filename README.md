I’ve always been passionate about how data can tell a story and solve real-world problems 🌍. I came across a fascinating HR analytics dataset online that included a case study with a clear set of challenges: how to best allocate a $1000 wellness bonus, calculate a specific wage increase for non-smokers from a $983,221 insurance budget, and, most importantly, uncover the hidden patterns behind employee absenteeism.
I saw this as the perfect opportunity to challenge myself 💪. I decided to take on the role of a BI Analyst and build a complete project from scratch, aiming to solve every problem presented in the case study.

**Step 1: Making Sense of the Data with SQL**

The provided data was raw and split across multiple files—an employee list, absence logs, and reasons for absence 📂. Before any analysis was possible, I needed to bring order to this chaos.
I rolled up my sleeves and got to work with SQL 💻. I wrote a series of queries to clean the information, correcting inconsistencies and formatting errors. Then, I carefully joined the different tables, linking each absence record to the specific employee and the reason provided. This process was like assembling a puzzle 🧩; by the end of it, I had created a clean, unified, and analysis-ready dataset. This was the bedrock of the entire project.

**Step 2: Bringing the Story to Life with Power BI**

With a solid data foundation in place, I moved into Power BI 🚀. The case study included a wireframe, which I used as my blueprint 🗺️. My goal wasn't just to create static charts, but to build an interactive tool that could dynamically answer the questions posed in the case study.
I began by creating visuals like donut charts to see the employee breakdown and line charts to track absence trends. To tackle the specific budget calculations, I wrote my own DAX measures ➕➖. One of the key formulas I developed calculated the precise $0.68/hr wage increase for all non-smoking employees, directly addressing one of the core tasks.

**Step 3: The "Aha!" Moments**

This was the most rewarding part of the project 🌟. As the dashboard came together and I started interacting with it, the story in the data began to emerge.
The patterns were striking 🎯. A line chart immediately revealed a significant spike in absences every Monday 🗓️. Another chart showed that absenteeism wasn't steady throughout the year but had clear peaks in the spring and mid-summer months. By drilling down into the reasons for absence, it became obvious that "Medical consultation" was the leading cause. The data was no longer just a collection of rows; it was a living story of workplace behavior.

**The Result**

The final result was a fully interactive dashboard that successfully solved every challenge laid out in the original case study ✅.
For me, this project was a powerful hands-on experience. It allowed me to manage an entire analytics workflow—from raw data to actionable insight—and solidified my skills in using SQL and Power BI to transform a complex business problem into a clear, visual solution 💡.

SQL Query used:
-- create a joint table
select * from Absenteeism_at_work a
left join [dbo].[compensation] b
on a.ID = b.ID
left join [dbo].[Reasons] c
on a.Reason_for_absence = c.Number

---find healthiest for the bonus
select * from Absenteeism_at_work
where Social_drinker = 0 and Social_smoker = 0
and Body_mass_index <25 and Absenteeism_time_in_hours < (select AVG(Absenteeism_time_in_hours) from Absenteeism_at_work)

--- comp rate increase for non smokers/ budget $983,221, rate = 0.68 rate per hr, $1414.4 per year for a non smoker employee
select count(*) as nonsmokers from Absenteeism_at_work
where Social_smoker = 0

--optimize query
select
a.ID,c.Reason,Month_of_absence,
Body_mass_index,
CASE WHEN Body_mass_index <18.5 then 'Underweight'
     WHEN Body_mass_index between 18.5 and 25 then 'Healthy Weight'
     WHEN Body_mass_index between 25 and 30 then 'Over Weight'
     WHEN Body_mass_index >30 then 'Obese'
     ELSE 'UNKNOWN' end as BMI_Category, 
CASE WHEN Month_of_absence IN (12,1,2) Then 'Winter'
     WHEN Month_of_absence IN (3,4,5) Then 'Spring'
     WHEN Month_of_absence IN (6,7,8) Then 'Summer'
     WHEN Month_of_absence IN (9,10,11) Then 'Fall'
     ELSE 'UNKNOWN' END as Season_Names,
Month_of_absence, 
Day_of_the_week,
Transportation_expense,
Education,
Son,
Social_drinker,
Social_smoker,
Pet,
Disciplinary_failure,
Age,
Work_load_Average_day,
Absenteeism_time_in_hours
from Absenteeism_at_work a
left join [dbo].[compensation] b
on a.ID = b.ID
left join [dbo].[Reasons] c
on a.Reason_for_absence = c.Number

**Here is the final Dashboard ** ![Image Alt](https://github.com/KBLovesme/HR-Analytics-Dashboard/blob/c177127ea3c59d02715f27e4753bda9790570338/Dashboard.png)
