<!-- _class: invert -->
# How Salesforce Migration Tool is Built

## A Deep Dive into Architecture & Design

**Version 1.0.0**

* Mohan Chinnappan*

---

## What is it?

A sophisticated **Node.js-based CLI tool** for seamless Salesforce data migration

### Core Capabilities
- 🎯 **Smart Field Mapping** - Automatic lookup resolution
- ✅ **Pre-Migration Validation** - Ensures data integrity
- 🔄 **Composite Key Matching** - Intelligent record identification
- 📊 **Bulk API Integration** - Efficient large-scale operations

---

## Architecture Overview

### Modular Design with Clear Separation of Concerns

```
┌─────────────────────────────────────────────┐
│            CLI Interface                    │
│   Command parsing, user interaction         │
└─────────────────┬───────────────────────────┘
                  │
┌─────────────────▼───────────────────────────┐
│        Migration Logic                      │
│   Workflows, validation, processing         │
└─────────────────┬───────────────────────────┘
                  │
┌─────────────────▼───────────────────────────┐
│    Salesforce Integration                   │
│   Auth, queries, Bulk API, metadata         │
└─────────────────────────────────────────────┘
```

---

## Module 1: CLI Interface 

### Responsibilities
- Command parsing using **Commander.js**
- Interactive prompts with **Inquirer.js**
- User interaction orchestration
- Command routing and validation

### Key Commands
```bash
migrate auth --source username@example.com
migrate load-plan --file plan.json
migrate validate
migrate migrate
migrate generate-csv
migrate export
```

---

## Module 2: Migration Logic 

### Core Workflows

1. **Validation Pipeline**
   - Object existence verification
   - Field accessibility checks
   - External ID configuration validation

2. **Data Processing**
   - Field mapping execution
   - Lookup resolution
   - Composite key matching

3. **Output Generation**
   - CSV file creation for insert/update
   - Failed record logging

---

## Module 3: Salesforce Integration 

### API Operations

| Operation | API Used | Purpose |
|-----------|----------|---------|
| Authentication | SF CLI | OAuth token retrieval |
| Queries | REST API | SOQL data extraction |
| Metadata | Describe API | Field information |
| Data Loading | Bulk API v2 | Large-scale uploads |

### Performance Features
- Metadata caching
- Query result pagination
- Job status monitoring

---

## Module 4: Utilities 

### Helper Functions

- **Logging**: Colored console output with severity levels
- **Data Extraction**: Nested field value retrieval
- **Lookup Building**: Map construction for relationship resolution
- **Field Resolution**: Dynamic field path parsing
- **CSV Generation**: Streaming CSV creation
- **ZIP Creation**: Archive bundling with JSZip

---

## Module 5: Configuration 

### Persistent Storage

Manages:
- Connection details (tokens, instance URLs)
- Load plan definitions
- User selections (selected objects)
- Migration state

```javascript
{
  "sourceOrg": { /* connection info */ },
  "targetOrg": { /* connection info */ },
  "loadPlan": [ /* migration objects */ ],
  "selectedObjects": [ /* user choices */ ]
}
```

---

## Migration Flow: Step-by-Step

### Phase 1: Setup
1. **Authenticate** source and target orgs
2. **Load** migration plan (JSON)
3. **Select** objects to migrate
4. **Validate** configuration

---

## Migration Flow: Step-by-Step

### Phase 2: Data Extraction
5. **Query** source org using SOQL
6. **Build** composite key map from source data
7. **Build** lookup maps from target org
8. **Resolve** field mappings (lookups, composite lookups)

---

## Migration Flow: Step-by-Step

### Phase 3: Reconciliation
9. **Match** records by composite keys
10. **Separate** into insert vs update sets
11. **Detect** field differences for updates
12. **Filter** based on updateable field metadata

---

## Migration Flow: Step-by-Step

### Phase 4: Execution
13. **Generate** insert CSV files
14. **Generate** update CSV files
15. **Upload** via Bulk API v2 
16. **Monitor** job status and handle errors

