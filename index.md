---
home: true
---
## null: connect to database

```bash
$ sqlite3 data/penguins.db
```

-   Not actually a query
-   But we have to do it before we can do anything else

## 001: select constant

```sql
select 1;
```
```
1
```

-   `select` is a keyword
-   Normally used to select data from table…
-   …but if all we want is a constant value, we don't need to specify one
-   Semi-colon terminator is required

## 002: select all values from table

```sql
select * from little_penguins;
```
```
Adelie|Torgersen|38.5|17.9|190|3325|FEMALE
Adelie|Biscoe|37.7|18.7|180|3600|MALE
Adelie|Torgersen|37.3|20.5|199|3775|MALE
Adelie|Dream|40.7|17|190|3725|MALE
Chinstrap|Dream|51|18.8|203|4100|MALE
Chinstrap|Dream|51.9|19.5|206|3950|MALE
Adelie|Dream|38.8|20|190|3950|MALE
Gentoo|Biscoe|45.2|15.8|215|5300|MALE
Adelie|Torgersen|35.9|16.6|190|3050|FEMALE
Chinstrap|Dream|45.2|16.6|191|3250|FEMALE
```

-   Use `*` to mean "all columns"
-   Use <code>from <em>tablename</em></code> to specify table
-   Output format is not particularly readable

## null: administrative commands

```sql
.headers on
.mode markdown
select * from little_penguins;
```
```
|  species  |  island   | bill_length_mm | bill_depth_mm | flipper_length_mm | body_mass_g |  sex   |
|-----------|-----------|----------------|---------------|-------------------|-------------|--------|
| Adelie    | Torgersen | 38.5           | 17.9          | 190               | 3325        | FEMALE |
| Adelie    | Biscoe    | 37.7           | 18.7          | 180               | 3600        | MALE   |
| Adelie    | Torgersen | 37.3           | 20.5          | 199               | 3775        | MALE   |
| Adelie    | Dream     | 40.7           | 17            | 190               | 3725        | MALE   |
| Chinstrap | Dream     | 51             | 18.8          | 203               | 4100        | MALE   |
| Chinstrap | Dream     | 51.9           | 19.5          | 206               | 3950        | MALE   |
| Adelie    | Dream     | 38.8           | 20            | 190               | 3950        | MALE   |
| Gentoo    | Biscoe    | 45.2           | 15.8          | 215               | 5300        | MALE   |
| Adelie    | Torgersen | 35.9           | 16.6          | 190               | 3050        | FEMALE |
| Chinstrap | Dream     | 45.2           | 16.6          | 191               | 3250        | FEMALE |
```

-   SQLite administrative commands start with `.` and *aren't* part of the SQL standard
    -   PostgreSQL's special commands start with `\`
-   Use `.help` for a complete list

## 003: specify columns

```sql
select species, island, sex
from little_penguins;
```
```
|  species  |  sex   |  island   |
|-----------|--------|-----------|
| Adelie    | FEMALE | Torgersen |
| Adelie    | MALE   | Biscoe    |
| Adelie    | MALE   | Torgersen |
| Adelie    | MALE   | Dream     |
| Chinstrap | MALE   | Dream     |
| Chinstrap | MALE   | Dream     |
| Adelie    | MALE   | Dream     |
| Gentoo    | MALE   | Biscoe    |
| Adelie    | FEMALE | Torgersen |
| Chinstrap | FEMALE | Dream     |
```

-   Specify column names separated by commas
    -   In any order
    -   Duplicates allowed
-   Line breaks ~allowed~ encouraged for readability

## 004: sort

```sql
select species, sex, island
from little_penguins
order by island asc, sex desc;
```
```
|  species  |  sex   |  island   |
|-----------|--------|-----------|
| Adelie    | MALE   | Biscoe    |
| Gentoo    | MALE   | Biscoe    |
| Adelie    | MALE   | Dream     |
| Chinstrap | MALE   | Dream     |
| Chinstrap | MALE   | Dream     |
| Adelie    | MALE   | Dream     |
| Chinstrap | FEMALE | Dream     |
| Adelie    | MALE   | Torgersen |
| Adelie    | FEMALE | Torgersen |
| Adelie    | FEMALE | Torgersen |
```

-   `order by` must follow `from` (which must follow `select`)
-   `asc` is ascending, `desc` is descending
    -   Default is ascending, but please specify

## 005: limit output

-   Full dataset has 344 rows

```sql
select species, sex, island
from penguins -- full table
limit 5;
```
```
| species |  sex   |  island   |
|---------|--------|-----------|
| Adelie  | MALE   | Torgersen |
| Adelie  | FEMALE | Torgersen |
| Adelie  | FEMALE | Torgersen |
| Adelie  |        | Torgersen |
| Adelie  | FEMALE | Torgersen |
```

-   Comments start with `--` and run to the end of the line
-   <code>limit <em>N</em></code> specifies maximum number of rows returned by query

## 006: page output

```sql
select species, sex, island
from penguins
limit 5 offset 5;
```
```
| species |  sex   |  island   |
|---------|--------|-----------|
| Adelie  | MALE   | Torgersen |
| Adelie  | FEMALE | Torgersen |
| Adelie  | MALE   | Torgersen |
| Adelie  |        | Torgersen |
| Adelie  |        | Torgersen |
```

-   <code>offset <em>N</em></code> must follow `limit`
-   Specifies number of rows to skip

## 007: remove duplicates

```sql
select distinct species, sex, island
from penguins;
```
```
|  species  |  sex   |  island   |
|-----------|--------|-----------|
| Adelie    | MALE   | Torgersen |
| Adelie    | FEMALE | Torgersen |
| Adelie    |        | Torgersen |
| Adelie    | FEMALE | Biscoe    |
| Adelie    | MALE   | Biscoe    |
| Adelie    | FEMALE | Dream     |
| Adelie    | MALE   | Dream     |
| Adelie    |        | Dream     |
| Chinstrap | FEMALE | Dream     |
| Chinstrap | MALE   | Dream     |
| Gentoo    | FEMALE | Biscoe    |
| Gentoo    | MALE   | Biscoe    |
| Gentoo    |        | Biscoe    |
```

-   `distinct` keyword must appear right after `select`
    -   SQL was supposed to read like English…
-   Shows distinct combinations
-   Blanks in `sex` column show missing data
    -   We'll talk about this in a bit

## 008: filter results

```sql
select distinct species, sex, island
from penguins
where island = 'Biscoe';
```
```
| species |  sex   | island |
|---------|--------|--------|
| Adelie  | FEMALE | Biscoe |
| Adelie  | MALE   | Biscoe |
| Gentoo  | FEMALE | Biscoe |
| Gentoo  | MALE   | Biscoe |
| Gentoo  |        | Biscoe |
```

-   <code>where <em>condition</em></code> filters the rows produced by selection
-   Condition is evaluated independently for each row
-   Only rows that pass the test appear in results
-   Use single quotes for `'text data'` and double quotes for `"weird column names"`
    -   SQLite will accept double-quoted text data

## 009: filter with more complex conditions

```sql
select distinct species, sex, island
from penguins
where island = 'Biscoe' and sex != 'MALE';
```
```
| species |  sex   | island |
|---------|--------|--------|
| Adelie  | FEMALE | Biscoe |
| Gentoo  | FEMALE | Biscoe |
```

-   `and`: both sub-conditions must be true
-   `or`: either or both part must be true
-   Notice that the row for Gentoo penguins on Biscoe island with unknown (empty) sex didn't pass the test
    -   We'll talk about this in a bit

## 010: do calculations

```sql
select
    flipper_length_mm / 10.0,
    body_mass_g / 1000.0
from penguins
limit 3;
```
```
| flipper_length_mm / 10.0 | body_mass_g / 1000.0 |
|--------------------------|----------------------|
| 18.1                     | 3.75                 |
| 18.6                     | 3.8                  |
| 19.5                     | 3.25                 |
```

-   Can do the usual kinds of arithmetic on individual values
    -   Calculation done for each row independently
-   Column name shows the calculation done

## 011: rename columns

```sql
select
    flipper_length_mm / 10.0 as flipper_cm,
    body_mass_g / 1000.0 as weight_kg,
    island as where_found
from penguins
limit 3;
```
```
| flipper_cm | weight_kg | where_found |
|------------|-----------|-------------|
| 18.1       | 3.75      | Torgersen   |
| 18.6       | 3.8       | Torgersen   |
| 19.5       | 3.25      | Torgersen   |
```

-   Use <code><em>expression</em> as <em>name</em></code> to rename
-   Give result of calculation a meaningful name
-   Can also rename columns without modifying

## 012: calculate with missing values

```sql
select
    flipper_length_mm / 10.0 as flipper_cm,
    body_mass_g / 1000.0 as weight_kg,
    island as where_found
from penguins
limit 5;
```
```
| flipper_cm | weight_kg | where_found |
|------------|-----------|-------------|
| 18.1       | 3.75      | Torgersen   |
| 18.6       | 3.8       | Torgersen   |
| 19.5       | 3.25      | Torgersen   |
|            |           | Torgersen   |
| 19.3       | 3.45      | Torgersen   |
```

