---
layout: post
title: "SQL 정리"
categories:
  - etc computer
tags:
  - SQL
  - DB
last_modified_at: 2023-03-10
comments: true
---
본 포스팅에서는 SQL에 대해서 정리한다.  

|DDL|DML|DCL|
 --- || --- | --- |
|CAD|SIDU|CRRG|

### 1. DDL $($Data definition language$)$
realtion의 스키마를 정의 하거나 바꾸는 방법  
Create 5개  
Alter 1개  
Drop 1개  

#### 1.1 CREATE SCHEMA
```
CREATE TABLE student AUTHORIZATION id_1354_uname
```
#### 1.2 CREATE DOMAIN
```
CREATE DOMAIN sex [AS] VARCHAR(1)
DEFAULT '남'
CONSTRAINT valid_sex CHECK(VALUE IN ('남','여'));
```
#### 1.3 CREATE TABLE
```
CREATE TABLE student(
  s_id VARCHAR(3) NOT NULL,
  major VARCHAR(10),
  PW VARCHAR(5),
  resion VARCHAR(20),
  sex_val sex,
  PRIMARY(s_id),
  FOREIGN KEY
    REFERENCES universe(major_id),
  CHECK(성별 = '남');
)
```
#### 1.4 CREATE VIEW
```
CREATE VIEW seoul_persion(name, s_id) 
AS SELECT name, s_id
FROM student
WHERE resion = 'seoul'; 
```
#### 1.5 CREATE INDEX
```
CREATE UNIQUE INDEX s_id_idx
ON student(s_id DESC, sex ASC)
CLUSTER;
```
#### 1.6 ALTER TABLE
ADD, ALTER, DROP COLLUMN
```
ALTER TABLE student ADD grade VARCHAR(1);
```

```
ALTER TABLE student ALTER s_id VARACHAR(10) NOT NULL;
```

```
ALTER TABLE student DROP COLUMN grade [CASCADE, RESTRICT]
```
#### 1.7 DROP 
```
DROP CONSTRAINT valid_sex;
DROP INDEX s_id_idx RESTRICT;
```

### 2. DML $($Data management language$)$

### 3. DVL $($Data control language$)$

