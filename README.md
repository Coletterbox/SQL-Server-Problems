# SQL Server Problems
## (My solutions to problems from HackerRank.)
(To become less rusty after alternating between different dialects.)

---

### You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N.

| Column | Type |
|--------|------|
| N | Integer |
| P | Integer |

### Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:

+ Root: If node is root node.
+ Leaf: If node is leaf node.
+ Inner: If node is neither root nor leaf node.

```sql
with c as (
    select * from bst
),
gp as (
select c.n as n0, bst.n as m, bst.p as n2
from bst
left join c on bst.n = c.p
)
select distinct m,
    case when (n0 is not null and n2 is not null) then "Inner"
    when (n0 is null and n2 is not null) then "Leaf"
    when (n0 is not null and n2 is null) then "Root"
    else null end as rli
from gp
```

---

### A conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy:

Founder\
    &darr;\
Lead Manager\
    &darr;\
Senior Manager\
    &darr;\
Employee

### Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, total number of managers, and total number of employees. Order your output by ascending company_code.

##### Note: The tables may contain duplicate records.

The company_code is a string, so the sorting should not be numeric. For example, if the company_codes are C_1, C_2, and C_10, then the ascending company_codes will be C_1, C_10, and C_2.

#### Input Format

The following tables contain company data:

+ Company: The company_code is the code of the company and founder is the founder of the company. 
+ Lead_Manager: The lead_manager_code is the code of the lead manager, and the company_code is the code of the working company. 
+ Senior_Manager: The senior_manager_code is the code of the senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company. 
+ Manager: The manager_code is the code of the manager, the senior_manager_code is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company. 
+ Employee: The employee_code is the code of the employee, the manager_code is the code of its manager, the senior_manager_code is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company. 

```sql
select c.company_code,
    c.founder,
    count(distinct lm.lead_manager_code),
    count(distinct sm.senior_manager_code),
    count(distinct m.manager_code),
    count(distinct e.employee_code)
from company c
full join lead_manager lm on lm.company_code = c.company_code
full join senior_manager sm on sm.company_code = c.company_code
full join manager m on m.company_code = c.company_code
full join employee e on e.company_code = c.company_code
group by c.company_code, c.founder
order by c.company_code
```

---

### Given the CITY and COUNTRY tables, query the sum of the populations of all cities where the CONTINENT is 'Asia'.

##### Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

```sql
with asia as (
    select code, continent
    from country
    where lower(continent) = "asia"
)
select sum(city.population)
from city
inner join asia on countrycode = asia.code
group by continent
```

---

### You are given two tables: Students and Grades.

### Students contains three columns ID, Name and Marks.

| Column | Type |
|--------|------|
| ID | Integer |
| Name | String |
| Marks | Integer |

### Grades contains the following data:

| Grade | Min_Mark | Max_Mark |
|-------|----------|----------|
| 1 | 0 | 9 |
| 2 | 10 | 19 |
| 3 | 20 | 29 |
etc.

Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

Write a query to help Eve.

```sql
select case when grade < 8 then null
    else name end,
    grade,
    marks
from students
left join grades on marks >= min_mark and marks <= max_mark
order by grade desc, name, marks
```