-   SQL uses a special value `null` to representing missing data
    -   Not 0 or empty string, but "I don't know"
-   Flipper length and body weight not known for one of the first five penguins
-   "I don't know" divided by 10 or 1000 is "I don't know"

## 013: null equality

```sql
-- repeated from earlier so it doesn't count against our query limit
select distinct species, sex, island
from penguins
where island = 'Biscoe';
```
```
| species |  sex   | island |
|---------|--------|--------|
| Adelie  | FEMALE | Biscoe |
| Adelie  | MALE   | Biscoe |
| Gentoo  | FEMALE | Biscoe |
| Gentoo  | MALE   | Biscoe |
| Gentoo  |        | Biscoe |
```

-   If we ask for female penguins the row with the missing sex drops out

```sql
select distinct species, sex, island
from penguins
where island = 'Biscoe' and sex == 'FEMALE';
```
```
| species |  sex   | island |
|---------|--------|--------|
| Adelie  | FEMALE | Biscoe |
| Gentoo  | FEMALE | Biscoe |
```

## 014: null inequality

-   But if we ask for penguins that *aren't* female it drops out as well

```sql
select distinct species, sex, island
from penguins
where island = 'Biscoe' and sex != 'FEMALE';
```
```
| species | sex  | island |
|---------|------|--------|
| Adelie  | MALE | Biscoe |
| Gentoo  | MALE | Biscoe |
```

## 015: ternary logic

```sql
select null = null;
```
```
| null = null |
|-------------|
|             |
```

-   If we don't know the left and right values, we don't know if they're equal or not
-   So the result is `null`
-   Get the same answer for `null != null`

-   *three-valued logic*

<table>
  <tr>
    <th colspan="4">equality</th>
  </tr>
  <tr>
    <th></th>
    <th>X</th>
    <th>Y</th>
    <th>null</th>
  </tr>
  <tr>
    <th>X</th>
    <td>true</td>
    <td>false</td>
    <td>null</td>
  </tr>
  <tr>
    <th>Y</th>
    <td>false</td>
    <td>true</td>
    <td>null</td>
  </tr>
  <tr>
    <th>null</th>
    <td>null</td>
    <td>null</td>
    <td>null</td>
  </tr>
</table>

## 016: handle null safely

```sql
select species, sex, island
from penguins
where sex is null;
```
```
| species | sex |  island   |
|---------|-----|-----------|
| Adelie  |     | Torgersen |
| Adelie  |     | Torgersen |
| Adelie  |     | Torgersen |
| Adelie  |     | Torgersen |
| Adelie  |     | Torgersen |
| Adelie  |     | Dream     |
| Gentoo  |     | Biscoe    |
| Gentoo  |     | Biscoe    |
| Gentoo  |     | Biscoe    |
| Gentoo  |     | Biscoe    |
| Gentoo  |     | Biscoe    |
```

-   Use `is null` and `is not null` to handle null safely
-   Other parts of SQL handle nulls specially

## 017: aggregate

```sql
select sum(body_mass_g) as total_mass
from penguins;
```
```
| total_mass |
|------------|
| 1437000    |
```

-   `sum` is an *aggregation function*
-   Combines corresponding values from multiple rows

## 018: common aggregation functions

```sql
select
    max(bill_length_mm) as longest_bill,
    min(flipper_length_mm) as shortest_flipper,
    avg(bill_length_mm) / avg(bill_depth_mm) as weird_ratio
from penguins;
```
```
| longest_bill | shortest_flipper |   weird_ratio    |
|--------------|------------------|------------------|
| 59.6         | 172              | 2.56087082530644 |
```

-   This actually shouldn't work: can't calculate maximum or average if any values are null
-   SQL does the useful thing instead of the right one

## 019: group

```sql
select
    avg(body_mass_g) as average_mass_g
from penguins
group by sex;
```
```
|  average_mass_g  |
|------------------|
| 4005.55555555556 |
| 3862.27272727273 |
| 4545.68452380952 |
```

-   Put rows in buckets based on distinct combinations of values in columns specified with `group by`
-   Then perform aggregation separately for each bucket
-   But, uh, which is which?

## 020: behavior of unaggregated columns

```sql
select
    sex,
    avg(body_mass_g) as average_mass_g
from penguins
group by sex;
```
```
|  sex   |  average_mass_g  |
|--------|------------------|
|        | 4005.55555555556 |
| FEMALE | 3862.27272727273 |
| MALE   | 4545.68452380952 |
```

-   All rows in each group have the same value for `sex`, so no need to aggregate

## 021: arbitrary choice in aggregation

```sql
select
    sex,
    body_mass_g                   
from penguins
group by sex;
```
```
|  sex   | body_mass_g |
|--------|-------------|
|        |             |
| FEMALE | 3800        |
| MALE   | 3750        |
```

-   If we don't specify how to aggregate a column, SQL can choose *any arbitrary value* from the group
-   All penguins in each group have the same sex because we grouped by that, so we get the right answer
-   The body mass values are in the data but unpredictable
-   A common mistake

## 022: filter aggregated values

```sql
select
    sex,
    avg(body_mass_g) as average_mass_g
from penguins
group by sex
having average_mass_g > 4000.0;
```
```
| sex  |  average_mass_g  |
|------|------------------|
|      | 4005.55555555556 |
| MALE | 4545.68452380952 |
```

-   Using <code>having <em>condition</em></code> instead of <code>where <em>condition</em></code> for aggregates

## 023: readable output

```sql
select
    sex,
    round(avg(body_mass_g), 1) as average_mass_g
from penguins
group by sex
having average_mass_g > 4000.0;
```
```
| sex  | average_mass_g |
|------|----------------|
|      | 4005.6         |
| MALE | 4545.7         |
```

-   Use <code>round(<em>value</em>, <em>decimals</em>)</code> to round off a number

# 024: filter aggregate inputs

```sql
select
    sex,
    round(
        avg(body_mass_g) filter (where body_mass_g < 4000.0),
        1
    ) as average_mass_g
from penguins
group by sex;
```
```
|  sex   | average_mass_g |
|--------|----------------|
|        | 3362.5         |
| FEMALE | 3417.3         |
| MALE   | 3752.5         |
```

-   <code>filter (where <em>condition</em>)</code> applies to *inputs*

## null: create in-memory database

```bash
$ sqlite3 :memory:
```

-   "Connect" to an *in-memory* database

## 024: create tables

```sql
create table work(
    person text not null,
    job text not null
);
create table job(
    name text not null,
    billable real not null
);
```

-   <code>create table <em>name</em></code> followed by parenthesized list of columns
-   Each column is a name, a data type, and optional extra information
    -   E.g., `not null` prevents nulls from being added

## 025: insert data

```sql
insert into job values
    ('calibrate', 1.5),
    ('clean', 0.5)
;
insert into work values
    ('mik', 'calibrate'),
    ('mik', 'clean'),
    ('mik', 'complain'),
    ('po', 'clean'),
    ('po', 'complain'),
    ('tay', 'complain')
;
```

-   Resulting tables

|   name    | billable |
|-----------|----------|
| calibrate | 1.5      |
| clean     | 0.5      |

| person |    job    |
|--------|-----------|
| mik    | calibrate |
| mik    | clean     |
| mik    | complain  |
| po     | clean     |
| po     | complain  |
| tay    | complain  |

## 026: update rows

```sql
update work
set person = "tae"
where person = "tay";

select * from work;
```
```
| person |    job    |
|--------|-----------|
| mik    | calibrate |
| mik    | clean     |
| mik    | complain  |
| po     | clean     |
| po     | complain  |
| tae    | complain  |
```

## 027: delete rows

```sql
delete from work
where person = "tae";

select * from work;
```
```
| person |    job    |
|--------|-----------|
| mik    | calibrate |
| mik    | clean     |
| mik    | complain  |
| po     | clean     |
| po     | complain  |
```

## 028: backing up

```sql
create table backup(
    person text not null,
    job text not null
);

insert into backup
select person, job
from work
where person = 'tae';

delete from work
where person = 'tae';

select * from backup;
```
```
| person |   job    |
|--------|----------|
| tae    | complain |
```

## 029: join tables

```sql
select *
from work cross join job;
```
```
| person |    job    |   name    | billable |
|--------|-----------|-----------|----------|
| mik    | calibrate | calibrate | 1.5      |
| mik    | calibrate | clean     | 0.5      |
| mik    | clean     | calibrate | 1.5      |
| mik    | clean     | clean     | 0.5      |
| mik    | complain  | calibrate | 1.5      |
| mik    | complain  | clean     | 0.5      |
| po     | clean     | calibrate | 1.5      |
| po     | clean     | clean     | 0.5      |
| po     | complain  | calibrate | 1.5      |
| po     | complain  | clean     | 0.5      |
| tay    | complain  | calibrate | 1.5      |
| tay    | complain  | clean     | 0.5      |
```

