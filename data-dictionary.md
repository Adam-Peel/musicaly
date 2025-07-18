# Data Dictionary

## Students

| Column Name | Data Type | Constraints    | Description                         | Allowed Values     | Misc Information                        |
| ----------- | --------- | -------------- | ----------------------------------- | ------------------ | --------------------------------------- |
| id          | uuid      | NOT NULL, PKEY | Unique identifier for each student. | N/A                | Automatically generated using UUID.     |
| name        | text      |                | Name of the student.                | N/A                |                                         |
| email       | text      | UNIQUE         | Email address of the student.       | Valid email format | Must be unique across all records.      |
| created_at  | timestamp | DEFAULT now()  | Timestamp when record was created.  | N/A                | Automatically sets to current time.     |
| updated_at  | timestamp | DEFAULT now()  | Timestamp when record last updated. | N/A                | Automatically updates on record change. |
| target_time | integer   |                | Weekly target practice mnutes       | Number             | Must take a number as a string          |

## Tutors

| Column Name | Data Type | Constraints    | Description                         | Allowed Values     | Misc Information                        |
| ----------- | --------- | -------------- | ----------------------------------- | ------------------ | --------------------------------------- |
| id          | uuid      | NOT NULL, PKEY | Unique identifier for each tutor.   | N/A                | Automatically generated using UUID.     |
| name        | text      |                | Name of the tutor.                  | N/A                |                                         |
| email       | text      | UNIQUE         | Email address of the tutor.         | Valid email format | Must be unique across all records.      |
| created_at  | timestamp | DEFAULT now()  | Timestamp when record was created.  | N/A                | Automatically sets to current time.     |
| updated_at  | timestamp | DEFAULT now()  | Timestamp when record last updated. | N/A                | Automatically updates on record change. |

## Games

| Column Name      | Data Type | Constraints               | Description                               | Allowed Values           | Misc Information                        |
| ---------------- | --------- | ------------------------- | ----------------------------------------- | ------------------------ | --------------------------------------- |
| id               | uuid      | NOT NULL, PKEY            | Unique identifier for each game.          | N/A                      | Automatically generated using UUID.     |
| student_id       | uuid      | NOT NULL, FKEY (students) | The student associated with the game.     | Must reference student   |                                         |
| score            | integer   |                           | Score achieved in the game.               | Non-negative integer     |                                         |
| created_at       | timestamp | DEFAULT now()             | Timestamp when the game was created.      | N/A                      | Automatically sets to current time.     |
| updated_at       | timestamp | DEFAULT now()             | Timestamp when the game was last updated. | N/A                      | Automatically updates on record change. |
| difficulty_level | smallint  |                           | Difficulty level of the game.             | 1-5 (1 = Easy, 5 = Hard) |                                         |

## Practice Sessions

| Column Name | Data Type | Constraints                         | Description                                  | Allowed Values                  | Misc Information                        |
| ----------- | --------- | ----------------------------------- | -------------------------------------------- | ------------------------------- | --------------------------------------- |
| id          | uuid      | NOT NULL, PKEY                      | Unique identifier for each session.          | N/A                             | Automatically generated using UUID.     |
| student_id  | uuid      | NOT NULL, FKEY (students)           | The student recording the session.           | Must reference existing student |                                         |
| duration    | integer   |                                     | Duration of the session (minutes).           | Non-negative integer            | Derived from created_at and ended_at    |
| notes       | text      |                                     | Notes about the practice session.            | N/A                             | Max length varies with implementation.  |
| ended_at    | timestamp |                                     | Timestamp when the session was ended         | N/A                             |                                         |
| created_at  | timestamp | DEFAULT now()                       | Timestamp when the session was created.      | N/A                             | Automatically sets to current time.     |
| updated_at  | timestamp | DEFAULT now()                       | Timestamp when the session was last updated. | N/A                             | Automatically updates on record change. |
| rating      | integer   | CHECK (rating >= 1 AND rating <= 5) | Rating of the practice session.              | 1-5                             | Represents the quality rating.          |

## Practice Sessions Units

| Column Name         | Data Type | Constraints                        | Description                         | Allowed Values                  | Misc Information                        |
| ------------------- | --------- | ---------------------------------- | ----------------------------------- | ------------------------------- | --------------------------------------- |
| id                  | uuid      | NOT NULL, PKEY                     | Unique identifier for each unit.    | N/A                             | Automatically generated using UUID.     |
| practice_session_id | uuid      | NOT NULL, FKEY (practice_sessions) | The session associated with unit.   | Must reference existing session |                                         |
| unit_id             | uuid      | NOT NULL, FKEY (units)             | The unit for the practice session.  | Must reference existing unit    |                                         |
| duration            | integer   |                                    | Duration of the unit (in minutes).  | Non-negative integer            | Derived from created_at and ended_at    |
| created_at          | timestamp | DEFAULT now()                      | Timestamp when record created.      | N/A                             | Automatically sets to current time.     |
| updated_at          | timestamp | DEFAULT now()                      | Timestamp when record last updated. | N/A                             | Automatically updates on record change. |
| ended_at            | timestamp |                                    | Timestamp when the unit ended.      | N/A                             |                                         |
| unit_comment        | text      |                                    | Comment about the unit.             | N/A                             | Max length varies with implementation.  |

