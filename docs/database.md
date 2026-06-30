# Database Design

## Overview
The AI-Powered Placement & Interview Preparation Platform uses MySQL as the relational database management system. The database is designed using normalization principles to minimize redundancy and maintain data consistency.

The database stores information related to:
- Students
- Resume Management
- AI Interviews
- Coding Practice
- Skill Gap Analysis
- Learning Roadmaps
- Company Preparation
- Administrator Management

## Database Tables

The platform consists of the following database tables:
<span style="color: yellow;">1</span> Student
<span style="color: yellow;">2</span> Resume
<span style="color: yellow;">3</span> ResumeAnalysis
<span style="color: yellow;">4</span> Interview
<span style="color: yellow;">5</span> InterviewQuestion
<span style="color: yellow;">6</span> CodingQuestion
<span style="color: yellow;">7</span> CodingSubmission
<span style="color: yellow;">8</span> Company
<span style="color: yellow;">9</span> LearningRoadmap
<span style="color: yellow;">10</span> SkillGap
<span style="color: yellow;">11</span> Admin

## Student Table

### Purpose
Stores the profile information of every student registered on the AI-Powered Placement & Interview Preparation Platform. It contains personal details, academic information, and account-related information.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| student_id | BIGINT | Primary Key, Auto Increment | Unique Student ID |
| full_name | VARCHAR(100) | NOT NULL | Student Full Name |
| email | VARCHAR(100) | UNIQUE, NOT NULL | Login Email Address |
| password | VARCHAR(255) | NOT NULL | Encrypted Password |
| phone_number | VARCHAR(15) | NULL | Optional Contact Number |
| date_of_birth | DATE | NOT NULL | Student Date of Birth |
| college | VARCHAR(150) | NOT NULL | College Name |
| department | VARCHAR(100) | NOT NULL | Department Name |
| graduation_year | INT | NOT NULL | Expected Graduation Year |
| cgpa | DECIMAL(3,2) | NULL | Current CGPA |
| profile_picture | VARCHAR(255) | NULL | Profile Image Path |
| created_at | TIMESTAMP | NOT NULL | Account Creation Time |
| updated_at | TIMESTAMP | NOT NULL | Last Profile Update Time |

### Relationship
- One Student can upload multiple Resumes.
- One Student can attend multiple AI Interviews.
- One Student can submit multiple Coding Solutions.
- One Student has one AI Learning Roadmap.
- One Student can have multiple Skill Gap records.


## Resume Table

### Purpose
Stores every resume uploaded by a student. A student can upload multiple versions of their resume over time. This table stores only the resume file information, while AI analysis results are stored separately.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| resume_id | BIGINT | Primary Key, Auto Increment | Unique Resume ID |
| student_id | BIGINT | Foreign Key, NOT NULL | References Student |
| resume_title | VARCHAR(100) | NOT NULL | Resume Name or Version |
| resume_path | VARCHAR(255) | NOT NULL | Resume File Location |
| upload_date | TIMESTAMP | NOT NULL | Resume Upload Time |
| resume_status | VARCHAR(20) | NOT NULL | Active / Archived |

### Relationship
- One Student can upload multiple Resumes.
- Each Resume belongs to only one Student.
- Each Resume has one Resume Analysis.


## ResumeAnalysis Table

### Purpose
Stores the AI-generated analysis results for each uploaded resume.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| analysis_id | BIGINT | Primary Key, Auto Increment | Unique Analysis ID |
| resume_id | BIGINT | Foreign Key, UNIQUE | References Resume |
| ats_score | DECIMAL(5,2) | NOT NULL | Overall ATS Score |
| grammar_score | DECIMAL(5,2) | NOT NULL | Grammar Quality Score |
| keyword_score | DECIMAL(5,2) | NOT NULL | Resume Keyword Match Score |
| formatting_score | DECIMAL(5,2) | NOT NULL | Resume Formatting Score |
| project_score | DECIMAL(5,2) | NOT NULL | Project Quality Score |
| skill_match_score | DECIMAL(5,2) | NOT NULL | Skill Matching Score |
| ai_feedback | TEXT | NOT NULL | AI Suggestions for Improvement |
| analyzed_at | TIMESTAMP | NOT NULL | Analysis Time |

