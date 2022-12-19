# Instructr

## Infrastructure
```mermaid
flowchart LR
    A --- B
    C --- D[OpenAPI] --- E
    C --- G --- E
    E -. Upload<br/>Client Video .-> H --- C
    

    subgraph Data Storage
    A[Azure SQL]
    H[Azure Media Services]
    end

    subgraph API
    B[EF Core] --- C[.NET Web API]
    end
    
    subgraph Frontend
    E[ReactJS] --- F[Chakra UI]
    end
 
    subgraph Authentication
    G[Firebase Auth]
    end
```

### User sign-up flows
**Regular client sign-up**
```mermaid
graph LR
    A(New client) --> |Send passwordless link| B(Pending verification)
    B --> |"Passwordless link press (email cached)"| C(Client created)
    B --> |Passwordless link press| D(Confirming email)
    D --> |Email typed in correctly| C
    subgraph Client created in<br/>Firebase and backend
    C
    end
```
**Instructor adds client to lesson**
```mermaid
graph LR
    A(New client) --> |Create by email and name| B(Pending verification)
    subgraph Client created in backend
    B --> |Send passwordless link| B
    D
    end
    B --> |"Passwordless link press (email cached)"| C(Client created)
    B --> |Passwordless link press| D(Confirming email)
    D --> |Email typed in correctly| C
    subgraph Client created in<br/>Firebase and backend
    C(Client created)
    end
```

### Client Video Upload & Conversion Flow
```mermaid
graph TD
    A[Frontend] -->|Upload video| B(Azure Blob Container named input-videos)
    B -->|Trigger Azure Function| C[Azure Function]
    C -->|Gets access to Media Service| D[Azure Media Services]
    D -->|1. Creates Input Asset| E[Input Asset]
    D -->|2. Creates Output Asset| F[Output Asset]
    D -->|3. Start Job to encode video| G[Azure Media Services Job]
    E -->|Takes Input Asset to convert| G
    G -->|Puts converted video and thumbnail in Output Asset| F

    subgraph Azure Media Services
    D
    E
    F
    G
    end

```

---

Made with [mermaid](https://mermaid-js.github.io/mermaid/#/flowchart)
