# AI-Powered Personalized Learning System
## Sequence Diagram: Student Views a Learning Material

```mermaid
sequenceDiagram
    participant Student as :Student
    participant Frontend as :Frontend
    participant BackendAPI as :BackendAPI
    participant Database as :Database

    Note over Student, Database: Use Case: Student Views a Learning Material<br/>Template 1: Simple Data Retrieval

    %% Step 1: Student clicks material link
    Student->>+Frontend: clicks on learning material link<br/>(video or article)
    
    %% Step 2: Frontend requests material data
    Frontend->>+BackendAPI: GET /api/materials/{materialId}
    
    %% Step 3: Backend queries for material details
    BackendAPI->>+Database: query material details<br/>by materialId
    
    %% Step 4: Database returns material data
    Database-->>-BackendAPI: return material data<br/>(title, URL, type, description)
    
    %% Step 5: Backend returns material data
    BackendAPI-->>-Frontend: return material data<br/>as JSON response
    
    %% Step 6: Frontend displays learning material
    Frontend-->>-Student: display learning material<br/>(video player or article content)

    Note over Student, Database: Learning material successfully loaded and displayed
```