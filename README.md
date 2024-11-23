# SQL Server Problems
(To become less rusty after alternating between different dialects.)

---

You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N.

| Column | Type |
|--------|------|
| N | Integer |
| P | Integer |

Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:

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