## Practice Session Unit Media

| Column Name              | Data Type | Constraints                              | Description                         | Allowed Values         | Misc Information                         |
| ------------------------ | --------- | ---------------------------------------- | ----------------------------------- | ---------------------- | ---------------------------------------- |
| id                       | uuid      | NOT NULL, PKEY                           | Unique identifier for media item.   | N/A                    | Automatically generated using UUID.      |
| practice_session_unit_id | uuid      | NOT NULL, FKEY (practice_sessions_units) | The session unit media sent in.     | Must reference unit    |                                          |
| file_path                | text      | NOT NULL                                 | Path to media file.                 | N/A                    | Must be a valid path in the file system. |
| file_name                | text      | NOT NULL                                 | Name of media file.                 | N/A                    | Max length varies with implementation.   |
| file_type                | text      | NOT NULL                                 | Type of media file (audio, video).  | Valid media types      | e.g., 'image/jpeg', 'audio/mpeg'         |
| file_size                | integer   |                                          | Size of media file in bytes.        | Non-negative integer   |                                          |
| created_at               | timestamp | NOT NULL DEFAULT now()                   | Timestamp when media was created.   | N/A                    | Automatically sets to current time.      |
| updated_at               | timestamp | DEFAULT now()                            | Timestamp when media last updated.  | N/A                    | Automatically updates on record change.  |
| student_id               | uuid      | NOT NULL                                 | The student uploading media item.   | Must reference student |                                          |
| description              | text      |                                          | Description of media item.          | N/A                    | Max length varies with implementation.   |
| message                  | text      |                                          | Message associated with media item. | N/A                    | Max length varies with implementation.   |

## Student Monthly Goals

| Column Name      | Data Type | Constraints               | Description                           | Allowed Values         | Misc Information                        |
| ---------------- | --------- | ------------------------- | ------------------------------------- | ---------------------- | --------------------------------------- |
| id               | uuid      | NOT NULL, PKEY            | Unique identifier for each goal.      | N/A                    | Automatically generated using UUID.     |
| student_id       | uuid      | NOT NULL, FKEY (students) | The student associated with the goal. | Must reference student |                                         |
| created_at       | timestamp | DEFAULT now()             | Timestamp when the goal was created.  | N/A                    | Automatically sets to current time.     |
| updated_at       | timestamp | DEFAULT now()             | Timestamp when recorded last updated. | N/A                    | Automatically updates on record change. |
| goal_description | text      |                           | Description of the goal.              | N/A                    | Max length varies with implementation.  |
| goal_date        | timestamp |                           | Deadline for the goal.                | N/A                    |                                         |
| goal_status      | smallint  |                           | Status of the goal.                   | TBC                    |                                         |


## Tutor Student Weekly Goals

| Column Name      | Data Type | Constraints                      | Description                               | Allowed Values                           | Misc Information                        |
| ---------------- | --------- | -------------------------------- | ----------------------------------------- | ---------------------------------------- | --------------------------------------- |
| id               | uuid      | NOT NULL, PKEY                   | Unique identifier for each goal.          | N/A                                      | Automatically generated using UUID.     |
| student_tutor_id | uuid      | NOT NULL, FKEY (students_tutors) | Association between student and tutor.    | Must reference existing student-tutor id |                                         |
| created_at       | timestamp | DEFAULT now()                    | Timestamp when the goal was created.      | N/A                                      | Automatically sets to current time.     |
| updated_at       | timestamp | DEFAULT now()                    | Timestamp when the goal was last updated. | N/A                                      | Automatically updates on record change. |
| goal_date        | timestamp |                                  | Date of the goal.                         | N/A                                      |                                         |
| goal_description | text      |                                  | Description of the goal.                  | N/A                                      | Max length varies with implementation.  |
| status           | smallint  |                                  | Status of the goal.                       | 0-2 (Not started, In progress, Complete) |                                         |

## Units

| Column Name  | Data Type | Constraints    | Description                       | Allowed Values       | Misc Information                        |
| ------------ | --------- | -------------- | --------------------------------- | -------------------- | --------------------------------------- |
| id           | uuid      | NOT NULL, PKEY | Unique identifier for each unit.  | N/A                  | Automatically generated using UUID.     |
| title        | text      | NOT NULL       | Title of the unit.                | N/A                  | Max length varies by database config.   |
| composer     | text      |                | Composer of the unit.             | N/A                  | Max length varies with implementation.  |
| created_at   | timestamp | DEFAULT now()  | Timestamp when unit created.      | N/A                  | Automatically sets to current time.     |
| updated_at   | timestamp | DEFAULT now()  | Timestamp when unit last updated. | N/A                  | Automatically updates on record change. |
| tempo_bpm    | smallint  |                | Tempo in beats per minute.        | Non-negative integer |                                         |
| beats_in_bar | smallint  |                | Beats per bar for metronome       | Non-negative integer |                                         |
| subdivision  | smallint  |                | Subdivisions per beat             | Non-negative integer |                                         |