---

<!-- _class: invert -->
## Load Plan Structure

### JSON-Based Configuration

Defines:
- Objects to migrate
- SOQL queries
- Composite keys
- Field mappings (simple, lookup, composite)
- Multi-step migrations

---

## Load Plan: Simple Object

```json
{
  "object": "Product2",
  "compositeKeys": ["StockKeepingUnit"],
  "query": "SELECT StockKeepingUnit, Name, ProductCode FROM Product2",
  "fieldMappings": {
    "StockKeepingUnit": "StockKeepingUnit",
    "Name": "Name",
    "ProductCode": "ProductCode"
  }
}
```

### Simple field-to-field mapping

---

## Load Plan: Lookup Fields

```json
{
  "object": "ProductCategory",
  "compositeKeys": ["Code"],
  "query": "SELECT Name, Catalog.Code, Code FROM ProductCategory",
  "fieldMappings": {
    "Name": "Name",
    "Code": "Code",
    "CatalogId": {
      "lookup": {
        "object": "ProductCatalog",
        "key": "Code",
        "field": "Catalog.Code"
      }
    }
  }
}
```

### Resolves relationship IDs automatically

---

## Load Plan: Composite Lookups

```json
{
  "ProductClassificationAttributeId": {
    "compositeLookup": {
      "object": "ProductClassificationAttr",
      "keyFields": [
        "AttributeCategory.Code",
        "AttributeDefinition.Code",
        "ProductClassification.Code"
      ],
      "sourceFieldMap": {
        "AttributeCategory.Code": "ProductClassificationAttribute.AttributeCategory.Code",
        "AttributeDefinition.Code": "ProductClassificationAttribute.AttributeDefinition.Code",
        "ProductClassification.Code": "ProductClassificationAttribute.ProductClassification.Code"
      }
    }
  }
}
```

---

## Load Plan: Multi-Step Migration

For hierarchical objects (e.g., parent-child):

```json
{
  "object": "ProductCategory",
  "steps": [
    {
      "query": "SELECT Name, Code FROM ProductCategory WHERE ParentCategoryId = null",
      "fieldMappings": { "Name": "Name", "Code": "Code" }
    },
    {
      "query": "SELECT Name, Code, ParentCategory.Code FROM ProductCategory WHERE ParentCategoryId != null",
      "fieldMappings": {
        "Name": "Name",
        "Code": "Code",
        "ParentCategoryId": { "lookup": { "object": "ProductCategory", "key": "Code", "field": "ParentCategory.Code" } }
      }
    }
  ]
}
```

---

<!-- _class: invert -->
## Key Features Deep Dive

---

## Feature 1: Composite Key Support

### Problem
Records don't have consistent IDs across orgs

### Solution
Use business-meaningful field combinations

```javascript
compositeKeys: ["Email", "Department"]
```

### How it Works
1. Build map of existing records in target org
2. Generate composite key from specified fields
3. Match source records to target records
4. Determine insert vs update operations

---

## Feature 2: Lookup Resolution

### Challenge
Relationship IDs differ between orgs

### Solution
Build lookup maps and translate IDs

```javascript
// Build map: Code → Id
const lookupMap = {
  "PROD001": "a0X1234567890ABC",
  "PROD002": "a0X1234567890DEF"
}

// Resolve source value to target ID
sourceValue = "PROD001"
targetId = lookupMap[sourceValue] // "a0X1234567890ABC"
```

---

## Feature 3: Composite Lookups

### Use Case
Relationships based on multiple fields

### Example
```javascript
{
  "keyFields": ["Country", "State", "City"],
  "sourceFieldMap": {
    "Country": "Address.Country",
    "State": "Address.State",
    "City": "Address.City"
  }
}
```

Creates compound key: `"USA|California|San Francisco"`

---

## Feature 4: Smart Difference Detection

### Optimization Strategy
Only update records with actual changes

### Process
1. Compare each field between source and target
2. Skip records with identical values
3. Generate update CSV only for changed records

