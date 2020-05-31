# Draw-SQL
Draw table sketches and we turn them into SQL INSERT Statements like magic.
Integration tests with relational databases will never be a pain again.

[![Build Status](https://travis-ci.org/talk-code/DrawSQL.svg?branch=master)](https://travis-ci.org/talk-code/DrawSQL)

## What does DrawSQL do?

### This sketch

```text
@person
-----------------------
id  | name | age      |
-----------------------
1     Enuar  21
2     Gaby   23
3     Yman   26
-----------------------
```

### Becomes this:

```sql
INSERT INTO person(id,name,age) VALUES(1,'Enuar',21);
INSERT INTO person(id,name,age) VALUES(2,'Gaby',23);
INSERT INTO person(id,name,age) VALUES(3,'Yman',26);
```


# How?

## Getting SQL from sketch file
```java
      DrawSQL.Builder builder = new DrawSQL.Builder();
      DrawSQL drawSQL = builder.fromSketch(new File("your_sketch_file_path")).build();
      String generatedSql = drawSQL.getSQL(); //This will return the generated SQL.

```


## Inserting from a sketch file directly to a JDBC connection
```java

      drawSQL.apply(jdbcConnection);//This will execute the sql against a JDBC Connection object.

```


# Get

## Download JAR
[Download JAR File clicking here](https://talk-code.github.io/releases/get/DrawSQL/drawSQL-1.0.jar)

## Maven project

### Repository
```xml
    ...
    <repository>
       <id>talk-code</id>
       <name>maven-repo</name>
       <url>https://github.com/talk-code/maven-repo/raw/master</url>
    </repository>
    ...

```

### Dependency
```xml
    ...
    <dependency>
        <groupId>mz.co.talkcode</groupId>
        <artifactId>DrawSQL</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    ...

```



# What are the sketching rules?
### Align the column name and the values both to left.
The first character of a column name should be aligned with its values

#### wrong
```text
@person
-----------------------
id  | name   | age    | 
-----------------------
 1     Enuar  21
 2     Gaby   23
 3     Yman   26
-----------------------
```
#### correct
```text
@person
-----------------------
id  | name | age      |
-----------------------
1     Enuar  21
2     Gaby   23
3     Yman   26
-----------------------
```

### Align the column separator (|) to the right of the largest column value
#### wrong
```text
@person
------------------------
id  | name |       age |      
------------------------
 1     Enuar Ben   21
 2     Gaby        23
 3     Yman        26
------------------------
```
#### correct
```text
@person
------------------------
id  | name      |  age |      
------------------------
 1     Enuar Ben   21
 2     Gaby        23
 3     Yman        26
------------------------
```

### Columns with semicolons
1. Columns with whitespaces will be trimmed automatically

2. In case you want to skip trimming and insert whitespace explicitly you have to put a semicolon where it ends.

3. In case you want to have whitespace and semicolon explicitly inserted, you have to put two semicolons.