-   `cross join` (also called *outer join*) constructs cross product of tables
    -   All combinations of rows from each
-   Result isn't particularly useful: `job` and `name` don't match

## 030: inner join

```sql
select *
from work inner join job
on work.job = job.name;
```
```
| person |    job    |   name    | billable |
|--------|-----------|-----------|----------|
| mik    | calibrate | calibrate | 1.5      |
| mik    | clean     | clean     | 0.5      |
| po     | clean     | clean     | 0.5      |
```

-   Use <code><em>table</em>.<em>column</em></code> notation to specify columns
    -   A column can have the same name as a table
-   Use <code>on <em>condition</em></code> to specify *join condition*
-   Since `complain` doesn't appear in `job.name`, none of those rows are kept

## 031: aggregate joined data

```sql
select
    work.person,
    sum(job.billable) as pay
from work inner join job
on work.job = job.name
group by work.person;
```
```
| person | pay |
|--------|-----|
| mik    | 2.0 |
| po     | 0.5 |
```

-   Combines ideas we've seen before
-   But Tay is missing from the table

## 032: left join

```sql
select *
from work left join job
on work.job = job.name;
```
```
| person |    job    |   name    | billable |
|--------|-----------|-----------|----------|
| mik    | calibrate | calibrate | 1.5      |
| mik    | clean     | clean     | 0.5      |
| mik    | complain  |           |          |
| po     | clean     | clean     | 0.5      |
| po     | complain  |           |          |
| tay    | complain  |           |          |
```

-   A *left outer join* keeps all rows from the left table
-   Fills missing values from right table with null

## 033: aggregate left joins

```sql
select
    work.person,
    sum(job.billable) as pay
from work left join job
on work.job = job.name
group by work.person;
```
```
| person | pay |
|--------|-----|
| mik    | 2.0 |
| po     | 0.5 |
| tay    |     |
```

-   That's better, but we'd like to see 0 rather than a blank

## 034: coalesce values

```sql
select
    work.person,
    coalesce(sum(job.billable), 0.0) as pay
from work left join job
on work.job = job.name
group by work.person;
```
```
| person | pay |
|--------|-----|
| mik    | 2.0 |
| po     | 0.5 |
| tay    | 0.0 |
```

-   <code>coalesce(<em>val1</em>, <em>val2</em>, …)</code> returns first non-null value

## 035: negate incorrectly

-   Who doesn't calibrate?

```sql
select distinct person
from work
where job != 'calibrate';
```
```
| person |
|--------|
| mik    |
| po     |
| tay    |
```

-   But Mik *does* calibrate
-   Problem is that there's an entry for Mik cleaning
-   And since `'clean' != 'calibrate'`, that row is included in the results
-   We need a different approach

## 036: set membership

```sql
select *
from work
where person not in ('mik', 'tay');
```
```
| person |   job    |
|--------|----------|
| po     | clean    |
| po     | complain |
```

-   <code>in <em>values</em></code> and <code>not in <em>values</em></code> do exactly what you expect

## 037: subqueries

```sql
select distinct person
from work
where person not in (
    select distinct person
    from work
    where job = 'calibrate'
);
```
```
| person |
|--------|
| po     |
| tay    |
```

-   Use a *subquery* to select the people who *do* calibrate
-   Then select all the people who aren't in that set
-   Initially feels odd, but subqueries are useful in other ways

## null: M to N relationships

-   Relationships between entities are usually characterized as:
    -   1-to-1: fields in the same record
    -   1-to-many: the many have a *foreign key* referring to the one's *primary key*
    -   many-to-many: don't know how many keys to add to records ("maximum" never is)
-   Nearly-universal solution is a *join table*
    -   Each record is a pair of foreign keys
    -   I.e., each record is the fact that records A and B are related

## 038: autoincrement and primary key

```sql
create table person(
    ident integer primary key autoincrement,
    name text not null
);
insert into person values
    (null, 'mik'),
    (null, 'po'),
    (null, 'tay')
;
select * from person;
```
```
| ident | name |
|-------|------|
| 1     | mik  |
| 2     | po   |
| 3     | tay  |
```

-   Database automatically increments `ident` each time a new record is added
-   Use that field as the primary key
    -   So that if Mik changes their name again, we only have to change one fact in the database
    -   Downside: manual queries are harder to read (who is person 17?)

## 039: alter tables

```sql
alter table job
add ident integer not null default -1;

update job
set ident = 1
where name = 'calibrate';

update job
set ident = 2
where name = 'clean';

select * from job;
```
```
|   name    | billable | ident |
|-----------|----------|-------|
| calibrate | 1.5      | 1     |
| clean     | 0.5      | 2     |
```

-   Add a column after the fact
-   Since it can't be null, we have to provide a default value
    -   Really want to make it the primary key, but SQLite doesn't allow that (easily) after the fact
-   Then use `update` to modify existing records
    -   Can modify any number of records at once
    -   So be careful about `where` clause
-   *Data migration*

## 040: create new tables from old

```sql
create table new_work(
    person_id integer not null,
    job_id integer not null,
    foreign key(person_id) references person(ident),
    foreign key(job_id) references job(ident)
);

insert into new_work
select
    person.ident as person_id,
    job.ident as job_id
from
    (person join work on person.name = work.person)
    join job on job.name = work.job
;
select * from new_work;
```
```
| person_id | job_id |
|-----------|--------|
| 1         | 1      |
| 1         | 2      |
| 2         | 2      |
```

-   `new_work` is our join table
-   Each column refers to a record in some other table

## 041: remove tables

```sql
drop table work;
alter table new_work rename to work;
```

-   Remove the old table and rename the new one to take its place
-   Be careful…

## null: display schema

```
.schema
```
```
CREATE TABLE job(
    name text not null,
    billable real not null,
    ident integer not null default -1
);
CREATE TABLE person(
    ident integer primary key autoincrement,
    name text not null
);
CREATE TABLE sqlite_sequence(name, seq);
CREATE TABLE IF NOT EXISTS "work"(
    person_id integer not null,
    job_id integer not null,
    foreign key(person_id) references person(ident),
    foreign key(job_id) references job(ident)
);
```

-   Remember, `.schema` is *not* standard SQL
-   SQLite has added a few things
    -   `create if not exists`
    -   upper-case keywords (SQL is case insensitive)
    -   sequence ID table

```sql
select * from sqlite_sequence;
```
```
|  name  | seq |
|--------|-----|
| person | 3   |
```

## 042: compare individual values to aggregates

-   Go back to penguins

```sql
select body_mass_g
from penguins
where body_mass_g > (
   select avg(body_mass_g)
   from penguins
)
limit 5;
```
```
| body_mass_g |
|-------------|
| 4675        |
| 4250        |
| 4400        |
| 4500        |
| 4650        |
```

-   Get average body mass in subquery
-   Compare each row against that
-   Requires two scans of the data, but there's no way to avoid that
-   Null values aren't included in the average or in the final results

## 043: compare individual values to aggregates within groups

```sql
select
    left.species,
    left.body_mass_g,
    round(right.avg_mass_g, 1) as avg_mass_g
from penguins as left join (
    select species, avg(body_mass_g) as avg_mass_g
    from penguins
    group by species
) as right
where left.body_mass_g > right.avg_mass_g
limit 5;
```
```
| species | body_mass_g | avg_mass_g |
|---------|-------------|------------|
| Adelie  | 3750        | 3700.7     |
| Adelie  | 3800        | 3700.7     |
| Adelie  | 4675        | 3700.7     |
| Adelie  | 4250        | 3700.7     |
| Adelie  | 3800        | 3700.7     |
```

## 044: common table expressions

```sql
with grouped as (
    select species, avg(body_mass_g) as avg_mass_g
    from penguins
    group by species
)
select
    penguins.species,
    penguins.body_mass_g,
    round(grouped.avg_mass_g, 1) as avg_mass_g
from penguins join grouped
where penguins.body_mass_g > grouped.avg_mass_g
limit 5;
```
```
| species | body_mass_g | avg_mass_g |
|---------|-------------|------------|
| Adelie  | 3750        | 3700.7     |
| Adelie  | 3800        | 3700.7     |
| Adelie  | 4675        | 3700.7     |
| Adelie  | 4250        | 3700.7     |
| Adelie  | 3800        | 3700.7     |
```

-   Use *common table expression* (CTE) to make queries clearer
    -   Nested subqueries quickly become difficult to understand
-   Database decides how to optimize

## null: explain query

