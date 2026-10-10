# Gen Z Career Aspirations & the Employer Expectation Gap

> A survey-based data analytics project (KultureHire Data Analytics Learnternship) that analyses 3,613 Gen Z responses using Excel pivot tables, SQL and Power BI to show where career expectations and employer offers do not match, and what employers can do about it.

![Power BI Dashboard](images/powerbi_manager_aspirations.png)
*Replace with a screenshot of your Power BI dashboard.*

## Business Problem
Organisations struggle to attract and retain Gen Z employees. Gen Z is often labelled "difficult" or "disloyal", but the real issue may be a mismatch in expectations and communication. This project measures that gap with data instead of opinion.

**Objective:** understand Gen Z's career goals, influences, preferred work environment, financial expectations and values, and turn them into recommendations for employers.

## Dataset
- **Source:** primary survey (Google Form) of students and early professionals
- **Size:** 3,613 valid Gen Z responses from 1,864 unique pincodes (96.5% India-based)
- **Gender split:** 59.65% male, 40.3% female, 0.03% other
- **Key fields:** gender, aspirational career, career influence factor, work environment, preferred manager type, learning environment, expected salary (3-year and 5-year), mission clarity, mission misalignment, social impact mission, study abroad

## Tools Used
| Stage | Tool |
|---|---|
| Survey collection | Google Forms |
| Cleaning & pivot analysis | Microsoft Excel (Pivot Tables) |
| Business-question analysis | SQL (aggregations, window functions) |
| Dashboards | Excel dashboard, Power BI |
| Reporting | 5W1H problem statement (PDF), stakeholder presentation |

## Project Flow
1. Problem statement (5W1H framework)
2. Survey design and data collection
3. Data cleaning and standardising
4. Pivot analysis in Excel
5. Excel dashboard
6. SQL analysis of 5 business questions
7. Power BI dashboards
8. Insights and recommendations

## Data Cleaning
- Removed "Not Answered" and blank values so percentages reflect only valid answers
- Standardised category labels (trimmed whitespace, unified inconsistent spellings)
- Checked for duplicate submissions and confirmed 3,613 valid responses
- Reconciled gender and country coding for consistent cross-tabulation

## SQL Analysis
All queries run on the table `genz_survey`. Each returns counts and percentages, using `SUM(COUNT(*)) OVER()` to calculate each category's share of the total.

### 1. Which industries are Gen Z most interested in?
```sql
SELECT
    aspirational_carrer AS industry_interested,
    COUNT(*) AS count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS percentage
FROM genz_survey
WHERE aspirational_carrer IS NOT NULL
  AND aspirational_carrer <> 'Not Answered'
GROUP BY 1
ORDER BY count DESC;
```

### 2. What are the top factors influencing career choices?
```sql
SELECT
    carrer_influence_factor AS influence_factor,
    COUNT(*) AS count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS percentage
FROM genz_survey
WHERE carrer_influence_factor IS NOT NULL
  AND carrer_influence_factor <> 'Not Answered'
GROUP BY 1
ORDER BY count DESC;
```

### 3. What is the desired work environment?
```sql
SELECT
    work_environment,
    COUNT(*) AS count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS percentage
FROM genz_survey
WHERE work_environment IS NOT NULL
  AND work_environment <> 'Not Answered'
GROUP BY 1
ORDER BY count DESC;
```

### 4. How do financial goals shape career aspirations?
```sql
SELECT
    aspirational_carrer,
    salary_3yr AS expected_salary_3yr,
    salary_5yr AS expected_salary_5yr,
    COUNT(*) AS count
FROM genz_survey
WHERE aspirational_carrer IS NOT NULL
  AND aspirational_carrer <> 'Not Answered'
  AND salary_3yr <> 'Not Answered'
GROUP BY 1, 2, 3
ORDER BY aspirational_carrer, count DESC;
```

### 5. What role do personal values and social impact play?
```sql
SELECT
    social_impact_mission AS impact_importance,
    mission_misaligned AS action_on_misalignment,
    COUNT(*) AS count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS percentage
FROM genz_survey
WHERE social_impact_mission IS NOT NULL
  AND social_impact_mission <> 'Not Answered'
GROUP BY 1, 2
ORDER BY count DESC;
```

