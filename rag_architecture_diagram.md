# RAG System Architecture Diagram

```mermaid
graph TD
    %% Title
    subgraph TITLE [" "]
        T[<b>🏗️ RAG SYSTEM ARCHITECTURE</b><br/>Retrieval-Augmented Generation Platform]
    end
    
    %% Left Column - INGESTION FLOW
    subgraph LEFT_COL ["🔵 INGESTION FLOW<br/>(When a Bot Creator uploads a document)"]
        %% Actor
        BC[👤 Bot Creator<br/>SaaS User]
        
        %% Backend Server Swimlane
        subgraph BACKEND_ING ["🖥️ Your Backend Server"]
            FASTAPI_ING[⚡ Python Application<br/>FastAPI]
            FAISS_ING[🧠 FAISS Library<br/>In-Memory]
            
            %% Local Disk Storage Sub-swimlane
            subgraph STORAGE_ING ["💾 Local Disk Storage"]
                ORIG_FOLDER[📁 /original_files/<br/>{user_id}/{bot_id}/]
                VEC_FOLDER[📁 /vector_indexes/<br/>{user_id}/{bot_id}/]
            end
        end
        
        %% Cloud APIs Swimlane
        subgraph CLOUD_ING ["☁️ Cloud APIs"]
            EMB_API_ING[🔗 OpenAI Embedding API]
        end
    end
    
    %% Vertical Separator
    subgraph SEPARATOR [" "]
        SEP["|"]
    end
    
    %% Right Column - QUERY FLOW
    subgraph RIGHT_COL ["🟢 QUERY FLOW<br/>(When an End-User chats with the bot)"]
        %% Actor
        EU[👥 End-User<br/>via Iframe]
        
        %% Backend Server Swimlane
        subgraph BACKEND_QRY ["🖥️ Your Backend Server"]
            FASTAPI_QRY[⚡ Python Application<br/>FastAPI]
            FAISS_QRY[🧠 FAISS Library<br/>In-Memory]
            
            %% Local Disk Storage Sub-swimlane
            subgraph STORAGE_QRY ["💾 Local Disk Storage"]
                VEC_FOLDER_QRY[📁 /vector_indexes/<br/>{user_id}/{bot_id}/]
                CHAT_FOLDER[📁 /chat_history/<br/>{chat_id}.json]
            end
            
            %% Prompt Note
            PROMPT_NOTE[📝 <b>Prompt Contains:</b><br/>• Retrieved Chunks<br/>• Chat History<br/>• New User Query]
        end
        
        %% Cloud APIs Swimlane
        subgraph CLOUD_QRY ["☁️ Cloud APIs"]
            EMB_API_QRY[🔗 OpenAI Embedding API]
            LLM_API[🤖 LLM API<br/>GPT-4o / Claude 3]
        end
    end
    
    %% INGESTION FLOW ARROWS (Blue)
    BC -->|"1. Uploads Document<br/>({user_id}, {bot_id})"| FASTAPI_ING
    FASTAPI_ING -->|"2. Saves Original File"| ORIG_FOLDER
    FASTAPI_ING -->|"3. Sends Chunks<br/>for Embedding"| EMB_API_ING
    EMB_API_ING -->|"4. Receives Vectors"| FASTAPI_ING
    FASTAPI_ING -->|"5. Builds Index"| FAISS_ING
    FAISS_ING -->|"6. Saves Index to Disk"| VEC_FOLDER
    
    %% QUERY FLOW ARROWS (Green)
    EU -->|"7. Sends Query<br/>({user_id}, {bot_id}, {chat_id})"| FASTAPI_QRY
    CHAT_FOLDER -->|"8. Loads History"| FASTAPI_QRY
    VEC_FOLDER_QRY -->|"9. Loads Index File"| FASTAPI_QRY
    FASTAPI_QRY -->|"10. Populates<br/>In-Memory Index"| FAISS_QRY
    FASTAPI_QRY -->|"11. Embeds User Query"| EMB_API_QRY
    FASTAPI_QRY -->|"12. Searches for Context"| FAISS_QRY
    FASTAPI_QRY -->|"13. Sends Augmented Prompt"| LLM_API
    FASTAPI_QRY -.->|"13. References"| PROMPT_NOTE
    FASTAPI_QRY -->|"14. Updates History"| CHAT_FOLDER
    FASTAPI_QRY -.->|"15. Streams Final Answer"| EU
    
    %% Styling
    classDef actor fill:#e1f5fe,stroke:#0277bd,stroke-width:3px,color:#000
    classDef backend fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    classDef storage fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    classDef cloud fill:#e8f5e8,stroke:#388e3c,stroke-width:2px,color:#000
    classDef note fill:#fff9c4,stroke:#f9a825,stroke-width:2px,color:#000
    classDef title fill:#ffffff,stroke:#333,stroke-width:3px,color:#000,font-weight:bold
    classDef separator fill:transparent,stroke:transparent
    
    class BC,EU actor
    class FASTAPI_ING,FASTAPI_QRY,FAISS_ING,FAISS_QRY backend
    class ORIG_FOLDER,VEC_FOLDER,VEC_FOLDER_QRY,CHAT_FOLDER storage
    class EMB_API_ING,EMB_API_QRY,LLM_API cloud
    class PROMPT_NOTE note
    class T title
    class SEP separator
    
    %% Link styles for different flows
    linkStyle 0 stroke:#2196f3,stroke-width:3px
    linkStyle 1 stroke:#2196f3,stroke-width:3px
    linkStyle 2 stroke:#2196f3,stroke-width:3px
    linkStyle 3 stroke:#2196f3,stroke-width:3px
    linkStyle 4 stroke:#2196f3,stroke-width:3px
    linkStyle 5 stroke:#2196f3,stroke-width:3px
    
    linkStyle 6 stroke:#4caf50,stroke-width:3px
    linkStyle 7 stroke:#4caf50,stroke-width:3px
    linkStyle 8 stroke:#4caf50,stroke-width:3px
    linkStyle 9 stroke:#4caf50,stroke-width:3px
    linkStyle 10 stroke:#4caf50,stroke-width:3px
    linkStyle 11 stroke:#4caf50,stroke-width:3px
    linkStyle 12 stroke:#4caf50,stroke-width:3px
    linkStyle 13 stroke:#4caf50,stroke-width:2px,stroke-dasharray: 5 5
    linkStyle 14 stroke:#4caf50,stroke-width:3px
    linkStyle 15 stroke:#4caf50,stroke-width:3px,stroke-dasharray: 5 5
```