```sql
explain
select
    species,
    avg(body_mass_g)
from penguins
group by species;
```
```
addr  opcode         p1    p2    p3    p4             p5  comment      
----  -------------  ----  ----  ----  -------------  --  -------------
 0    Init            0    43     0                   0   Start at 43
 1    SorterOpen      1     2     0    k(1,B)         0   
 2    Integer         0     2     0                   0   r[2]=0
 3    Null            0     5     5                   0   r[5..5]=NULL
 4    Gosub           4    39     0                   0   
 5    OpenRead        0     2     0    6              0   root=2 iDb=0
 6    Rewind          0    12     0                   0   
 7    Column          0     0    10                   0   r[10]= cursor 0 column 0
 8    Column          0     5    11                   0   r[11]= cursor 0 column 5
 9    MakeRecord     10     2    12                   0   r[12]=mkrec(r[10..11])
10    SorterInsert    1    12     0                   0   key=r[12]
11    Next            0     7     0                   1   
12    OpenPseudo      2    12     2                   0   2 columns in r[12]
13    SorterSort      1    42     0                   0   GROUP BY sort
14    SorterData      1    12     2                   0   r[12]=data
15    Column          2     0     6                   0   r[6]= cursor 2 column 0
16    Compare         5     6     1    k(1,B)         0   r[5] <-> r[6]
17    Jump           18    22    18                   0   
18    Move            6     5     1                   0   r[5]=r[6]
19    Gosub           3    32     0                   0   output one row
20    IfPos           2    42     0                   0   if r[2]>0 then r[2]-=0, goto 42
21    Gosub           4    39     0                   0   reset accumulator
22    Column          2     1    13                   0   r[13]=penguins.body_mass_g
23    AggStep         0    13     9    avg(1)         1   accum=r[9] step(r[13])
24    If              1    26     0                   0   
25    Column          2     0     7                   0   r[7]=penguins.species
26    Integer         1     1     0                   0   r[1]=1
27    SorterNext      1    14     0                   0   
28    Gosub           3    32     0                   0   output final row
29    Goto            0    42     0                   0   
30    Integer         1     2     0                   0   r[2]=1
31    Return          3     0     0                   0   
32    IfPos           1    34     0                   0   if r[1]>0 then r[1]-=0, goto 34
33    Return          3     0     0                   0   
34    AggFinal        9     1     0    avg(1)         0   accum=r[9] N=1
35    Copy            7    14     0                   0   r[14]=r[7]
36    Copy            9    15     0                   0   r[15]=r[9]
37    ResultRow      14     2     0                   0   output=r[14..15]
38    Return          3     0     0                   0   end groupby result generator
39    Null            0     7     9                   0   r[7..9]=NULL
40    Integer         0     1     0                   0   r[1]=0
41    Return          4     0     0                   0   
42    Halt            0     0     0                   0   
43    Transaction     0     0     2    0              1   usesStmtJournal=0
44    Goto            0     1     0                   0
```

## 045: enumerate rows

-   Every table has a special column called `rowid`

```sql
select rowid, *
from penguins
limit 5;
```
```
1|Adelie|Torgersen|39.1|18.7|181|3750|MALE
2|Adelie|Torgersen|39.5|17.4|186|3800|FEMALE
3|Adelie|Torgersen|40.3|18|195|3250|FEMALE
4|Adelie|Torgersen|||||
5|Adelie|Torgersen|36.7|19.3|193|3450|FEMALE
```

-   `rowid` is persistent within a session
    -   I.e., if we delete the first 5 rows we now have row IDs 6…N
-   *Do not rely on row ID*
    -   In particular, do not use it as a key

## 046: if-else function

```sql
with sized_penguins as (
    select
        species,
        iif(
            body_mass_g < 3500,
            'small',
            'large'
        ) as size
    from penguins
)
select species, size, count(*) as num
from sized_penguins
group by species, size
order by species, num;
```
```
|  species  | size  | num |
|-----------|-------|-----|
| Adelie    | small | 54  |
| Adelie    | large | 98  |
| Chinstrap | small | 17  |
| Chinstrap | large | 51  |
| Gentoo    | large | 124 |
```

-   <code>iif(<em>condition</em>, <em>true_result</em>, <em>false_result</em>)</code>
    -   Note: `iif` with two i's

## 047: select a case

-   What if we want small, medium, and large?
-   Can nest `iif`, but quickly becomes unreadable

```sql
with sized_penguins as (
    select
        species,
        case
            when body_mass_g < 3500 then 'small'
            when body_mass_g < 5000 then 'medium'
            else 'large'
        end as size
    from penguins
)
select species, size, count(*) as num
from sized_penguins
group by species, size
order by species, num;
```
```
|  species  |  size  | num |
|-----------|--------|-----|
| Adelie    | large  | 1   |
| Adelie    | small  | 54  |
| Adelie    | medium | 97  |
| Chinstrap | small  | 17  |
| Chinstrap | medium | 51  |
| Gentoo    | medium | 56  |
| Gentoo    | large  | 68  |
```

-   Evaluate `when` options in order and take first
-   Result of `case` is null if no condition is true
-   Use `else` as fallback

## 048: check range

```sql
with sized_penguins as (
    select
        species,
        case
            when body_mass_g between 3500 and 5000 then 'normal'
            else 'abnormal'
        end as size
    from penguins
)
select species, size, count(*) as num
from sized_penguins
group by species, size
order by species, num;
```
```
|  species  |   size   | num |
|-----------|----------|-----|
| Adelie    | abnormal | 55  |
| Adelie    | normal   | 97  |
| Chinstrap | abnormal | 17  |
| Chinstrap | normal   | 51  |
| Gentoo    | abnormal | 62  |
| Gentoo    | normal   | 62  |
```

-   `between` can make queries easier to read
-   But be careful of the `and` in the middle

## null: yet another database

-   *entity-relationship diagram* (ER diagram) shows relationships between tables
-   Like everything to do with databases, there are lots of variations

![assay database table diagram](./img/assays_tables.svg)

![assay ER diagram](./img/assays_er.svg)

```sql
select * from staff;
```
```
| ident | personal |  family  | dept |
|-------|----------|----------|------|
| 1     | Yashvi   | Sankaran |      |
| 2     | Aarav    | Loyal    | mb   |
| 3     | Badal    | Kakar    | hist |
| 4     | Kaira    | Chander  | gen  |
| 5     | Sana     | Hora     | mb   |
| 6     | Riya     | Doctor   | mb   |
| 7     | Nitya    | Kata     | mb   |
| 8     | Bhavin   | Ravel    | mb   |
| 9     | Faiyaz   | Devan    |      |
| 10    | Mahika   | De       |      |
```

## 049: pattern matching

```sql
select personal, family from staff
where personal like '%ya%' or family glob '*De*';
```
```
| personal |  family  |
|----------|----------|
| Yashvi   | Sankaran |
| Riya     | Doctor   |
| Nitya    | Kata     |
| Faiyaz   | Devan    |
| Mahika   | De       |
```

-   `like` is the original SQL pattern matcher
    -   `%` matches zero or more characters at the start or end of a string
    -   Case insensitive by default
-   `glob` supports Unix-style wildcards

| name      | purpose |
| --------- | |
| `substr`  | Get substring given starting point and length |
| `trim`    | Remove characters from beginning and end of string |
| `ltrim`   | Remove characters from beginning of string |
| `rtrim`   | Remove characters from end of string |
| `length`  | Length of string |
| `replace` | Replace occurrences of substring with another string |
| `upper`   | Return upper-case version of string |
| `lower`   | Return lower-case version of string |
| `instr`   | Find location of first occurrence of substring (returns 0 if not found) |

## 050: select first and last rows

```sql
select * from (
    select * from (select * from experiment order by started asc limit 5)
    union all
    select * from (select * from experiment order by started desc limit 5)
)
order by started asc
;
```
```
| ident |    kind     |  started   |   ended    |
|-------|-------------|------------|------------|
| 16    | trial       | 2023-01-29 | 2023-01-30 |
| 34    | calibration | 2023-01-30 | 2023-01-30 |
| 35    | trial       | 2023-02-02 | 2023-02-03 |
| 24    | trial       | 2023-02-12 | 2023-02-14 |
| 5     | trial       | 2023-02-15 | 2023-02-16 |
| 39    | calibration | 2024-01-21 | 2024-01-21 |
| 11    | trial       | 2024-01-26 | 2024-01-28 |
| 43    | trial       | 2024-01-27 | 2024-01-29 |
| 33    | trial       | 2024-02-01 | 2024-02-02 |
| 13    | calibration | 2024-02-03 | 2024-02-03 |
```

-   `union all` combines records
    -   Keeps duplicates: `union` on its own keeps unique records
-   Yes, it feels like the extra `select * from` should be unnecessary
-   `intersect` and `except` perform set intersection and one-sided set difference respectively

## 051: intersection

```sql
select personal, family, dept, age
from staff
where dept == 'mb'
intersect
    select personal, family, dept, age from staff
    where age < 50
;
```
```
| personal |  family   | dept | age |
|----------|-----------|------|-----|
| Indrans  | Sridhar   | mb   | 47  |
| Ishaan   | Ramaswamy | mb   | 35  |
```

-   Tables being intersected must have same structure
-   Intersection usually used when pulling values from different tables
    -   In this case, would be clearer to use `where`

## 052: exclusion

```sql
select personal, family, dept, age
from staff
where dept == 'mb'
except
    select personal, family, dept, age from staff
    where age < 50
;
```
```
| personal | family | dept | age |
|----------|--------|------|-----|
| Pranay   | Khanna | mb   | 51  |
```