### Relationship
- One Resume has one Resume Analysis.
- Each Resume Analysis belongs to one Resume.

## Interview Table

### Purpose
Stores every interview session attended by a student.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| interview_id | BIGINT | Primary Key, Auto Increment | Unique Interview ID |
| student_id | BIGINT | Foreign Key, NOT NULL | References Student |
| interview_type | VARCHAR(50) | NOT NULL | HR / Technical / Managerial |
| difficulty_level | VARCHAR(20) | NOT NULL | Easy / Medium / Hard |
| overall_score | DECIMAL(5,2) | NULL | Overall Interview Score |
| confidence_score | DECIMAL(5,2) | NULL | AI Confidence Score |
| communication_score | DECIMAL(5,2) | NULL | Communication Score |
| interview_date | TIMESTAMP | NOT NULL | Interview Date |
| duration_minutes | INT | NOT NULL | Interview Duration |

### Relationship
- One Student can attend multiple Interviews.
- One Interview contains multiple Interview Questions.

## InterviewQuestion Table

### Purpose
Stores every question asked during an interview along with the student's answer and AI evaluation.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| question_id | BIGINT | Primary Key, Auto Increment | Unique Question ID |
| interview_id | BIGINT | Foreign Key, NOT NULL | References Interview |
| question | TEXT | NOT NULL | Interview Question |
| student_answer | LONGTEXT | NOT NULL | Student Response |
| ai_feedback | TEXT | NOT NULL | AI Feedback |
| score | DECIMAL(5,2) | NULL | Question Score |

### Relationship
- One Interview contains multiple Interview Questions.
- Each Interview Question belongs to one Interview.

## CodingQuestion Table

### Purpose
Stores all coding problems available in the platform.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| coding_question_id | BIGINT | Primary Key, Auto Increment | Unique Coding Question ID |
| title | VARCHAR(150) | NOT NULL | Question Title |
| difficulty | VARCHAR(20) | NOT NULL | Easy / Medium / Hard |
| topic | VARCHAR(100) | NOT NULL | Arrays, Graphs, DP, etc. |
| problem_statement | LONGTEXT | NOT NULL | Coding Problem Description |
| sample_input | TEXT | NULL | Example Input |
| sample_output | TEXT | NULL | Example Output |
| constraints | TEXT | NULL | Problem Constraints |

### Relationship
- One Coding Question can have multiple Coding Submissions.

## CodingSubmission Table

### Purpose
Stores every coding solution submitted by a student.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| submission_id | BIGINT | Primary Key, Auto Increment | Unique Submission ID |
| student_id | BIGINT | Foreign Key, NOT NULL | References Student |
| coding_question_id | BIGINT | Foreign Key, NOT NULL | References Coding Question |
| programming_language | VARCHAR(30) | NOT NULL | Java / Python / C++ |
| source_code | LONGTEXT | NOT NULL | Submitted Source Code |
| execution_time | DECIMAL(5,2) | NULL | Execution Time |
| memory_usage | DECIMAL(6,2) | NULL | Memory Usage |
| score | DECIMAL(5,2) | NULL | Submission Score |
| submitted_at | TIMESTAMP | NOT NULL | Submission Time |

### Relationship
- One Student can have multiple Coding Submissions.
- One Coding Question can have multiple Coding Submissions.

## Company Table

