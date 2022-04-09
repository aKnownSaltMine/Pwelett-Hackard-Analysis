# Pwelett-Hackard-Analysis
## Overview of the Project
The overall goal of this project was to provide a detailed look into the retirement eligible employees over the upcoming years to as a way to prepare for the impending spike in attrition rate due to retirement.   
As a way to help mitigate this and to ramp up replacement employees a mentorship program is also examined to have employees work with people who have held the position to better come to speed on the job processes. 
## Results
#### Table of Retirment Eligible Employees by Job Title
| Title              | Count  |
| :----------------- | :----- |
| Senior Engineer    | 25,916 |
| Senior Staff       | 24,926 |
| Engineer           | 9,285  |
| Staff              | 7,626  |
| Technique Leader   | 3,603  |
| Assistant Engineer | 1,090  |
| Manager            | 2      |

* Out of 240,124 active employees with the company, 72,448 employees are set to retire in the upcoming years leading to a retirement of 30.17% of the total workforce of the company. 

* The retirement eligible employees skew heavily toward senior level staff and technical employees meaning that as they begin to retire with 70% of the retiring employees possessing "Senior" level titles.
* Development and Production departments stand to lose the most employees accounting for 47% of the retiring employees.
* Out of the retiring employees only 1,549 of them are mentorship eligible.

## Summary
As this "silver tsunami" begins, the Pewlett Hackard will need to plan on replacing a large portion of its total senior leadership and a total of 72,448 roles over the next 5-10 years. However, there does not seem to be enough mentorship eligible employees who with only 1549 qualifying under the current criteria. However, if we expand the criteria to a bithdate falling from 1960-1965 as in the following query:
```
SELECT DISTINCT ON(e.emp_no) e.emp_no,
	e.first_name,
	e.last_name,
	e.birth_date,
	de.from_date,
	de.to_date,
	t.title
FROM employees AS e
INNER JOIN dept_emp AS de
ON (e.emp_no = de.emp_no)
INNER JOIN titles as t
ON (e.emp_no = t.emp_no)
WHERE (de.to_date = '9999-01-01') 
AND (e.birth_date BETWEEN '1960-01-01' AND '1965-12-31')
ORDER BY (e.emp_no);
```
Then the total amount of eligible employees goes from just over 1500 to almost 94,000.
However, selecting the mentor eligible employees would make more sense based on experience rather than birth year as laid out before. The following query shows that making selecting the employees that would be eligible for mentorship based on 25 years of experience in the role that would need replacing:
```
-- creating table of mentor eligible employees
SELECT DISTINCT ON(e.emp_no) e.emp_no,
	e.first_name,
	e.last_name,
	e.birth_date,
	de.from_date,
	de.to_date,
	t.title,
	DATE_PART('year', AGE(t.from_date)) as experience
FROM employees AS e
INNER JOIN dept_emp AS de
ON (e.emp_no = de.emp_no)
INNER JOIN titles as t
ON (e.emp_no = t.emp_no)
WHERE (t.to_date = '9999-01-01') 
AND (DATE_PART('year', AGE(t.from_date)) > 25)
ORDER BY (e.emp_no), experience;
```

This returns an eligibility of over 103,640 eligible employees which would allow  for a further narrowing of the eligibility in order to have a 1:1 retiring mentor role to new hire ratio.