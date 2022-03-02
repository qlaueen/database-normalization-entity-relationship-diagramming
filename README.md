# Data Normalization and Entity-Relationship Diagramming
## Original dataset

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

## Fourth Normal Form
The Fourth Normal Form states that a record type should not contain two or more independent multi-valued facts about an entity. In the case of the record given, it is possible to see that it violates the 4NF in a few occasions.
First, the Professors' names are repeated. The reason why this is not optimal is because if a mistake was made in one of the professors names, multiple records would have to be updated. It would also be a problem to update the professors'email if it changed. 

## Suggested dataset updates

### Professors table

| professor_id * | professor | professor_email   |
|:---------------|:--------- |:------------------|
| 1              | Melvin    | l.melvin@foo.edu  |
| 2              | Longston  | e.logston@foo.edu |
| 3              | Nevarez   | i.nevarez@foo.edu |
| ...            | ...       | ...               |

### Classrooms table

| classroom_id * | classroom |
|:---------------|:----------|
| 1              | 60FA 314  |
| 2              | WWH 101   |
| ...            | ...       |

### Courses table

| section * | course name | professor_id | classroom_id | 
|:----------|:----------- |:-------------|:-------------|
| 1         | Course 1    | 1            | 1            |
| 2         | Course 2    | 2            | 2            |
| 3         | Course 3    | 3            | 1            |
| ...       | ...         | ...          | ...          |

### Assignments and relevat readings table

| assignment_id * | assignment_topic                | relevant_reading    | section |
| :-------------- | :------------------------------ | :------------------ | :------ | 
| 1               | Data normalization              | Deumlich Chapter 3  | 1       | 
| 2               | Single table queries            | Dümmlers Chapter 11 | 2       | 
| 5               | Python and pandas               | Dümmlers Chapter 14 | 2       | 
| 4               | Spreadsheet aggregate functions | Dümmlers Chapter 14 | 3       | 
| ...             | ...                             | ...                 | ...     | 

### Assignments with due dates and grades 

| assignment_id * | student_id | due_date | section | grade |
| :-------------- | :--------- | :------- | :------ | :---- |
| 1               | 1          | 23.02.21 | 1       | 80    | 
| 2               | 7          | 18.11.21 | 2       | 25    | 
| 1               | 4          | 23.02.21 | 1       | 75    | 
| 4               | 2          | 04.07.21 | 3       | 65    | 
| ...             | ...        | ...      | ...     | ...   |

## ER Diagram
![Entity-Relationship Diagram](./images/entity-diagram.drawio.svg)

## Changes to original database
In order to change the table to be compliant to the 4FN, I had to create multiple tables for each entity, with the exception of students since there were no information on them besides the ID. I separated between Professors, where I was able to put their emails and names. With that, if their email changed, it would only be necessary to change one single record. Then, I created a classrooms table, which would be helpful if more classrooms were added or deleted, or if they changed. A table for the courses were also created, since different sessions can be taught by the same professor. Although courses can have the same name, they can be taught by different professors, and if they are by the same one, the session would be different. That means that it wouldn't be a problem to repeat the course name. After that, I created the table relating the assignments, the section (which already had been separated by the professors and classrooms in the previous table) and the relevant readings. That would also be helpful if the relevant reading changed  since it wouldn't be necessary to update many times. THe last step was to relate the students, which included a table with the assignment id, the student id, the due date, the section and the grade that the student got.