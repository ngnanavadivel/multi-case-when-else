# multi-case-when-else

```sql

create table t_file(
  pw text,
  _desc text,
  month integer,
  src integer,
  usd_value integer);
  
  insert into t_file values('PW1', 'DESC1', 1, 1, 10);
  insert into t_file values('PW1', 'DESC1', 1, 1, 15);
  
  insert into t_file values('PW1', 'DESC1', 1, 2, 20);
  insert into t_file values('PW1', 'DESC1', 1, 2, 25);
  
  insert into t_file values('PW1', 'DESC1', 1, 3, 30);
  insert into t_file values('PW1', 'DESC1', 1, 3, 35);
  
  insert into t_file values('PW1', 'DESC1', 1, 4, 40);
  insert into t_file values('PW1', 'DESC1', 1, 4, 45);
  
  insert into t_file values('PW1', 'DESC1', 1, 5, 50);
  insert into t_file values('PW1', 'DESC1', 1, 5, 55);


-- ################# Query #######################

select 

pw as payment_week,
_desc as pw_desc,
month as month,
src as src,

case when src = 1 then sum(usd_value) else 0 
end as PRODUCTION,
  
case  when src = 4 then sum(usd_value) else 0 
end as NON_PRODUCTION,

case when src = 2 then sum(usd_value) else 0 
end as E_AND_S,

case when src = 3 then sum(usd_value) else 0 
end as PROJECTS,

case when src = 5 then sum(usd_value) else 0 
end as FREIGHT
from
t_file
group by
pw,
_desc,
month,
src;

```

| payment_week | pw_desc | month | src | PRODUCTION | NON_PRODUCTION | E_AND_S | PROJECTS | FREIGHT |
|--------------|---------|-------|-----|------------|----------------|---------|----------|---------|
|          PW1 |   DESC1 |     1 |   1 |         25 |              0 |       0 |        0 |       0 |
|          PW1 |   DESC1 |     1 |   2 |          0 |              0 |      45 |        0 |       0 |
|          PW1 |   DESC1 |     1 |   3 |          0 |              0 |       0 |       65 |       0 |
|          PW1 |   DESC1 |     1 |   4 |          0 |             85 |       0 |        0 |       0 |
|          PW1 |   DESC1 |     1 |   5 |          0 |              0 |       0 |        0 |     105 |


```sql
select
pw as payment_week,
_desc as pw_desc,
month as month,
sum(PRODUCTION) as PROD,
sum(NON_PRODUCTION) as NON_PROD,
sum(E_AND_S) as EAS,
sum(PROJECTS) as PROJ,
sum(FREIGHT) as FREIG,
sum(PRODUCTION + NON_PRODUCTION + E_AND_S + PROJECTS + FREIGHT)
from
(select
pw as payment_week,
_desc as pw_desc,
month as month,
src as src,
case when src = 1 then sum(usd_value) else 0 
end as PRODUCTION,  
case  when src = 4 then sum(usd_value) else 0 
end as NON_PRODUCTION,
case when src = 2 then sum(usd_value) else 0 
end as E_AND_S,
case when src = 3 then sum(usd_value) else 0 
end as PROJECTS,
case when src = 5 then sum(usd_value) else 0 
end as FREIGHT
from
t_file
group by
pw,
_desc,
month,
src) as t_intermediate
```
