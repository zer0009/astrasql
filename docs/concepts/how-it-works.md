# How AstraSQL Works

Understanding the architecture and workflow of AstraSQL Agent.

## Overview

AstraSQL Agent converts natural language questions into SQL queries using AI, while maintaining privacy by only sending database metadata (schema information) to the AI model, never your actual data.

## Architecture

```
┌─────────────┐
│   User      │
│  (Browser)  │
└──────┬──────┘
       │
       │ Natural Language Query
       │ "Show top 10 customers"
       ▼
┌─────────────────────┐
│  Frontend (React)   │
│  - Query Interface  │
│  - Dashboards       │
│  - Visualization    │
└──────┬──────────────┘
       │
       │ HTTP Request (REST API)
       ▼
┌─────────────────────────────┐
│  Backend (Python FastAPI)  │
│  - API Endpoints            │
│  - Authentication & RBAC    │
│  - Request Validation       │
└──────┬──────────────────────┘
       │
       │ Invoke Agent
       ▼
┌─────────────────────────────┐
│  LangGraph Agent           │
│  (Orchestration Layer)      │
│                             │
│  - State Management         │
│  - Multi-step Reasoning     │
│  - Tool Selection           │
│  - Error Recovery           │
└──────┬──────────────────────┘
       │
       ├─────────────────┐
       │                 │
       ▼                 ▼
┌──────────────┐  ┌──────────────────────┐
│   MongoDB  │  │  LangChain Tools       │
│  (Metadata)  │  │  - Schema Retriever  │
│              │  │  - SQL Generator     │
│              │  │  - Query Validator   │
└──────┬───────┘  └──────┬───────────────┘
       │                 │
       │ Schema Info     │ SQL Query
       │                 │
       ▼                 ▼
┌─────────────────────────────┐
│      LLM Provider           │
│  (via LangChain)            │
│  (OpenAI, Claude, Gemini,   │
│   Ollama, Azure OpenAI)     │
│                             │
│  Receives:                  │
│  - Schema (tables, columns) │
│  - User Query               │
│  - Context & History        │
│                             │
│  Returns:                   │
│  - Generated SQL            │
│  - Reasoning Steps          │
└─────────────────────────────┘
       │
       │ Validated SQL
       ▼
┌──────────────┐
│   Your DB    │
│ (PostgreSQL, │
│   MySQL...)  │
│              │
│ Execute SQL  │
│ Return Data  │
└──────────────┘
```

## Workflow: From Question to Answer

### Step 1: User Input
User types a natural language question:
```
"Show me the top 10 customers by revenue this month"
```

### Step 2: Schema Retrieval
AstraSQL retrieves the database schema:
- Table names
- Column names and types
- Relationships (foreign keys)
- Indexes
- Constraints

**Example Schema:**
```json
{
  "tables": [
    {
      "name": "customers",
      "columns": [
        {"name": "id", "type": "integer"},
        {"name": "name", "type": "varchar"},
        {"name": "email", "type": "varchar"}
      ]
    },
    {
      "name": "orders",
      "columns": [
        {"name": "id", "type": "integer"},
        {"name": "customer_id", "type": "integer"},
        {"name": "total", "type": "decimal"},
        {"name": "created_at", "type": "timestamp"}
      ],
      "foreign_keys": [
        {"column": "customer_id", "references": "customers.id"}
      ]
    }
  ]
}
```

### Step 3: LangGraph Agent Processing
The FastAPI backend invokes the **LangGraph agent**, which orchestrates the query generation process:

**Agent State (LangGraph):**
```python
{
  "messages": [
    {"role": "user", "content": "Show me the top 10 customers by revenue this month"}
  ],
  "schema": { /* schema from step 2 */ },
  "database_type": "postgresql",
  "user_permissions": ["customers", "orders"],
  "query_history": [],
  "current_step": "analyze_query"
}
```

