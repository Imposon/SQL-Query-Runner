# 🎨 Design & Code Quality  

## 🧱 ER Diagram  

A well-structured and clearly labeled **Entity-Relationship Diagram (ERD)** shows how different entities in the **SQL Query Runner** system interact with each other.  
This diagram represents the logical structure of the underlying database — designed using **Prisma ORM** and compatible with **SQLite / MySQL / PostgreSQL**.

---

### 🗂️ Database Entities Overview  

| Entity | Description |
|--------|--------------|
| **User** | Stores information about registered users who can execute queries and save history. |
| **Query** | Represents each SQL command executed by a user, along with timestamp and result status. |
| **DatabaseConnection** | Contains credentials/configuration details for connecting to MySQL/PostgreSQL/SQLite databases. |
| **TableSchema** | Stores metadata about tables created or imported by users (columns, types, constraints). |
| **QueryHistory** | Keeps records of previously executed queries for re-run and reference. |

---

### 🧩 ER Diagram (Mermaid Code)  

```mermaid
erDiagram
    USER {
        int user_id PK
        string name
        string email
        string password
        datetime created_at
    }

    DATABASE_CONNECTION {
        int connection_id PK
        string db_type
        string host
        string username
        string password
        string database_name
        int user_id FK
    }

    QUERY {
        int query_id PK
        string query_text
        string status
        datetime executed_at
        int user_id FK
        int connection_id FK
    }

    TABLE_SCHEMA {
        int table_id PK
        string table_name
        string columns
        int connection_id FK
    }

    QUERY_HISTORY {
        int history_id PK
        string query_text
        string execution_time
        string result_preview
        int user_id FK
    }

    USER ||--o{ DATABASE_CONNECTION : "manages"
    USER ||--o{ QUERY : "executes"
    USER ||--o{ QUERY_HISTORY : "has"
    DATABASE_CONNECTION ||--o{ QUERY : "runs on"
    DATABASE_CONNECTION ||--o{ TABLE_SCHEMA : "defines"