### Purpose
Stores information about companies visiting for placements. This helps students prepare based on company requirements and interview patterns.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| company_id | BIGINT | Primary Key, Auto Increment | Unique Company ID |
| company_name | VARCHAR(150) | UNIQUE, NOT NULL | Company Name |
| package_lpa | DECIMAL(5,2) | NULL | Salary Package (LPA) |
| eligibility_cgpa | DECIMAL(3,2) | NULL | Minimum Required CGPA |
| job_role | VARCHAR(100) | NOT NULL | Offered Job Role |
| location | VARCHAR(100) | NULL | Job Location |
| interview_pattern | TEXT | NULL | Interview Process |
| preparation_tips | LONGTEXT | NULL | AI Preparation Guide |
| created_at | TIMESTAMP | NOT NULL | Record Creation Time |

### Relationship
- One Company can be recommended to multiple Students.

## LearningRoadmap Table

### Purpose
Stores AI-generated personalized learning roadmaps for students based on their resume analysis, coding performance, and interview results.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| roadmap_id | BIGINT | Primary Key, Auto Increment | Unique Roadmap ID |
| student_id | BIGINT | Foreign Key, NOT NULL | References Student |
| roadmap_title | VARCHAR(150) | NOT NULL | Roadmap Title |
| roadmap_content | LONGTEXT | NOT NULL | AI Generated Learning Plan |
| target_company | VARCHAR(100) | NULL | Target Company |
| estimated_completion_days | INT | NULL | Estimated Completion Time |
| generated_at | TIMESTAMP | NOT NULL | Roadmap Generation Time |
| updated_at | TIMESTAMP | NOT NULL | Last Update Time |

### Relationship
- One Student can have multiple Learning Roadmaps.

## SkillGap Table

### Purpose
Stores the skills that the AI identifies as missing or requiring improvement for each student.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| skillgap_id | BIGINT | Primary Key, Auto Increment | Unique Skill Gap ID |
| student_id | BIGINT | Foreign Key, NOT NULL | References Student |
| skill_name | VARCHAR(100) | NOT NULL | Missing Skill Name |
| current_level | VARCHAR(30) | NULL | Beginner / Intermediate / Advanced |
| required_level | VARCHAR(30) | NULL | Required Skill Level |
| priority | VARCHAR(20) | NOT NULL | High / Medium / Low |
| recommendation | LONGTEXT | NULL | AI Recommendation |
| identified_at | TIMESTAMP | NOT NULL | Skill Analysis Time |

### Relationship
- One Student can have multiple Skill Gap records.

## Admin Table

### Purpose
Stores administrator account details used to manage the platform, companies, coding questions, and users.

| Column | Type | Constraints | Description |
|---------|------|-------------|-------------|
| admin_id | BIGINT | Primary Key, Auto Increment | Unique Admin ID |
| full_name | VARCHAR(100) | NOT NULL | Administrator Name |
| email | VARCHAR(100) | UNIQUE, NOT NULL | Administrator Email |
| password | VARCHAR(255) | NOT NULL | Encrypted Password |
| role | VARCHAR(30) | NOT NULL | Super Admin / Admin |
| created_at | TIMESTAMP | NOT NULL | Account Creation Time |

### Relationship
- One Admin can manage multiple Companies.
- One Admin can manage multiple Coding Questions.





                    +----------------+
                    |    Student     |
                    +----------------+
                           |
         +-----------------+------------------+
         |                 |                  |
         |                 |                  |
         v                 v                  v
     +--------+      +-----------+     +--------------+
     | Resume |      | Interview |     | CodingSubmission |
     +--------+      +-----------+     +--------------+
         |                 |                  |
         v                 v                  |
+----------------+   +------------------+     |
| ResumeAnalysis |   | InterviewQuestion|     |
+----------------+   +------------------+     |
                                              |
                                              |
                                     +------------------+
                                     | CodingQuestion   |
                                     +------------------+

         Student
             |
             +----------------------+
             |                      |
             v                      v
     +----------------+     +------------------+
     | LearningRoadmap|     |    SkillGap      |
     +----------------+     +------------------+

+-------------+
|   Company   |
+-------------+

+-------------+
|    Admin    |
+-------------+