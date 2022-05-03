# Note.MySQL
## Data type
```SQL
bit             ; 0, 1, null
int             ; 32 bits integer
decimal         ; 38 bits number
float           ; -1.79E+308 ~ +1.79E+308
datetime        ; date and time
money           ; -922337203685477.5808 ~ +922337203685477.5807
char(len)       ; fixed  string , len < 8000
varchar(len)    ; change string , len < 8000
nvarchar(len)   ; change string , len < 4000 , encoding unicode
varchar(max)    ; change string , max number of character is 2G
text            ; change string , max number of character is 2G
varbinary(len)  ; store binary data , max number of byte is 2G
image           ; store binary data , max number of byte is 2G
xml             ; store XML
```
## Define
### Create 
#### Database
```SQL
create database ${database}
```
#### Table (Column)
```SQL
create table ${table}
  (
    ${column} ${column.type} [${column.value:${column.null:"not null","null"}, ${column.default:"default ${column.defaultValue}"}}], 
    ... , 
    primary key ${column} [, ${column} ... ] , 
    foreign key (${column}) references ${table} (${column}) , 
    ... , 
    check (${condition}) , 
    ... , 
  )
```
Example:
```SQL
create table mark
  (
    ID         char(16)    not null       , 
    Illustrate text        default "null" , 
    Link       varchar(16)                , 
    primary key ID,
    foreign key (Link) references content (line) , 
  )
```
```SQL
create table mark
  (
    [ID]         char(16)    not null       , 
    [Illustrate] text        default "null" , 
    [Link]       varchar(16)                , 
    primary key [ID],
    foreign key ([Link]) references [content] ([line]) , 
  )
```
#### Index
```SQL
create [unique] index ${index.name} on ${table} (${column}) [${order:"asc","desc"}]
```
Example:
```SQL
create unique index index_ on mark (ID) asc
```
### Alter 
#### Add 
```SQL
alter table ${table} 
add [column ${column} ${column.type}], 
    [primary key ${column}], 
    [
      constraint ${constraint.name)
      [foreign key (${column}) references ${table} (${column})], 
      [default ${column.defaultValue} for ${column}], 
      [check (${condition})]
    ]
```
### Drop 
```SQL
drop [table ${table}], 
     [index ${table}.${index.name}], 
     [column ${column}], 
     [primary key], 
     [constraint ${constraint.name)]
```
### Truncate 
```SQL
truncate table ${table}
```
## operate (Row)
### Insert
```SQL
insert [into] ${table} [(${column} [, ${column} ... ] )]
value (${column.value} [, ${column.value} ... ] )
```
### Update
```SQL
update ${table}
set ${column} = ${column.value} [, ${column} = ${column.value} ... ]
[from ${table} [ ... ]]
[where ${condition}]
```
### Delete
```SQL
delete ${table}
[from ${table} [ ... ]]
[where ${condition}]
```
## Control (Permission)
### Grant (Authorize)
### Revork

## Select (Search) 
```SQL
select [${table}.]${column} [[as] ${alias}] [, [${table}.]${column} [[as] ${alias}] ... ]
from ${table} [ ... ]
[where ${condition}]
[group by [${table}.]${column} [, [${table}.]${column} ... ]]
[having ${condition}]
[order by [${table}.]${column} [, [${table}.]${column} ... ]]
```
### Condition
#### Comparison Operators
| Operator | Description  |   | Operator | Description           |
| :------: | :----------- | - | :------: | :-------------------- |
|   `=`    | equal to     |   |   `>`    | more than             |
|   `<>`   | not equal to |   |   `>=`   | more than or equal to |
|   `!=`   | not equal to |   |   `!>`   | no more than          |
|          |              |   |   `<`    | less than             |
|          |              |   |   `<=`   | less than or equal to |
|          |              |   |   `!<`   | no less than          |
#### Logical Operators
| Operator        | Format                                    | Description  | 
| :-------------: | :---------------------------------------: | :----------- | 
| `like`          | `${column} like ${format}`                |              | 
| `between...and` | `${column} between ${min} and ${max}`     |              | 
| `in`            | `${column} in (${value}[, ${value} ...])` |              | 
| `is null`       | `${column} is null`                       |              | 
| `exists`        | `exists (${subQuery})`                    |              | 
| `not`           | `not ${condition}`                        |              | 
| `and`           | `${condition} and ${condition}`           |              | 
| `or`            | `${condition} or ${condition}`            |              | 
#### Arithmetic Operators
| Operator | Description  | 
| :------: | :----------- | 
| `+`      |              | 
| `-`      |              | 
| `*`      |              | 
| `/`      |              | 
#### Aggregate Operators
| Operator              | Description  | 
| :-------------------: | :----------- | 
| `count(${column})`    |              | 
| `avg(${column})`      |              | 
| `sum(${column})`      |              | 
| `max(${column})`      |              | 
| `min(${column})`      |              | 
### Combine queries
| Type          | Description                                   | Format                                                                            |
| ------------- | --------------------------------------------- | --------------------------------------------------------------------------------- |
| cross   join  | Outer product merger                          | `from ${table1} cross join ${table2}`                                             |
| theta   join  | Use compare to merge                          | `where ${column} ${ComparisonOperators} ${column}`                                |
| equi    join  | Use equality to combine                       | `where ${column} = ${column}`                                                     |
| natural join  | Merge duplicate column data                   | `from ${table1}, ${table2} where ${table1}.${column} = ${table2}${column}`        |
| inner   join  | Merge duplicate column data                   | `from ${table1} inner join ${table2} on ${table1}.${column} = ${table2}${column}` |
| outer   join  | Consolidation contains incomplete information | `from ${table1}, ${table2}`                                                       |
| left    join  | Consolidation contains incomplete information | `from ${table1} left  join ${table2} on ${table1}.${column} = ${table2}${column}` |
| right   join  | Consolidation contains incomplete information | `from ${table1} right join ${table2} on ${table1}.${column} = ${table2}${column}` |
| full    join  | Consolidation contains incomplete information | `from ${table1} full  join ${table2} on ${table1}.${column} = ${table2}${column}` |
### Set operations
| Type      | Description | Format                                                                      |
| --------- | ----------- | --------------------------------------------------------------------------- |
| union     | or merge    | `select ${column} from ${table1} union     select ${column} from ${table2}` |
| intersect | and merge   | `select ${column} from ${table1} intersect select ${column} from ${table2}` |
| except    | remove      | `select ${column} from ${table1} except    select ${column} from ${table2}` |
### Nested Query (SubQuery)
+ get a single field
+ use in `( )` for transform into a list
+ can't use `order by` in SubQuery
+ can't use `between...and` in MainQuery
