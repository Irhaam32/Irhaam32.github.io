# AI-Powered Personalized Learning System - Database Schema Diagram

```mermaid
erDiagram
    users {
        SERIAL user_id PK
        VARCHAR username UK "UNIQUE"
        VARCHAR email UK "UNIQUE"
        VARCHAR password_hash
        VARCHAR role
        TIMESTAMP created_at
    }
    
    courses {
        SERIAL course_id PK
        INTEGER teacher_id FK
        VARCHAR title
        TEXT description
    }
    
    enrollments {
        SERIAL enrollment_id PK
        INTEGER student_id FK
        INTEGER course_id FK
        TIMESTAMP enrollment_date
    }
    
    modules {
        SERIAL module_id PK
        INTEGER course_id FK
        VARCHAR title
        INTEGER module_order
    }
    
    quizzes {
        SERIAL quiz_id PK
        INTEGER module_id FK
        VARCHAR title
        BOOLEAN is_adaptive
    }
    
    questions {
        SERIAL question_id PK
        INTEGER quiz_id FK
        TEXT question_text
        VARCHAR topic_tag
        INTEGER difficulty_level
    }
    
    answer_options {
        SERIAL option_id PK
        INTEGER question_id FK
        TEXT option_text
        BOOLEAN is_correct
    }
    
    quiz_attempts {
        SERIAL attempt_id PK
        INTEGER student_id FK
        INTEGER quiz_id FK
        NUMERIC score
        TIMESTAMP completed_at
    }
    
    student_responses {
        SERIAL response_id PK
        INTEGER attempt_id FK
        INTEGER question_id FK
        INTEGER chosen_option_id FK
    }
    
    ai_analyses {
        SERIAL analysis_id PK
        INTEGER student_id FK
        TEXT_ARRAY weaknesses
        TEXT_ARRAY strengths
        TIMESTAMP analysis_timestamp
    }
    
    recommendations {
        SERIAL recommendation_id PK
        INTEGER student_id FK
        INTEGER recommended_material_id FK
        TEXT reason
    }
    
    learning_materials {
        SERIAL material_id PK
        INTEGER module_id FK
        VARCHAR title
        VARCHAR type
        VARCHAR content_url
    }
    
    %% Relationships
    users ||--o{ courses : "teaches (teacher_id)"
    users ||--o{ enrollments : "enrolls (student_id)"
    courses ||--o{ enrollments : "has enrollments"
    courses ||--|{ modules : "contains"
    modules ||--o{ learning_materials : "contains"
    modules ||--o{ quizzes : "contains"
    quizzes ||--|{ questions : "contains"
    questions ||--|{ answer_options : "has options"
    users ||--o{ quiz_attempts : "attempts (student_id)"
    quizzes ||--o{ quiz_attempts : "is attempted"
    quiz_attempts ||--|{ student_responses : "consists of"
    questions ||--o{ student_responses : "is answered"
    answer_options ||--o{ student_responses : "is chosen"
    users ||--o{ ai_analyses : "has analysis"
    learning_materials ||--o{ recommendations : "is recommended"
    users ||--o{ recommendations : "receives"
```

## Schema Overview

This diagram represents the complete database schema for an AI-Powered Personalized Learning System with the following key components:

### Core Entities
- **users**: System users (students and teachers)
- **courses**: Learning courses created by teachers
- **modules**: Course content organized into modules
- **learning_materials**: Educational resources within modules

### Assessment System
- **quizzes**: Assessments within modules (can be adaptive)
- **questions**: Individual quiz questions with difficulty levels
- **answer_options**: Multiple choice options for questions

### Learning Analytics
- **enrollments**: Student course registrations
- **quiz_attempts**: Student quiz completion records
- **student_responses**: Individual question responses
- **ai_analyses**: AI-generated student performance analysis
- **recommendations**: AI-powered learning material suggestions

### Key Features
- **Adaptive Learning**: Quiz adaptability based on student performance
- **AI Integration**: Automated analysis of student weaknesses/strengths
- **Personalized Recommendations**: AI-driven content suggestions
- **Comprehensive Tracking**: Full audit trail of student interactions
- **Role-based Access**: Support for different user roles (students/teachers)