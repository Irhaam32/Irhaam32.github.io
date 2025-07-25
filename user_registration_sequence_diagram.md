# AI-Powered Personalized Learning System
## Sequence Diagram: User Registration

```mermaid
sequenceDiagram
    participant User as :User
    participant Frontend as :Frontend
    participant BackendAPI as :BackendAPI
    participant Database as :Database

    Note over User, Database: Use Case: User Registration<br/>Template 2: Simple Data Submission

    %% Step 1: User fills registration form
    User->>+Frontend: fills registration form<br/>(username, email, password)
    
    %% Step 2: Frontend sends registration request
    Frontend->>+BackendAPI: POST /api/users/register<br/>with form data
    
    %% Step 3: Backend validates data
    BackendAPI->>+Database: validate data<br/>(check username uniqueness)
    Database-->>-BackendAPI: return validation result
    
    %% Step 4: Backend hashes password
    Note over BackendAPI: Hash password for<br/>secure storage
    
    %% Step 5: Backend creates new user
    BackendAPI->>+Database: INSERT new user record<br/>with hashed password
    
    %% Step 6: Database confirms creation
    Database-->>-BackendAPI: confirm record created<br/>with user_id
    
    %% Step 7: Backend returns success response
    BackendAPI-->>-Frontend: return success JSON response<br/>{ "success": true, "user_id": 456 }
    
    %% Step 8: Frontend displays success message
    Frontend-->>-User: display "Registration Successful"<br/>message

    Note over User, Database: User successfully registered and confirmation displayed
```