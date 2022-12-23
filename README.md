# Employee Database Challenge Pewlett-Hackard Analysis

## Resources
Data - https://github.com/dh4rt/Pewlett-Hackard-Analysis/tree/main/Data
pgAdmin v 6.15


## Analysis
This analysis was commishened by Pewlett-Hackard, to help establish a forecast for the upcoming retirements through the totality of the company and how to best 
leverage talent going forward to ensure that the productivity maintains its rate. This analysis was performed by myself and Bobby at the request of his manager.
Our instructed directions where to create two distinct outputs: The first would have the number of employees retiring by title, and the second output was to create a table of the employees based on the age based mentorship program. How these two fit together will be explained later.


### Deliverable 1
The first deliverable requested was a table that would show the **Number of Retiring Employees**, specifically the number retiring by job title. This table took
multiple steps to create. The first of which was to determine what tables and columns would be needed to establish the necessary connections between distinct
tables, that code is as follows:

```
SELECT e.emp_no,
    e.first_name,
e.last_name,
    ti.title,
    ti.from_date,
    ti.to_date
INTO retirement_titles
FROM employees as e
```

The `SELECT` function and the following lines tell the code where to pull columns from, with the `employees` table being shortend to e to allow the code know which
tables specifically to pull individual columns from.  This code then insturcts where to place the output and the name of the first of two tables needed for 
this information. The second set of code needed was as follows:

```
INNER JOIN titles as ti
ON (e.emp_no = ti.emp_no)
```


The second table nickname is declared with the `INNER JOIN` function which also tells the code to do is match and merge the two tables on the variale  `emp_no` which
is a distinct indentifier given to every employee. The next section of code (below) repeats the process, but with a different table this time drawing from the 
 `dept_emp` table which tells us how long individual employees have held their roles, with the employees being organized in ascending order based on their `emp_no`.

```
INNER JOIN dept_emp as de
ON (e.emp_no = de.emp_no)
WHERE (birth_date BETWEEN '1952-01-01' AND '1955-12-31')
order by e.emp_no;
```

The next step to getting the instructed outcome involved creating another table. The first second of this code is as follows:

```
SELECT DISTINCT ON (emp_no) emp_no,
first_name,
last_name,
title

INTO unique_titles
FROM retirement_titles
WHERE (to_date = '9999-01-01')
ORDER BY emp_no, to_date DESC;
```
The goal of this section of code is to give us an output that shows the current title for folks who are qualified for retirement but have been yet to do so. This
table was again organized in order by the employees `emp_no`. The last section of code for **Deliverable 1** was as follows:

```
SELECT COUNT(title),title
INTO retiring_titles
FROM unique_titles 
GROUP BY title
order by COUNT(title) DESC;
```
This code's instructions tell the program to pull and count individual titles from the  `unique_titles` and to create a table within the newly constructed
`retiring_titles` table. The out put of that was generated is attached

```
25916	"Senior Engineer"
24926	"Senior Staff"
9285	"Engineer"
7636	"Staff"
3603	"Technique Leader"
1090	"Assistant Engineer"
2	"Manager"
```
### Deliverable 2
The second requested deliverable was create a table of **The Employees Eligible for the Mentorship Program** the outcomes of this table tell us which employees
should considered for which potentially opening positions. The first section of this code is as follows:

```
SELECT  e.emp_no,
		e.first_name,
		e.last_name,
		e.birth_date,
		de.from_date,
		de.to_date,
		ti.title
INTO mentorship_eligibilty
FROM employees as e
INNER JOIN dept_emp as de 
```
This section illistrates what columns are being pulled from which tables, and again included the nicknames given to specific tables so that the program knows where
to pull from.  The code then is told where to send the output (`mentorship_eligibility`). The next section of code follows a familar pattern to what we've seen prior.
That code is attached below:

```
ON (e.emp_no = de.emp_no)
INNER JOIN titles as ti
ON (e.emp_no = ti.emp_no)
WHERE (de.to_date = '9999-01-01')
AND (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
ORDER BY e.emp_no;
```
This section of code tells the program what column within respective tables to join on, and you'll see a familiar section of code. One that instructs to join on 
the `emp_no` column. The next section tells that we should considering employees who have not formally retired, thus thier `to_date` is in the future. Following that
instruction to the age range for folks eligible for the program are filtered as the content of the table and again orded by their `emp_no`.

The output for this code can be found here: https://github.com/dh4rt/Pewlett-Hackard-Analysis/blob/main/Data/mentorship_eligibilty.csv
A link was included instead of a table because this out put is many times larger.

## Results
The overall results from this analysis show that their are several problems facing the company in the near future. But there is plenty of reason to be optimistic
about the future at Pewlett-Hackard. Each deliverable has four major take-aways

### Deliverable 1
+ The number of Senior Staff and Eningeers puts the company at tremendous risk for brain drain if there is even a moderate increase in the number of persons with
those roles retiring.
+ The second takeaway from this deliverable is that Manager seems to be the position with most stablility going forward.
+ The process to become a Senior Engineer or Staff member is constructed in such a way as to make it seemingly too easy, with so many senior employees then earning
a salary that is comprable.
+ Finally the last take away is that there is also seemingly a good deal of stability with the Technique Leaders.

### Deliverable 2
+ There is a steady pipeline of senior level quality employees on the payroll.
+ The pathways from entry level to senior level quality is clear and obvious, aiding in the recruitment of new talent.
+ Employees are able to move up the chain quickly.
+ There are not enough of these employees to make up for the numbers being lost.

## Summary
To put it bluntly the company is facing a crisis, with more than 75,000 employees qualifiying for retirement and having less than 50,000 in the pipeline to replace
them serious consideration to future must be given. My suggestions to seek answers invole two tables:
salaries
dept_manager

The goals here should be to see if either the salaries are driving folks away or if specific managers are.