**Agent Workflow (LangGraph Nodes):**
1. **Analyze Query** - Understand user intent
2. **Retrieve Schema** - Get relevant table/column info
3. **Generate SQL** - Use LangChain SQL agent tools
4. **Validate SQL** - Check syntax and permissions
5. **Refine if Needed** - Handle errors and retry

### Step 4: LangChain SQL Agent
The LangGraph agent uses **LangChain SQL tools** to interact with the LLM:

**LangChain Tool Invocation:**
```python
# LangChain SQL Database Agent
agent = create_sql_agent(
    llm=llm,  # OpenAI, Claude, Gemini, etc.
    db=sql_database,  # Schema only, no data
    tools=[sql_query_tool, schema_tool],
    verbose=True
)
```

**Only the schema is sent to the LLM**, along with the user's question:

```json
{
  "schema": { /* schema from step 2 */ },
  "query": "Show me the top 10 customers by revenue this month",
  "context": {
    "database_type": "postgresql",
    "user_permissions": ["customers", "orders"]
  }
}
```

**The LLM never sees:**
- Actual row data
- Customer names
- Email addresses
- Financial amounts
- Any sensitive information

### Step 5: SQL Generation
The LangChain agent generates SQL through the LLM:

```sql
SELECT 
  c.name,
  SUM(o.total) AS revenue
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.created_at >= DATE_TRUNC('month', CURRENT_DATE)
GROUP BY c.id, c.name
ORDER BY revenue DESC
LIMIT 10;
```

### Step 6: Validation & Execution
The LangGraph agent validates and executes:

**Validation (LangChain Tools):**
- Syntax checking via SQL parser
- Permission verification (RBAC)
- Safety checks (read-only enforcement)
- Query explanation generation

**Execution:**
If validation passes, the query executes against your database:
- Results are returned directly to FastAPI
- No data is stored by AstraSQL
- Results are cached (optional) in memory
- Query is logged for audit

### Step 7: Response & Presentation
FastAPI returns the results to the frontend:
- JSON response with query results
- Generated SQL query
- Execution metadata
- Error messages (if any)

**Frontend displays:**
- As a data table
- As visualizations (charts, graphs)
- Exportable formats (CSV, Excel, PDF)

## Privacy-First Design

### Metadata-Only Processing

**What is Metadata?**
- Table structures
- Column names and types
- Relationships
- Indexes
- Constraints

**What is NOT Metadata?**
- Row data
- Actual values
- Sensitive information
- Query results

### Privacy Guarantees

1. **No Data Transmission**: Your actual data never leaves your database
2. **Schema Only**: Only structural information is sent to AI
3. **Local Execution**: Queries run on your infrastructure
4. **Optional Local AI**: Use Ollama for completely local processing
5. **Audit Logs**: Track all queries without storing data

## Security Features

### Access Control
- Role-based permissions
- Per-user table access
- Row-level security (coming soon)
- IP whitelisting

### Query Safety
- Read-only enforcement
- SQL injection prevention
- Query validation
- Rate limiting

### Compliance
- Audit logging
- Data retention policies
- GDPR compliance
- SOC 2 ready

## Agent Architecture: LangGraph + LangChain

### How LangGraph Orchestrates the Process

**LangGraph** provides the orchestration layer that manages the multi-step reasoning process:

```python
# Simplified LangGraph workflow
from langgraph.graph import StateGraph, END

# Define agent state
class AgentState(TypedDict):
    messages: List[Message]
    schema: Dict
    sql_query: Optional[str]
    query_result: Optional[Dict]
    errors: List[str]
    current_step: str

# Create state graph
workflow = StateGraph(AgentState)

# Add nodes (steps in the workflow)
workflow.add_node("analyze_query", analyze_query_node)
workflow.add_node("retrieve_schema", retrieve_schema_node)
workflow.add_node("generate_sql", generate_sql_node)
workflow.add_node("validate_sql", validate_sql_node)
workflow.add_node("execute_query", execute_query_node)

# Define edges (flow control)
workflow.set_entry_point("analyze_query")
workflow.add_edge("analyze_query", "retrieve_schema")
workflow.add_edge("retrieve_schema", "generate_sql")
workflow.add_conditional_edges(
    "validate_sql",
    should_retry,  # Conditional logic
    {
        "retry": "generate_sql",
        "execute": "execute_query",
        "error": END
    }
)
workflow.add_edge("execute_query", END)

# Compile the graph
agent = workflow.compile()
```