-   Again, tables must have same structure
    -   And this would be clearer with `where`
-   SQL operates on sets, not tables, except where it doesn't

## 053: random numbers and why not

```sql
with decorated as (
    select random() as rand,
    personal || ' ' || family as name
    from staff
)
select rand, abs(rand) % 10 as selector, name
from decorated
where selector < 5;
```
```
|         rand         | selector |      name       |
|----------------------|----------|-----------------|
| 1695842608889063964  | 5        | Yashvi Sankaran |
| -2821512941569335882 | 5        | Aarav Loyal     |
| -6460956243954552845 | 3        | Badal Kakar     |
| 5527384638176261870  | 2        | Bhavin Ravel    |
```

-   There is no way to seed SQLite's random number generator
-   Which means there is no way to reproduce one of its "random" sequences

## 054: generate sequence

```sql
select value from generate_series(1, 5);
```
```
| value |
|-------|
| 1     |
| 2     |
| 3     |
| 4     |
| 5     |
```

-   A (non-standard) *table-valued function*

## 055: generate sequence sequence based on data

```sql
create table temp(
    num integer not null
);
insert into temp values (1), (5);
select value from generate_series(
    (select min(num) from temp),
    (select max(num) from temp)
);
```
```
| value |
|-------|
| 1     |
| 2     |
| 3     |
| 4     |
| 5     |
```

-   Must have the parentheses around the `min` and `max` selections to keep SQLite happy

## 056: generate sequence of dates

```sql
select
    date((select julianday(min(started)) from experiment) + value) as some_day
from (
    select value from generate_series(
        (select 0),
        (select count(*) - 1 from experiment)
    )
)
limit 5;
```

-   SQLite represents dates as YYYY-MM-DD strings or as Julian days or as Unix milliseconds or…
    -   Julian days is fractional number of days since November 24, 4714 BCE
-   `julianday` and `date` convert back and forth

## 057: count experiments started per day without gaps

```sql
with
-- complete sequence of days with 0 as placeholder for number of experiments
all_days as (
    select
        date((select julianday(min(started)) from experiment) + value) as some_day,
        0 as zeroes
    from (
        select value from generate_series(
            (select 0),
            (select count(*) - 1 from experiment)
        )
    )
),
-- sequence of actual days with actual number of experiments started
actual_days as (
    select started, count(started) as num_exp
    from experiment
    group by started
)
-- combined by joining on day and taking actual number (if available) or zero
select
    all_days.some_day as day,
    coalesce(actual_days.num_exp, all_days.zeroes) as num_exp
from
    all_days left join actual_days on all_days.some_day = actual_days.started
limit 5
;
```
```
|    day     | num_exp |
|------------|---------|
| 2023-01-29 | 1       |
| 2023-01-30 | 1       |
| 2023-01-31 | 0       |
| 2023-02-01 | 0       |
| 2023-02-02 | 1       |
```

## 058: self join

```sql
with person as (
    select
        ident,
        personal || ' ' || family as name
    from staff
)
select left.name, right.name
from person as left join person as right
limit 10;
```
```
|      name       |      name       |
|-----------------|-----------------|
| Yashvi Sankaran | Yashvi Sankaran |
| Yashvi Sankaran | Aarav Loyal     |
| Yashvi Sankaran | Badal Kakar     |
| Yashvi Sankaran | Kaira Chander   |
| Yashvi Sankaran | Sana Hora       |
| Yashvi Sankaran | Riya Doctor     |
| Yashvi Sankaran | Nitya Kata      |
| Yashvi Sankaran | Bhavin Ravel    |
| Yashvi Sankaran | Faiyaz Devan    |
| Yashvi Sankaran | Mahika De       |
```

-   Join a table to itself
    -   Give copies aliases using `as` to distinguish them
    -   Nothing special about the name `left` and `right`
-   Get all *n<sup>2</sup>* pairs, including person with themself

## 059: generate unique pairs

```sql
with person as (
    select
        ident,
        personal || ' ' || family as name
    from staff
)
select left.name, right.name
from person as left join person as right
on left.ident < right.ident
where left.ident <= 4 and right.ident <= 4;
```
```
|      name       |     name      |
|-----------------|---------------|
| Yashvi Sankaran | Aarav Loyal   |
| Yashvi Sankaran | Badal Kakar   |
| Yashvi Sankaran | Kaira Chander |
| Aarav Loyal     | Badal Kakar   |
| Aarav Loyal     | Kaira Chander |
| Badal Kakar     | Kaira Chander |
```

-   `left.ident < right.ident` ensures distinct pairs without duplicates
-   Use `left.ident <= 4 and right.ident <= 4` to limit output
-   Quick check: n*(n-1)/2 pairs

## 060: filter pairs

```sql
with
person as (
    select
        ident,
        personal || ' ' || family as name
    from staff
),
together as (
    select
        left.staff as left_staff,
        right.staff as right_staff
    from performed as left join performed as right
    on left.experiment = right.experiment
    where left_staff < right_staff
)
select
    left.name as person_1,
    right.name as person_2
from person as left join person as right join together
on left.ident = left_staff and right.ident = right_staff;
```
```
|    person_1     |   person_2   |
|-----------------|--------------|
| Sana Hora       | Nitya Kata   |
| Kaira Chander   | Riya Doctor  |
| Badal Kakar     | Bhavin Ravel |
| Nitya Kata      | Faiyaz Devan |
| Kaira Chander   | Riya Doctor  |
| Yashvi Sankaran | Nitya Kata   |
| Nitya Kata      | Bhavin Ravel |
| Yashvi Sankaran | Aarav Loyal  |
| Aarav Loyal     | Nitya Kata   |
| Kaira Chander   | Faiyaz Devan |
| Badal Kakar     | Bhavin Ravel |
| Yashvi Sankaran | Faiyaz Devan |
| Yashvi Sankaran | Mahika De    |
| Yashvi Sankaran | Nitya Kata   |
| Kaira Chander   | Bhavin Ravel |
| Aarav Loyal     | Faiyaz Devan |
```

## 061: existence and correlated subqueries

```sql
select name, building
from department
where exists (
    select 1
    from staff
    where dept == department.ident
)
order by name;
```
```
|       name        |     building     |
|-------------------|------------------|
| Genetics          | Chesson          |
| Histology         | Fashet Extension |
| Molecular Biology | Chesson          |
```

-   Nobody works in Endocrinology
-   `select 1` could equally be `select true` or any other value
-   A *correlated subquery* depends on a value from the outer query
    -   Equivalent to nested loop

## 062: nonexistence

```sql
select name, building
from department
where not exists (
    select 1
    from staff
    where dept == department.ident
)
order by name;
```
```
|     name      | building |
|---------------|----------|
| Endocrinology | TGVH     |
```

## null: avoiding correlated subqueries

```sql
select distinct
    department.name as name,
    department.building as building
from department join staff
on department.ident = staff.dept
order by name;
```
```
|       name        |     building     |
|-------------------|------------------|
| Genetics          | Chesson          |
| Histology         | Fashet Extension |
| Molecular Biology | Chesson          |
```

-   The join might or might not be faster than the correlated subquery
-   Hard to find unstaffed departments without either `not exists` or `count` and a check for 0

## 063: lead and lag

```sql
with ym_num as (
    select
        strftime('%Y-%m', started) as ym,
        count(*) as num
    from experiment
    group by ym
)
select
    ym,
    lag(num) over (order by ym) as prev_num,
    num,
    lead(num) over (order by ym) as next_num
from ym_num
order by ym;
```
```
|   ym    | prev_num | num | next_num |
|---------|----------|-----|----------|
| 2023-01 |          | 2   | 5        |
| 2023-02 | 2        | 5   | 5        |
| 2023-03 | 5        | 5   | 1        |
| 2023-04 | 5        | 1   | 6        |
| 2023-05 | 1        | 6   | 5        |
| 2023-06 | 6        | 5   | 3        |
| 2023-07 | 5        | 3   | 2        |
| 2023-08 | 3        | 2   | 4        |
| 2023-09 | 2        | 4   | 6        |
| 2023-10 | 4        | 6   | 4        |
| 2023-12 | 6        | 4   | 5        |
| 2024-01 | 4        | 5   | 2        |
| 2024-02 | 5        | 2   |          |
```

-   Use `strftime` to extract year and month
    -   Clumsy, but date/time handling is not SQLite's strong point
-   Use *window functions* `lead` and `lag` to shift values
    -   Unavailable values are null

## 064: window functions