## Key Insights

| # | Theme | Finding |
|---|---|---|
| 1 | **Industries** | Gen Z leans to creative and digital roles: Design (503), Operations (480), Data (420). Traditional roles such as manufacturing and sales may need re-branding. |

| 2 | **Influence** | Parents are the top career influence (34.68%, 1,253 respondents), followed by World Changers (22.81%) and Influencers (17.22%). Friends and movies are close to zero. |

| 3 | **Work environment** | About 55% prefer remote-flexible or hybrid (15+ days). Only 21% want a daily office and 5.4% want no office at all. |

| 4 | **Manager style** | 57% (2,058) want a manager who explains and helps. Only 50 want an "unrealistic" manager. Those preferring explain-and-help managers report the fewest abusive-manager experiences (10). |

| 5 | **Learning** | Instructor-led learning is the top choice (911), ahead of observing others (745). Self-portal (189) and external courses (183) rank lowest. |

| 6 | **Salary** | About 30% expect the top salary bracket within 3 years. At the 5-year mark, 22.2% expect more than $151K. |

| 7 | **Mission clarity** | 72.2% (2,610) say their employer's mission is not clearly communicated. Respondents with clarity show a higher share willing to work for social-impact missions. |

| 8 | **Retention** | 57.7% say they would only "conditionally stay" in their current path; 36.6% are firm on staying. |

| 9 | **Study abroad** | 1,632 want to study abroad by self-funding, 793 only if sponsored, and 1,188 do not want to. |

## Recommendations
- **Clarify the mission early:** state it at the offer stage and connect each role to it during onboarding. The gap looks like a communication problem before it is a values problem.
- 
- **Coach, do not command:** train managers to explain their reasoning, and set up regular 1:1 check-ins.
- 
- **Make flexibility the default:** offer remote-flexible and hybrid models with clear rules; keep daily office optional.
- 
- **Show a growth and pay path at hiring:** benchmark pay bands regularly, explain total compensation, and use transparent, merit-based raise criteria.
- 
- **Structure onboarding:** use instructor-led training rather than assuming self-directed learning.
- 
- **Involve families and role models:** run career-awareness sessions for parents and bring role models into campus career talks.

## Repository Structure
```
├── data/
│   ├── Career_Aspirations_Raw_data.xlsx    # raw survey responses (anonymise first)
│   └── Clean_Data.xlsx                      # cleaned dataset
├── excel/
│   ├── Pivot_table_Analysis.xlsx            # 5 pivot analyses
│   └── Dashboard.xlsx                       # Excel executive dashboard
├── powerbi/
│   └── Gen_Z_dashboard.pbix                 # Power BI report
├── sql/
│   └── genz_analysis.sql                    # the 5 queries above
├── reports/
│   ├── Gen_Z_Report.pdf                     # 5W1H problem statement
│   ├── Power_BI_Dashboard.pdf               # dashboard export
│   └── Stakeholder_Presentation.pdf         # final presentation
├── images/                                  # dashboard screenshots
└── README.md
```

## How to Run
1. **Excel:** open `Pivot_table_Analysis.xlsx` and `Dashboard.xlsx` in Excel.
   
3. **SQL Analysis:
4. Load the cleaned data into a table named genz_survey
5. Then run the queries in SQL/ genz_analysis.sql
   
8. **Power BI:** open `Gen_Z_dashboard.pbix` in Power BI Desktop.

## Limitations
- Survey-based and self-reported, so responses show stated preferences, not actual behaviour.
- The sample is 96.5% India-based and 59.65% male, so results speak most directly to India's urban Gen Z workforce.

## Author
Astha Kumari Jha | MBA (Data Science & Analytics), Jain University [www.linkedin.com/in/astha-kumari-jha] | GitHub
**Astha Kumari Jha** | MBA (Data Science & Analytics), Jain University
[LinkedIn URL] | [GitHub](https://github.com/Asthajha4275)
