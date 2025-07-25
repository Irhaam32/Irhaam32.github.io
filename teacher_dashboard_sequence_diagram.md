# AI-Powered Personalized Learning System
## Sequence Diagram: Teacher Views Dashboard

```mermaid
sequenceDiagram
    participant Teacher as :Teacher
    participant Frontend as :Frontend
    participant BackendAPI as :BackendAPI
    participant Database as :Database

    Note over Teacher, Database: Use Case: Teacher Views Dashboard<br/>Template 1: Simple Data Retrieval

    %% Step 1: Teacher logs in and navigates
    Teacher->>+Frontend: logs in and navigates<br/>to dashboard
    
    %% Step 2: Frontend requests dashboard data
    Frontend->>+BackendAPI: GET /api/teacher/dashboard
    
    %% Step 3: Backend queries for teacher's courses
    BackendAPI->>+Database: query teacher's courses<br/>and enrollment data
    
    %% Step 4: Database returns course data
    Database-->>-BackendAPI: return courses list<br/>with student counts
    
    %% Step 5: Backend queries for student alerts
    BackendAPI->>+Database: query student alerts<br/>and performance issues
    
    %% Step 6: Database returns alerts data
    Database-->>-BackendAPI: return student alerts<br/>and notifications
    
    %% Step 7: Backend returns aggregated data
    BackendAPI-->>-Frontend: return dashboard JSON<br/>(courses, alerts, statistics)
    
    %% Step 8: Frontend displays dashboard
    Frontend-->>-Teacher: display teacher dashboard<br/>with courses and alerts

    Note over Teacher, Database: Teacher dashboard successfully loaded with course and student information
```