### Benefits
- Reduces API calls
- Faster execution
- Lower governor limit consumption

---

## Feature 5: Field Type Conversion

### Handles Type Mismatches

```json
{
  "Sequence": {
    "field": "Sequence__c",
    "convertToInt": true
  }
}
```

### Supported Conversions
- String → Integer
- String → Decimal
- Date format transformations

---

## Feature 6: Metadata Caching

### Performance Optimization

```javascript
const describeCache = new Map();

async function describe(object) {
  if (describeCache.has(object)) {
    return describeCache.get(object);
  }
  const metadata = await fetchDescribe(object);
  describeCache.set(object, metadata);
  return metadata;
}
```

### Impact
- Reduces API calls by 90%+
- Faster validation
- Improved execution time

---

## Feature 7: Bulk API Integration

### Why Bulk API v2?

| Feature | REST API | Bulk API v2 |
|---------|----------|-------------|
| Max records per call | 200 | 150 million |
| Async processing | ❌ | ✅ |
| Job monitoring | ❌ | ✅ |
| Failed record details | Limited | Complete |

### Implementation
1. Create bulk job
2. Upload CSV data
3. Monitor job status
4. Retrieve failed records
5. Log errors for review

---

## Feature 8: Export Capabilities

### Comprehensive Data Backup

```
export-{timestamp}.zip
├── source/
│   ├── Product2.csv
│   ├── ProductCategory.csv
│   └── ...
├── target/
│   ├── Product2.csv
│   ├── ProductCategory.csv
│   └── ...
└── summary.json
```

### Use Cases
- Pre-migration backups
- Data analysis
- Audit trails

---

<!-- _class: invert -->
## Technical Stack

---

## Dependencies

### Core Libraries

| Package | Purpose |
|---------|---------|
| **commander** | CLI framework, command parsing |
| **inquirer** | Interactive prompts, user input |
| **node-fetch** | HTTP client for API calls |
| **csv-stringify** | Streaming CSV generation |
| **jszip** | ZIP archive creation |
| **sf CLI** | Salesforce authentication |

---

## API Integration Details

### REST API
- SOQL queries for data extraction
- Pagination handling (2000 records/batch)
- Nested relationship queries
- Query result caching

### Bulk API v2
- Job creation and management
- CSV upload streaming
- Job state monitoring
- Failed record retrieval
- Batch processing

### Describe API
- Object metadata fetching
- Field property inspection
- Picklist value retrieval
- Relationship mapping

---

<!-- _class: invert -->
## Error Handling Strategy

---

## Multi-Layer Error Handling

### 1. Pre-Migration Validation
- Object existence checks
- Field accessibility verification
- External ID validation
- Lookup target verification

### 2. Runtime Error Handling
- API call retry logic
- Line ending normalization
- Type conversion errors
- Missing lookup resolution

### 3. Post-Migration Analysis
- Failed record logging
- Error categorization
- Detailed error messages
- Recovery recommendations

---

## Validation Checks

```javascript
async function validate() {
  const errors = [];
  
  // Check target objects exist
  for (const obj of plan) {
    if (!await objectExists(obj.object)) {
      errors.push(`Object ${obj.object} not found`);
    }
  }
  
  // Check fields are accessible
  for (const [target, source] of Object.entries(obj.fieldMappings)) {
    if (!await fieldExists(obj.object, target)) {
      errors.push(`Field ${target} not found on ${obj.object}`);
    }
  }
  
  return errors;
}
```

---

<!-- _class: invert -->
## Best Practices & Patterns

---

## Migration Best Practices

### 🎯 Planning Phase
1. **Always validate first** - Catch issues before execution
2. **Test with CSV** - Review transformations before loading
3. **Order dependencies** - Migrate parent objects first
4. **Use composite keys** - Choose stable, unique identifiers

### ⚡ Execution Phase
5. **Monitor job results** - Review logs and failed records
6. **Export for backup** - Create safety net before major changes
7. **Incremental approach** - Migrate object by object
8. **Test in sandbox** - Never test in production first

---