```sql
with ym_num as (
    select
        strftime('%Y-%m', started) as ym,
        count(*) as num
    from experiment
    group by ym
)
select
    ym,
    num,
    sum(num) over (order by ym) as num_done,
    cume_dist() over (order by ym) as progress
from ym_num
order by ym;
```
```
|   ym    | num | num_done |      progress      |
|---------|-----|----------|--------------------|
| 2023-01 | 2   | 2        | 0.0769230769230769 |
| 2023-02 | 5   | 7        | 0.153846153846154  |
| 2023-03 | 5   | 12       | 0.230769230769231  |
| 2023-04 | 1   | 13       | 0.307692307692308  |
| 2023-05 | 6   | 19       | 0.384615384615385  |
| 2023-06 | 5   | 24       | 0.461538461538462  |
| 2023-07 | 3   | 27       | 0.538461538461538  |
| 2023-08 | 2   | 29       | 0.615384615384615  |
| 2023-09 | 4   | 33       | 0.692307692307692  |
| 2023-10 | 6   | 39       | 0.769230769230769  |
| 2023-12 | 4   | 43       | 0.846153846153846  |
| 2024-01 | 5   | 48       | 0.923076923076923  |
| 2024-02 | 2   | 50       | 1.0                |
```

-   `sum() over` does a running total
-   `cume_dist` is fraction *of rows seen so far*

## 065: partitioned windows

```sql
with y_m_num as (
    select
        strftime('%Y', started) as year,
        strftime('%m', started) as month,
        count(*) as num
    from experiment
    group by year, month
)
select
    year,
    month,
    num,
    sum(num) over (partition by year order by month) as num_done
from y_m_num
order by year, month;
```
```
| year | month | num | num_done |
|------|-------|-----|----------|
| 2023 | 01    | 2   | 2        |
| 2023 | 02    | 5   | 7        |
| 2023 | 03    | 5   | 12       |
| 2023 | 04    | 1   | 13       |
| 2023 | 05    | 6   | 19       |
| 2023 | 06    | 5   | 24       |
| 2023 | 07    | 3   | 27       |
| 2023 | 08    | 2   | 29       |
| 2023 | 09    | 4   | 33       |
| 2023 | 10    | 6   | 39       |
| 2023 | 12    | 4   | 43       |
| 2024 | 01    | 5   | 5        |
| 2024 | 02    | 2   | 7        |
```

-   `partition by` creates groups
-   So this counts experiments started since the beginning of each year

## 066: blobs

```sql
create table images (
    name text not null,
    content blob
);

insert into images(name, content) values
    ("biohazard", "img/biohazard.png"),
    ("crush", "img/crush.png"),
    ("fire", "img/fire.png"),
    ("radioactive", "img/radioactive.png"),
    ("tripping", "img/tripping.png")
;

select content from images where name == "fire";
select name, length(content) from images;
```
```
| content |
|---------|
| ?PNG     |
|         |
|         |

|    name     | length(content) |
|-------------|-----------------|
| biohazard   | 19629           |
| crush       | 15967           |
| fire        | 18699           |
| radioactive | 16661           |
| tripping    | 17208           |
```

-   A *blob* is a binary large object
    -   Bytes in, bytes out…
-   If you think that's odd, check out [Fossil][fossil]

## null: yet another database

```bash
$ sqlite3 data/lab_log.db
```
```
.schema
```
```
CREATE TABLE person(
       ident            integer primary key autoincrement,
       details          text not null
);
CREATE TABLE machine(
       ident            integer primary key autoincrement,
       name             text not null,
       details          text not null
);
CREATE TABLE usage(
       ident            integer primary key autoincrement,
       log              text not null
);
```

## 067: store JSON

```sql
select * from machine;
```
```
| ident |      name      |                         details                         |
|-------|----------------|---------------------------------------------------------|
| 1     | WY401          | {"acquired": "2023-05-01"}                              |
| 2     | Inphormex      | {"acquired": "2021-07-15", "refurbished": "2023-10-22"} |
| 3     | AutoPlate 9000 | {"note": "needs software update"}                       |
```

-   Store heterogeneous data as JSON-formatted text (with double-quoted strings)
    -   Database parses it each time it is queried
-   Alternatively store as blob
    -   Can't just view it
    -   But more efficient

## 068: select field from JSON

```sql
select
    details->'$.acquired' as single_arrow,
    details->>'$.acquired' as double_arrow
from machine;
```
```
| single_arrow | double_arrow |
|--------------|--------------|
| "2023-05-01" | 2023-05-01   |
| "2021-07-15" | 2021-07-15   |
|              |              |
```

-   Single arrow `->` returns JSON representation result
-   Double arrow `->>` returns SQL text, integer, real, or null
-   Left side is column
-   Right side is *path expression*
    -   Start with `$` (meaning "root")
    -   Fields separated by `.`

## 069: JSON array access

```sql
select
    ident,
    json_array_length(log->'$') as length,
    log->'$[0]' as first
from usage;
```
```
| ident | length |                            first                             |
|-------|--------|--------------------------------------------------------------|
| 1     | 4      | {"machine":"Inphormex","person":["Xavier","Bouchard"]}       |
| 2     | 5      | {"machine":"Inphormex","person":["B\u00e9atrice","Lachance"] |
|       |        | }                                                            |
| 3     | 2      | {"machine":"sterilizer","person":["Nicolas","Couture"]}      |
| 4     | 1      | {"machine":"sterilizer","person":["Louis","Drouin"]}         |
| 5     | 2      | {"machine":"AutoPlate 9000","person":["Danielle","Nguyen"]}  |
| 6     | 1      | {"machine":"sterilizer","person":["B\u00e9atrice","Lachance" |
|       |        | ]}                                                           |
| 7     | 3      | {"machine":"WY401","person":["Louis","Drouin"]}              |
| 8     | 1      | {"machine":"AutoPlate 9000"}                                 |
```

-   SQLite (and other database managers) has lots of JSON manipulation functions
-   `json_array_length` gives number of elements in selected array
-   subscripts start with 0
-   Characters outside 7-bit ASCII represented as Unicode escapes

## 070: unpack JSON array

```sql
select
    ident,
    json_each.key as key,
    json_each.value as value
from usage, json_each(usage.log)
limit 10;
```
```
| ident | key |                            value                             |
|-------|-----|--------------------------------------------------------------|
| 1     | 0   | {"machine":"Inphormex","person":["Xavier","Bouchard"]}       |
| 1     | 1   | {"machine":"Inphormex","person":["Xavier","Bouchard"]}       |
| 1     | 2   | {"machine":"WY401","person":["Xavier","Bouchard"]}           |
| 1     | 3   | {"machine":"Inphormex","person":["Xavier","Bouchard"]}       |
| 2     | 0   | {"machine":"Inphormex","person":["B\u00e9atrice","Lachance"] |
|       |     | }                                                            |
| 2     | 1   | {"machine":"AutoPlate 9000","person":["B\u00e9atrice","Lacha |
|       |     | nce"]}                                                       |
| 2     | 2   | {"machine":"sterilizer","person":["B\u00e9atrice","Lachance" |
|       |     | ]}                                                           |
| 2     | 3   | {"machine":"AutoPlate 9000","person":["J\u00e9r\u00f4me","Ro |
|       |     | bert"]}                                                      |
| 2     | 4   | {"machine":"sterilizer","person":["B\u00e9atrice","Lachance" |
|       |     | ]}                                                           |
| 3     | 0   | {"machine":"sterilizer","person":["Nicolas","Couture"]}      |
```

-   `json_each` is another table-valued function
-   Use <code>json_each.<em>name</em></code> to get properties of unpacked array

## 071: last element of array

```sql
select
    ident,
    log->'$[#-1].machine' as final
from usage
limit 5;
```
```
| ident |    final     |
|-------|--------------|
| 1     | "Inphormex"  |
| 2     | "sterilizer" |
| 3     | "Inphormex"  |
| 4     | "sterilizer" |
| 5     | "sterilizer" |
```

## 072: modify JSON

```sql
select
    ident,
    name,
    json_set(details, '$.sold', json_quote('2024-01-25')) as updated
from machine;
```
```
| ident |      name      |                           updated                            |
|-------|----------------|--------------------------------------------------------------|
| 1     | WY401          | {"acquired":"2023-05-01","sold":"2024-01-25"}                |
| 2     | Inphormex      | {"acquired":"2021-07-15","refurbished":"2023-10-22","sold":" |
|       |                | 2024-01-25"}                                                 |
| 3     | AutoPlate 9000 | {"note":"needs software update","sold":"2024-01-25"}         |
```

-   Updates the in-memory copy of the JSON, *not* the database record
-   Please use `json_quote` rather than trying to format JSON with string operations

## null: refresh penguins

```sql
select species, count(*) as num
from penguins
group by species;
```
```
|  species  | num |
|-----------|-----|
| Adelie    | 152 |
| Chinstrap | 68  |
| Gentoo    | 124 |
```

-   We will restore full database after each example

## 073: tombstones

```sql
alter table penguins
add active integer not null default 1;

update penguins
set active = iif(species = 'Adelie', 0, 1);

select species, count(*) as num
from penguins
where active
group by species;
```
```
|  species  | num |
|-----------|-----|
| Chinstrap | 68  |
| Gentoo    | 124 |
```

-   Use a *tombstone* to mark (in)active records
-   Every query must now include it

## 074: views

