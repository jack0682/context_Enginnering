# 02. Interfaces

> **Purpose**: This document defines the interfaces (APIs, schemas, data contracts) used in this project. Update this document BEFORE implementing interface changes.

---

## 1. How to Use This Document

### 1.1 When to Update
- **BEFORE** adding a new public function, class, or API endpoint
- **BEFORE** changing the signature of an existing interface
- **BEFORE** deprecating or removing an interface

### 1.2 Format
Each interface section should include:
- Name and location
- Input/output types
- Example usage
- Version information

---

## 2. Internal APIs

### 2.1 [API: Example Module API]

**Location**: `src/example/api.py`

**Version**: 1.0.0

| Function | Input | Output | Description |
|----------|-------|--------|-------------|
| `process_data(data: Dict)` | `{"key": "value", ...}` | `Result` object | Processes input data |
| `validate_input(input: str)` | String to validate | `bool` | Returns True if valid |

**Example**:
```python
from example.api import process_data

result = process_data({"key": "value"})
print(result.status)  # "success"
```

**Change History**:
| Version | Date | Change |
|---------|------|--------|
| 1.0.0 | 2026-01-02 | Initial |

---

## 3. External APIs

### 3.1 [API: REST Endpoints]

**Base URL**: `https://api.example.com/v1`

**Authentication**: Bearer token in `Authorization` header

| Endpoint | Method | Request Body | Response | Description |
|----------|--------|--------------|----------|-------------|
| `/items` | GET | — | `[Item, ...]` | List all items |
| `/items/{id}` | GET | — | `Item` | Get single item |
| `/items` | POST | `ItemCreate` | `Item` | Create item |

**Example**:
```bash
curl -X GET https://api.example.com/v1/items \
  -H "Authorization: Bearer ${TOKEN}"
```

**Response Schema**:
```json
{
  "id": "string",
  "name": "string",
  "created_at": "ISO8601 timestamp"
}
```

---

## 4. Data Schemas

### 4.1 [Schema: Item]

**Used by**: REST API, Internal processing

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string (UUID) | Yes | Unique identifier |
| `name` | string | Yes | Item name (max 255 chars) |
| `description` | string | No | Optional description |
| `created_at` | ISO8601 | Yes | Creation timestamp |
| `updated_at` | ISO8601 | No | Last update timestamp |
| `metadata` | object | No | Arbitrary key-value pairs |

**Example**:
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Example Item",
  "description": "This is an example",
  "created_at": "2026-01-02T12:00:00+09:00",
  "metadata": {"priority": "high"}
}
```

### 4.2 [Schema: ItemCreate]

**Used by**: POST /items

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Item name |
| `description` | string | No | Optional description |
| `metadata` | object | No | Arbitrary key-value pairs |

**Example**:
```json
{
  "name": "New Item",
  "description": "A new item"
}
```

---

## 5. Message Formats

### 5.1 [Message: Event]

**Used by**: Event bus, logging

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `event_id` | string (UUID) | Yes | Unique event ID |
| `event_type` | string | Yes | Type of event |
| `timestamp` | ISO8601 | Yes | When event occurred |
| `payload` | object | Yes | Event-specific data |
| `source` | string | Yes | Origin module/service |

**Example**:
```json
{
  "event_id": "evt-12345",
  "event_type": "item.created",
  "timestamp": "2026-01-02T12:00:00+09:00",
  "payload": {"item_id": "550e8400-..."},
  "source": "items-service"
}
```

---

## 6. File Formats

### 6.1 [Format: Configuration YAML]

**Location**: `config/*.yaml`

```yaml
# config/app.yaml
app:
  name: "MyApp"
  version: "1.0.0"
  
database:
  host: "${DB_HOST}"    # Environment variable
  port: 5432
  name: "mydb"
  
logging:
  level: "INFO"
  format: "json"
```

**Required Fields**:
- `app.name`: Application name
- `app.version`: Semantic version

---

## 7. Interface Change Checklist

When changing any interface:

- [ ] Updated this document (`02_interfaces.md`)
- [ ] Added version bump if breaking change
- [ ] Included example usage
- [ ] Updated dependent code
- [ ] Updated tests
- [ ] Notified downstream consumers (if external)

---

## 8. Template for New Interfaces

Use this template when adding new interfaces:

```markdown
### X.X [Type: Name]

**Location**: `path/to/file`

**Version**: 1.0.0

| Input/Field | Type | Required | Description |
|-------------|------|----------|-------------|
| ... | ... | ... | ... |

**Example**:
[code block with example]

**Change History**:
| Version | Date | Change |
|---------|------|--------|
| 1.0.0 | YYYY-MM-DD | Initial |
```

---

**END OF DOCUMENT**