## Code Design Patterns

### 1. Strategy Pattern
Different migration strategies for simple vs multi-step

### 2. Builder Pattern
Constructing complex lookup maps and CSV files

### 3. Factory Pattern
Creating field mappers based on type (simple, lookup, composite)

### 4. Observer Pattern
Job status monitoring and progress reporting

---

## Performance Patterns

### Caching Strategy
```javascript
// Metadata caching
const cache = new Map();

// Lookup map building
const lookups = new Map();

// Query result pagination
let done = false;
let nextRecordsUrl = null;
while (!done) {
  const result = await query(nextRecordsUrl || soql);
  records.push(...result.records);
  done = result.done;
  nextRecordsUrl = result.nextRecordsUrl;
}
```

---

## Data Processing Patterns

### Streaming for Large Datasets
```javascript
// Stream CSV generation
const stringifier = stringify({ header: true });
stringifier.pipe(writeStream);

for (const record of records) {
  stringifier.write(processRecord(record));
}

stringifier.end();
```

### Batch Processing
- Process records in chunks
- Avoid memory overflow
- Maintain progress tracking

---

<!-- _class: invert -->
## Real-World Use Cases

---

## Use Case 1: Sandbox Refresh

### Scenario
Refresh sandbox with production-like data

### Approach
1. Export from production
2. Load plan with all custom objects
3. Validate against sandbox
4. Generate CSVs for review
5. Execute migration
6. Verify data integrity

---

## Use Case 2: Org Consolidation

### Scenario
Merge data from multiple orgs into one

### Approach
1. Create comprehensive load plan
2. Define composite keys to prevent duplicates
3. Use lookup resolution for relationships
4. Execute migrations sequentially
5. Validate data consistency
6. Decommission source orgs

---

## Use Case 3: Data Seeding

### Scenario
Populate new org with reference data

### Approach
1. Export reference data from template org
2. Create load plan with dependencies ordered
3. Generate CSVs for new org
4. Load foundation data first (e.g., Products)
5. Load dependent data (e.g., Price Books)
6. Validate completeness

---

## Use Case 4: Partial Migration

### Scenario
Migrate specific objects or subsets

### Approach
1. Define filtered SOQL queries
2. Use interactive object selection
3. Validate subset dependencies
4. Execute targeted migration
5. Review exceptions
6. Iterate as needed

---

<!-- _class: invert -->
## Architecture Benefits

---

## Why This Architecture Works

### ✅ Modularity
- Clear separation of concerns
- Easy to test individual components
- Simple to extend with new features

### ✅ Flexibility
- JSON-based configuration
- Multi-step support
- Pluggable field mappers

### ✅ Robustness
- Comprehensive validation
- Detailed error handling
- Recovery mechanisms

---

## Scalability Considerations

### Handles Large Volumes
- Bulk API for 150M+ records
- Streaming data processing
- Efficient memory usage
- Paginated queries

### Supports Complex Scenarios
- Multi-level lookups
- Composite relationships
- Hierarchical data
- Custom field mappings

---

<!-- _class: invert -->
## Future Enhancements

---

## Potential Improvements

### 🚀 Feature Additions
- Parallel object processing
- Resume failed migrations
- Incremental/delta migrations
- UI dashboard for monitoring

### 🔧 Technical Enhancements
- TypeScript migration
- Plugin architecture
- Advanced scheduling
- Cloud-based execution

### 📊 Analytics
- Migration metrics
- Performance benchmarks
- Cost analysis
- Data quality reports

---

<!-- _class: invert -->
## Summary

---

## Key Takeaways

### Architecture
✓ Modular design with 5 core components
✓ Clear separation between UI, logic, and integration
✓ Extensible and maintainable codebase

### Features
✓ Smart field mapping with lookups
✓ Composite key matching
✓ Bulk API integration
✓ Comprehensive validation

### Benefits
✓ Reliable data migration
✓ Time-saving automation
✓ Error prevention
✓ Production-ready tool

---


*Built with ❤️ for the Salesforce community*

---