```sql
create view if not exists
active_penguins(
    species,
    island,
    bill_length_mm,
    bill_depth_mm,
    flipper_length_mm,
    body_mass_g,
    sex
) as
select
    species,
    island,
    bill_length_mm,
    bill_depth_mm,
    flipper_length_mm,
    body_mass_g,
    sex
from penguins
where active;

select species, count(*) as num
from active_penguins
group by species;
```
```
|  species  | num |
|-----------|-----|
| Chinstrap | 68  |
| Gentoo    | 124 |
```

-   A *view* is a saved query that other queries can invoke
-   View is re-run each time it's used
-   Like a CTE, but:
    -   Can be shared between queries
    -   Views came first
-   Some databases offer *materialized views*
    -   Update-on-demand temporary tables

## null: hours reminder

```sql
create table job(
    name text not null,
    billable real not null
);
insert into job values
    ('calibrate', 1.5),
    ('clean', 0.5)
;
select * from job;
```
```
|   name    | billable |
|-----------|----------|
| calibrate | 1.5      |
| clean     | 0.5      |
```

## 075: add check

```sql
create table job(
    name text not null,
    billable real not null,
    check(billable > 0.0)
);
insert into job values ('calibrate', 1.5);
insert into job values ('reset', -0.5);
select * from job;
```
```
Runtime error: CHECK constraint failed: billable > 0.0 (19)
|   name    | billable |
|-----------|----------|
| calibrate | 1.5      |
```

-   `check` adds constraint to table
    -   Must produce a Boolean result
    -   Run each time values added or modified
-   But changes made before the error have taken effect

## null: ACID

-   *Atomic*: change cannot be broken down into smaller ones (i.e., all or nothing)
-   *Consistent*: database goes from one consistent state to another
-   *Isolated*: looks like changes happened one after another
-   *Durable*: if change takes place, it's still there after a restart

## 076: transactions

```
create table job(
    name text not null,
    billable real not null,
    check(billable > 0.0)
);

insert into job values ('calibrate', 1.5);

begin transaction;
insert into job values ('clean', 0.5);
rollback;

select * from job;
```
```
|   name    | billable |
|-----------|----------|
| calibrate | 1.5      |
```

-   Statements outside transaction execute and are committed immediately
-   Statement(s) inside transaction don't take effect until:
    -   `end transaction` (success)
    -   `rollback` (undo)
-   Can have any number of statements inside a transaction
-   But *cannot* nest transactions in SQLite
    -   Other databases support this

## 077: rollback in constraint

```sql
create table job(
    name text not null,
    billable real not null,
    check(billable > 0.0) on conflict rollback
);

insert into job values
    ('calibrate', 1.5);
insert into job values
    ('clean', 0.5),
    ('reset', -0.5);

select * from job;
```
```
|   name    | billable |
|-----------|----------|
| calibrate | 1.5      |
```

-   All of second `insert` rolled back as soon as error occurred
-   But first `insert` took effect

## 078: rollback in statement

```sql
create table job(
    name text not null,
    billable real not null,
    check(billable > 0.0)
);

insert or rollback into job values
    ('calibrate', 1.5);
insert or rollback into job values
    ('clean', 0.5),
    ('reset', -0.5);

select * from job;
```
```
Runtime error near line 9: CHECK constraint failed: billable > 0.0 (19)
|   name    | billable |
|-----------|----------|
| calibrate | 1.5      |
```

-   Constraint is in table definition
-   Action is in statement

## null: normalization

-   First normal form (1NF):
    every field of every record contains one indivisible value.

-   Second normal form (2NF) and third normal form (3NF):
    every value in a record that isn't a key depends solely on the key,
    not on other values.

-   *Denormalization*: explicitly store values that could be calculated on the fly
    -   To simplify queries and/or make processing faster

## 079: triggers

-   A *trigger* automatically runs before or after a specified operation
-   Can have side effects (e.g., update some other table)
-   And/or implement checks (e.g., make sure other records exist)
-   Add processing overhead…
-   …but data is either cheap or correct, never both
-   Inside trigger, refer to old and new versions of record
    as <code>old.<em>column</em></code> and <code>new.<em>column</em></code>

```sql
-- Track hours of lab work.
create table job(
    person text not null,
    reported real not null check(reported >= 0.0)
);

-- Explicitly store per-person total rather than using sum().
create table total(
    person text unique not null,
    hours real
);

-- Initialize totals.
insert into total values
    ("gene", 0.0),
    ("august", 0.0)
;

-- Define a trigger.
create trigger total_trigger
before insert on job
begin
    -- Check that the person exists.
    select case
        when not exists (select 1 from total where person = new.person)
        then raise(rollback, 'Unknown person ')
    end;
    -- Update their total hours (or fail if non-negative constraint violated).
    update total
    set hours = hours + new.reported
    where total.person = new.person;
end;

-- Test successful insertion.
insert into job values
    ('gene', 1.5),
    ('august', 0.5),
    ('gene', 1.0)
;

-- Show that both tables have been updated.
select * from job;
select * from total;
```
```
| person | reported |
|--------|----------|
| gene   | 1.5      |
| august | 0.5      |
| gene   | 1.0      |

| person | hours |
|--------|-------|
| gene   | 2.5   |
| august | 0.5   |
```

```sql
-- If any values are negative, operation fails.
insert into job values
    ('gene', 1.0),
    ('august', -1.0)
;
select * from job;
select * from total;
```
```
Runtime error near line 47: CHECK constraint failed: reported >= 0.0 (19)

| person | reported |
|--------|----------|
| gene   | 1.5      |
| august | 0.5      |
| gene   | 1.0      |

| person | hours |
|--------|-------|
| gene   | 2.5   |
| august | 0.5   |
```

```sql
-- If person doesn't exist, operation fails.
insert into job values
    ('mo', 1.0)
;
```
```
Runtime error near line 55: Unknown person  (19)

| person | reported |
|--------|----------|
| gene   | 1.5      |
| august | 0.5      |
| gene   | 1.0      |

| person | hours |
|--------|-------|
| gene   | 2.5   |
| august | 0.5   |
```

## null: represent graphs

```sql
create table lineage(
    parent text not null,
    child text not null
);
insert into lineage values
    ('Arturo', 'Clemente'),
    ('Darío', 'Clemente'),
    ('Clemente', 'Homero'),
    ('Clemente', 'Ivonne'),
    ('Ivonne', 'Lourdes'),
    ('Soledad', 'Lourdes'),
    ('Lourdes', 'Santiago')
;
select * from lineage;
```
```
|  parent  |  child   |
|----------|----------|
| Arturo   | Clemente |
| Darío    | Clemente |
| Clemente | Homero   |
| Clemente | Ivonne   |
| Ivonne   | Lourdes  |
| Soledad  | Lourdes  |
| Lourdes  | Santiago |
```

![lineage diagram](./img/lineage.svg)

## 080: recursive query

```sql
with recursive descendent as (
    select
        'Clemente' as person,
        0 as generations
    union all
    select
        lineage.child as person,
        descendent.generations + 1 as generations
    from descendent join lineage
    on descendent.person = lineage.parent
)
select person, generations from descendent;
```
```
|  person  | generations |
|----------|-------------|
| Clemente | 0           |
| Homero   | 1           |
| Ivonne   | 1           |
| Lourdes  | 2           |
| Santiago | 3           |
```

-   Use a *recursive CTE* to create a temporary table (`descendent`)
-   *Base case* seeds this table
-   *Recursive case* relies on value(s) already in that table and external table(s)
-   `union all` to combine rows
    -   Can use `union` but that has lower performance (must check uniqueness each time)
-   Stops when the recursive case yields an empty row set (nothing new to add)
-   Then select the desired values from the CTE

## null: contact tracing database

```sql
select * from person;
```
```
| ident |         name          |
|-------|-----------------------|
| 1     | Juana Baeza           |
| 2     | Agustín Rodríquez     |
| 3     | Ariadna Caraballo     |
| 4     | Micaela Laboy         |
| 5     | Verónica Altamirano   |
| 6     | Reina Rivero          |
| 7     | Elias Merino          |
| 8     | Minerva Guerrero      |
| 9     | Mauro Balderas        |
| 10    | Pilar Alarcón         |
| 11    | Daniela Menéndez      |
| 12    | Marco Antonio Barrera |
| 13    | Cristal Soliz         |
| 14    | Bernardo Narváez      |
| 15    | Óscar Barrios         |
```

```sql
select * from contact;
```
```
|       left        |         right         |
|-------------------|-----------------------|
| Agustín Rodríquez | Ariadna Caraballo     |
| Agustín Rodríquez | Verónica Altamirano   |
| Juana Baeza       | Verónica Altamirano   |
| Juana Baeza       | Micaela Laboy         |
| Pilar Alarcón     | Reina Rivero          |
| Cristal Soliz     | Marco Antonio Barrera |
| Cristal Soliz     | Daniela Menéndez      |
| Daniela Menéndez  | Marco Antonio Barrera |
```

![contact diagram](./img/contact_tracing.svg)

## 081: bidirectional contacts

```sql
select count(*) as original_count from contact;

create temporary table bi_contact (
    left text,
    right text
);

insert into bi_contact
select
    left, right from contact
    union all
    select right, left from contact
;

select count(*) as num_contact from bi_contact;
```
```
| original_count |
|----------------|
| 8              |

| num_contact |
|-------------|
| 16          |
```

