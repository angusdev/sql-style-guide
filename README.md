# Angus's SQL Style Guide

## Use UPPERCASE for datbase objects name

Use UPPERCASE for datbase objects name (table, column scheme etc.) and lowercase for reserved words, table alias, variables etc.

```sql
-- Good
select c.CUST_ID,
       c.CUST_NAME,
       a.COUNTRY,
       a.ADDRESS
  from PROD.CUSTOMER c
 inner join PROD.ADDR a
         on c.CUST_ID = a.CUST_ID
 where c.CUST_ID = p_cust_id;

-- Okay
select c.cust_id,
       c.cust_name,
       a.country,
       a.address
  from prod.customer c
 inner join prod.addr a
         on c.cust_id = a.cust_id
 where c.cust_id = p_cust_id;

-- Bad
SELECT c.cust_id,
       c.cust_name,
       a.country,
       a.address
  FROM prod.customer c
 INNER JOIN prod.addr a
         ON c.cust_id = a.cust_id
 WHERE c.cust_id = p_cust_id;
```

### Why don't use upper case for reserved words
Traditionally using uppercase for reserved words is a de-facto standard.  Since SQL has many reserved words comparing to modern computer languages, it is more to distinguish them from normal word with uppercase.  As mentioned in [Joe Celko's SQL Programming Style](https://learning.oreilly.com/library/view/joe-celkos-sql/9780120887972/) (ISBN: 9780120887972, 2005):

> 2.1.4 Uppercase the Reserved Words
>
> Rationale:
>
> Uppercase words are seen as a unit, rather than being read as a series of syllables or letters. The eye is drawn to them, and they act to announce a statement or clause. That is why headlines and warning signs work.
>
> Typographers use the term bouma for the shape of a word. The term appears in Paul Saenger’s book (1975). Imagine each letter on a rectangular card that just fits it, so you see the ascenders, descenders, and baseline letters as various-sized “Lego blocks” that are snapped together to make a word.

It is true without syntax highlighting or in printed book. For example
```plaintext
SELECT * FROM customer WHERE customer_id = 1

-- is more readable then

select * from customer where customer_id = 1
```

However with syntax highlighting available in almost all editors, IDEs and viewers, there is no longer need to highlight the reserved words with case.

### Why use uppercase for database object name?

All lowercase is acceptable.  But we used to see table name and column name in upper case. For example in design specifications, `desc table_name`, `select table_name from all_tables`, Toad Schema Browser. It is more easy for us to recognize the name in upper case. In addition, we can easiliy copy and paste the table name or column name every where.

## Indentation

* Right align the select-from-where-group-order.
* Table and column name always aligned at 7th position (0 based) as `select`, `update`, `delete` all has 6 characters.
* `and` and `or` put at the end of line.
* New line for each column in `select`, `where`, `group by`
* `order by` is ok in one line

```sql
-- Good
select c.CUST_TYPE,
       a.COUNTRY,
       count(*) as CNT       
  from CUSTOMER c
 inner join ADDR a on
       c.BUSINESS_DATE = a.BUSINESS_DATE and
       c.CUST_ID = a.CUST_ID
 where c.STATUS = 'ACTIVE' and
       c.BUSINESS_DATE = p_business_date
 group by
       c.CUST_TYPE,
       a.COUNTRY
 order by CUST_TYPE, COUNTRY desc;

-- Bad
select c.CUST_TYPE,
       a.COUNTRY,
       count(*) as CNT       
  from CUSTOMER c
 inner join ADDR a
    on c.BUSINESS_DATE = a.BUSINESS_DATE
   and c.CUST_ID = a.CUST_ID
 where c.STATUS = 'ACTIVE'
   and c.BUSINESS_DATE = p_business_date
 group by c.CUST_TYPE,
          a.COUNTRY
 order by CUST_TYPE, COUNTRY desc;

-- Bad
select
    c.CUST_TYPE,
    a.COUNTRY,
    ount(*) as CNT       
from CUSTOMER c
inner join ADDR a on
    c.BUSINESS_DATE = a.BUSINESS_DATE and
    c.CUST_ID = a.CUST_ID
where
    c.STATUS = 'ACTIVE' and
    c.BUSINESS_DATE = p_business_date
group by
    c.CUST_TYPE,
    a.COUNTRY
order by
    CUST_TYPE, COUNTRY desc;

-- Bad
select c.CUST_TYPE, a.COUNTRY, count(*) as CNT       
from CUSTOMER c
inner join ADDR a on c.BUSINESS_DATE = a.BUSINESS_DATE
    and c.CUST_ID = a.CUST_ID
where c.STATUS = 'ACTIVE'
    and c.BUSINESS_DATE = p_business_date
group by c.CUST_TYPE, a.COUNTRY
order by CUST_TYPE, COUNTRY desc;
```
 
## Column name conventions
* Date-only fields should be suffixed with _DATE. E.g. `BUSINESS_DATE`, `TRADE_DATE`.
* Date+time fields should be suffixed with _TIME. E.g. `LOGIN_TIME`, `ORDER_TIME`, except:
* For audit columns, use `CREATED_AT`, `CREATED_BY`, `LAST_UPDATED_AT`, `LAST_UPDATED_BY`. Don't use CREATE_USER, LAST_MODIFIED_BY, CREATED_DATE, LAST_UPDATED_DATE.
