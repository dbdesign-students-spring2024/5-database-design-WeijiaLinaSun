## Report: Data Normalization and Entity-Relationship Diagramming

### a table containing the original data set (research how to write tables in Markdown or simply see the example table in this document's source code)

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

### your description of what makes this data set not compliant with 4NF

The dataset does not match the following:

- A non-key field *must provide a fact about the entity uniquely identified by the primary key*.
- It is not allowed for a non-key field to provide a fact about only a part of that entity or about some other unrelated entity.
- The fact could be a one-to-many relationship, such as the department of an employee, or a one-to-one relationship.
- Third normal form is *violated when a non-key field is a fact about another non-key field*.
- Not contain more than one independent multi-valued fact about an entity.

### tables containing the 4NF-compliant version of the data set

#### Professors:

| ProfessorID | Name    | Email             |
| ----------- | ------- | ----------------- |
| 1           | Melvin  | l.melvin@foo.edu  |
| 2           | Logston | e.logston@foo.edu |
| 3           | Nevarez | i.nevarez@foo.edu |

#### Classrooms:

| ClassroomID | ClassroomLocation |
| ----------- | ----------------- |
| 1           | WWH 101           |
| 2           | WWH 201           |
| 3           | 60FA 314          |

#### Courses:

| CourseID | CourseName                       |
| -------- | -------------------------------- |
| 1        | Python                           |
| 2        | Database Design & Implementation |
| 3        | Data Science                     |

#### Sections:

| SectionID | ClassroomID | ProfessorID | CourseID | Assignment_topic                |
| --------- | ----------- | ----------- | -------- | ------------------------------- |
| 1         | 3           | 2           | 2        | Single table queries            |
| 2         | 3           | 2           | 1        | Python and pandas               |
| 3         | 1           | 1           | 2        | Data normalization              |
| 4         | 2           | 3           | 2        | Spreadsheet aggregate functions |

#### Students:

| StudentID | Name      | Gender | Email                 | Address                             |
| --------- | --------- | ------ | --------------------- | ----------------------------------- |
| 1         | Elizabeth | Female | elizabeth@example.com | 678 Elm St, Chicago, IL, USA        |
| 2         | James     | Male   | james@example.com     | 162 Maple St, New York, NY, USA     |
| 3         | Ava       | Female | ava@example.com       | 736 Pine St, Los Angeles, CA, USA   |
| 4         | Jennifer  | Female | jennifer@example.com  | 692 Oak St, Houston, TX, USA        |
| 5         | Mary      | Female | mary@example.com      | 210 Elm St, Phoenix, AZ, USA        |
| 6         | William   | Male   | william@example.com   | 122 Maple St, Philadelphia, PA, USA |
| 7         | Isabella  | Female | isabella@example.com  | 424 Main St, San Antonio, TX, USA   |

#### Readings:

| ReadingID | AssignmentID | MaterialDescription |
| --------- | ------------ | ------------------- |
| 1         | 3            | Deumlich Chapter 3  |
| 2         | 1            | Dümmlers Chapter 11 |
| 3         | 2            | Dümmlers Chapter 14 |
| 4         | 4            | Zehnder Page 87     |

#### Assignments:

| AssignmentID | SectionID | DueDate  | AssignmentDescription     |
| ------------ | --------- | -------- | ------------------------- |
| 1            | 1         | 23.02.21 | Single table queries...   |
| 2            | 2         | 05.05.21 | Python and pandas ....    |
| 3            | 3         | 23.02.21 | Data normalization ...    |
| 4            | 4         | 04.07.21 | Spreadsheet aggregate ... |

#### Grade:

| GradeID | StudentID | AssignmentID | Grade |
| ------- | --------- | ------------ | ----- |
| 1       | 7         | 1            | 25    |
| 2       | 2         | 2            | 92    |
| 3       | 1         | 3            | 80    |
| 4       | 4         | 3            | 75    |
| 5       | 2         | 4            | 65    |

### the ER diagram(s) you created of your 4NF-compliant version of the data set (this diagram must be visible on the `README.md` document, not simply linked from there - research how to publish images to your pages in Markdown or simply see the example image in this document's source code)

![Entity-Relationship Diagram](./images/ER.svg)

### your description of what changes you made and how these changes make the data 4NF-compliant

The requirement for 4NF is that the data tables must be in third normal form and not contain any multi-value dependencies unless they are part of a candidate key. To make the data structure 4NF compliant, I made the following key changes.

First I decomposed the entities and attributes, identified and defined multiple entities including Classrooms, Courses, Professors, Readings, Students, Assignments, Sections. The primary key of all these entities was set to surrogate key. I also defined the associated attributes of these entities.

I then clarified the relationships between the defining entities, including that Courses, Classrooms, and Professors are all one-to-many with Sections, respectively; Professors are many-to-many with Assignments, Grades, and Readings, respectively; Assignments are many-to-one with Sections and Students, respectively; and Readings are one-to-one with Assignments.

Then again, I also set foreign keys. This includes setting ClassroomID, ProfessorID and CourseID as foreign key in the sections table. in the assignments table, setting SectionID as foreign key. In the grades table, set AssignmentID and StudentID to foreign key.

After that, I normalized the relationship. I made sure all the data tables were 3NF compliant, then checked further and eliminated any multi-value dependencies.

Finally, I refined the entities and attributes. For each defined entity, I make sure that its attributes contain only information that is directly relevant to that entity.