-   Create a *temporary table* rather than using a long chain of CTEs
    -   Only lasts as long as the session (not saved to disk)
-   Duplicate information rather than writing more complicated query

## 082: update group identifiers

```sql
select
    left.name as left_name,
    left.ident as left_ident,
    right.name as right_name,
    right.ident as right_ident,
    min(left.ident, right.ident) as new_ident
from
    (person as left join bi_contact on left.name = bi_contact.left)
    join person as right on bi_contact.right = right.name;
```
```
|       left_name       | left_ident |      right_name       | right_ident | new_ident |
|-----------------------|------------|-----------------------|-------------|-----------|
| Juana Baeza           | 1          | Micaela Laboy         | 4           | 1         |
| Juana Baeza           | 1          | Verónica Altamirano   | 5           | 1         |
| Agustín Rodríquez     | 2          | Ariadna Caraballo     | 3           | 2         |
| Agustín Rodríquez     | 2          | Verónica Altamirano   | 5           | 2         |
| Ariadna Caraballo     | 3          | Agustín Rodríquez     | 2           | 2         |
| Micaela Laboy         | 4          | Juana Baeza           | 1           | 1         |
| Verónica Altamirano   | 5          | Agustín Rodríquez     | 2           | 2         |
| Verónica Altamirano   | 5          | Juana Baeza           | 1           | 1         |
| Reina Rivero          | 6          | Pilar Alarcón         | 10          | 6         |
| Pilar Alarcón         | 10         | Reina Rivero          | 6           | 6         |
| Daniela Menéndez      | 11         | Cristal Soliz         | 13          | 11        |
| Daniela Menéndez      | 11         | Marco Antonio Barrera | 12          | 11        |
| Marco Antonio Barrera | 12         | Cristal Soliz         | 13          | 12        |
| Marco Antonio Barrera | 12         | Daniela Menéndez      | 11          | 11        |
| Cristal Soliz         | 13         | Daniela Menéndez      | 11          | 11        |
| Cristal Soliz         | 13         | Marco Antonio Barrera | 12          | 12        |
```

-   `new_ident` is minimum of own identifier and identifiers one step away
-   Doesn't keep people with no contacts

## 083: recursive labeling

```sql
with recursive labeled as (
    select
        person.name as name,
	person.ident as label
    from
        person
    union -- not 'union all'
    select
        person.name as name,
	labeled.label as label
    from
        (person join bi_contact on person.name = bi_contact.left)
	join labeled on bi_contact.right = labeled.name
    where labeled.label < person.ident
)
select name, min(label) as group
from labeled
group by name
order by label, name;
```
```
|         name          | label |
|-----------------------|-------|
| Agustín Rodríquez     | 1     |
| Ariadna Caraballo     | 1     |
| Juana Baeza           | 1     |
| Micaela Laboy         | 1     |
| Verónica Altamirano   | 1     |
| Pilar Alarcón         | 6     |
| Reina Rivero          | 6     |
| Elias Merino          | 7     |
| Minerva Guerrero      | 8     |
| Mauro Balderas        | 9     |
| Cristal Soliz         | 11    |
| Daniela Menéndez      | 11    |
| Marco Antonio Barrera | 11    |
| Bernardo Narváez      | 14    |
| Óscar Barrios         | 15    |
```

-   Use `union` instead of `union all` to prevent *infinite recursion*

## 084: query from Python

```py
import sqlite3

connection = sqlite3.connect("data/penguins.db")
cursor = connection.execute("select count(*) from penguins;")
rows = cursor.fetchall()
print(rows)
```
```
[(344,)]
```

-   `sqlite3` is part of Python's standard library
-   Create a connection to a database file
-   Get a *cursor* by executing a query
    -   More common to create cursor and use that to run queries
-   Fetch all rows at once as list of tuples

## 085: incremental fetch

```py
connection = sqlite3.connect("data/penguins.db")
cursor = connection.cursor()
cursor = cursor.execute("select species, island from penguins limit 5;")
while (row := cursor.fetchone()):
    print(row)
```
```
('Adelie', 'Torgersen')
('Adelie', 'Torgersen')
('Adelie', 'Torgersen')
('Adelie', 'Torgersen')
('Adelie', 'Torgersen')
```

-   `cursor.fetchone` returns `None` when no more data
-   There is also `fetchmany(N)` to fetch (up to) a certain number of rows

## 086: insert, delete, and all that

```py
connection = sqlite3.connect(":memory:")
cursor = connection.cursor()
cursor.execute("create table example(num integer);")

cursor.execute("insert into example values (10), (20);")
print("after insertion", cursor.execute("select * from example;").fetchall())

cursor.execute("delete from example where num < 15;")
print("after deletion", cursor.execute("select * from example;").fetchall())
```
```
after insertion [(10,), (20,)]
after deletion [(20,)]
```

-   Each `execute` is its own transaction

## 087: interpolate values

```py
connection = sqlite3.connect(":memory:")
cursor = connection.cursor()
cursor.execute("create table example(num integer);")

cursor.executemany("insert into example values (?);", [(10,), (20,)])
print("after insertion", cursor.execute("select * from example;").fetchall())
```
```
after insertion [(10,), (20,)]
```

-   From [XKCD][xkcd-tables]

![XKCD Exploits of a Mom](./img/xkcd_327_exploits_of_a_mom.png)

## 088: script execution

```py
SETUP = """\
drop table if exists example;
create table example(num integer);
insert into example values (10), (20);
"""

connection = sqlite3.connect(":memory:")
cursor = connection.cursor()
cursor.executescript(SETUP)
print("after insertion", cursor.execute("select * from example;").fetchall())
```
```
after insertion [(10,), (20,)]
```

-   But what if something goes wrong?

## 089: SQLite exceptions in Python

```py
SETUP = """\
create table example(num integer check(num > 0));
insert into example values (10);
insert into example values (-1);
insert into example values (20);
"""

connection = sqlite3.connect(":memory:")
cursor = connection.cursor()
try:
    cursor.executescript(SETUP)
except sqlite3.Error as exc:
    print(f"SQLite exception: {exc}")
print("after execution", cursor.execute("select * from example;").fetchall())
```
```
SQLite exception: CHECK constraint failed: num > 0
after execution [(10,)]
```

## 090: Python in SQLite

```py
SETUP = """\
create table example(num integer);
insert into example values (-10), (10), (20), (30);
"""

def clip(value):
    if value < 0: return 0
    if value > 20: return 20
    return value

connection = sqlite3.connect(":memory:")
connection.create_function("clip", 1, clip)
cursor = connection.cursor()
cursor.executescript(SETUP)
for row in cursor.execute("select num, clip(num) from example;").fetchall():
    print(row)
```
```
(-10, 0)
(10, 10)
(20, 20)
(30, 20)
```

-   SQLite calls back into Python to execute the function
-   Other databases can run Python (and other languages) in the database server process
-   Be careful

## 091: handle dates and times

```py
from datetime import date
import sqlite3

# Convert date to ISO-formatted string when writing to database
def _adapt_date_iso(val):
    return val.isoformat()
sqlite3.register_adapter(date, _adapt_date_iso)

# Convert ISO-formatted string to date when reading from database
def _convert_date(val):
    return date.fromisoformat(val.decode())
sqlite3.register_converter("date", _convert_date)

SETUP = """\
create table events(
    happened date not null,
    description text not null
);
"""

connection = sqlite3.connect(":memory:", detect_types=sqlite3.PARSE_DECLTYPES)
cursor = connection.cursor()
cursor.execute(SETUP)

cursor.executemany(
    "insert into events values (?, ?);",
    [
        (date(2024, 1, 10), "started tutorial"),
        (date(2024, 1, 29), "finished tutorial")
    ]
)

for row in cursor.execute("select * from events;").fetchall():
    print(row)
```
```
(datetime.date(2024, 1, 10), 'started tutorial')
(datetime.date(2024, 1, 29), 'finished tutorial')
```

-   `sqlite3.PARSE_DECLTYPES` tells `sqlite3` library to use converts based on declared column types
-   Adapt on the way in, convert on the way out

## *Acknowledgments*

-   [Andi Albrecht][albrecht-andi] for the [`sqlparse`][sqlparse] module
-   [Dimitri Fontaine][fontaine-dimitri] for [*The Art of PostgreSQL*][art-postgresql]
-   Manos Pitsidianakis for online help
-   David Rozenshtein for *The Essence of SQL* (now sadly out of print)

---

## *To Do*

-   upsert
-   TODO: `autocommit` seems to have no effect with Python 3.12.1

[albrecht-andi]: http://andialbrecht.de/
[art-postgresql]: https://theartofpostgresql.com/
[fontaine-dimitri]: https://tapoueh.org/
[fossil]: https://fossil-scm.org/
[sqlparse]: https://pypi.org/project/sqlparse/
[xkcd-tables]: https://xkcd.com/327/