### How LangChain Provides the Tools

**LangChain** provides the actual LLM interaction and SQL generation capabilities:

```python
from langchain.agents import create_sql_agent
from langchain.agents.agent_toolkits import SQLDatabaseToolkit
from langchain.sql_database import SQLDatabase

# Create SQL database connection (schema only)
db = SQLDatabase.from_uri(
    connection_string,
    include_tables=["customers", "orders"],  # Only metadata
    sample_rows_in_table_info=0  # No actual data
)

# Create SQL agent with LangChain
agent = create_sql_agent(
    llm=llm,  # LangChain LLM wrapper
    db=db,
    toolkit=SQLDatabaseToolkit(db=db, llm=llm),
    verbose=True,
    agent_type="openai-tools"  # or "openai-functions"
)
```

### Integration: LangGraph + LangChain

The LangGraph agent orchestrates the workflow, while LangChain tools handle the actual LLM interactions:

1. **LangGraph** manages:
   - State transitions
   - Error recovery
   - Multi-step reasoning
   - Conditional logic

2. **LangChain** handles:
   - LLM API calls
   - SQL generation
   - Tool execution
   - Response parsing

## Technical Stack

### Backend Framework
- **Python FastAPI** - High-performance async API framework
- **LangChain** - LLM application framework and tooling
- **LangGraph** - Agent orchestration and state management
- **SQLAlchemy** - Database abstraction layer
- **Pydantic** - Data validation and type safety

### Agent Architecture
- **LangGraph StateGraph** - Manages agent state and workflow
- **LangChain SQL Agent** - Specialized SQL generation agent
- **LangChain Tools** - Schema retrieval, SQL validation, query execution
- **Memory Management** - Conversation history and context via LangChain memory
- **Error Recovery** - LangGraph conditional edges for retry logic

### Performance Optimization

### Caching
- Query result caching (in-memory)
- Schema caching (Redis/Memory)
- LLM response caching (LangChain cache)
- Agent state caching

### Query Optimization
- Index recommendations
- Execution plan analysis
- Performance monitoring
- Query result streaming

### Scalability
- Async FastAPI endpoints
- Horizontal scaling with multiple workers
- Load balancing
- Database connection pooling (SQLAlchemy)
- Agent instance pooling

## Supported Databases

- **PostgreSQL** - Full support
- **MySQL** - Full support
- **SQLite** - Full support
- **SQL Server** - Full support
- **Oracle** - Full support
- **MongoDB** - Read-only queries

## LLM Providers (via LangChain)

All LLM providers are accessed through LangChain's unified interface:

- **OpenAI** - GPT-4, GPT-3.5, GPT-4 Turbo
- **Anthropic** - Claude 3 (Opus, Sonnet, Haiku)
- **Google** - Gemini Pro, Gemini Ultra
- **Azure OpenAI** - Enterprise deployments
- **Ollama** - Local models (completely private, via LangChain Ollama integration)

**LangChain Benefits:**
- Unified API across all providers
- Easy provider switching
- Automatic retry and error handling
- Streaming support
- Token usage tracking

## Next Steps

- [Database Connections](./database-connections.md)
- [Query Generation](./query-generation.md)
- [Security Model](./security-model.md)

---

**Related**: [Feature Roadmap](../../FEATURE_ROADMAP.md)

