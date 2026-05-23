# Data Flow & ETL Pipeline

Because workforce databases contain sensitive details (compensation rates, employee IDs, genders, and statuses), the HR Ecosystem Portal utilizes a zero-storage data policy. All cleaning, transformation, and visualizations occur strictly within the client browser's sandbox.

## ETL Process sequence

```mermaid
sequenceDiagram
    actor HR as HR User
    participant App as AppController (Frontend)
    participant Parser as MiniCSV / MiniXLSX
    participant UI as Dashboard & Tables
    participant Backend as GAS askChatbot
    participant Gemini as Gemini API

    HR->>App: 1. Drag/Select Dataset (CSV / XLSX)
    App->>Parser: 2. Parse Raw File bytes
    Parser-->>App: 3. Return JSON Array
    
    note over App: 4. Execute ETL Pipeline<br/>- Dynamic header resolution (Regex)<br/>- Currency cleansing (Remove $, commas)<br/>- Standardize statuses (Active/Terminated)<br/>- Calculate KPIs & Distributions
    
    App->>UI: 5. Render KPIs, Data Grid & Canvas Charts
    
    note over App: 6. Build Context Vector<br/>(Aggregated summary string of non-PII metrics)
    
    HR->>App: 7. Submits Chat Query / Generates Brief
    App->>Backend: 8. Call with Query + Context Vector
    Backend->>Gemini: 9. Prompt Engineering payload + Key
    Gemini-->>Backend: 10. Analytical Markdown Response
    Backend-->>App: 11. Success Callback
    App->>UI: 12. Parse & Render in Markdown View
```

## Security & Data Integrity

1. **Extract**: The user selects a document. The client-side parser reads file content in-memory as an ArrayBuffer or Text stream.
2. **Transform**: The `AppController` runs mapping logic:
   - Matches keys like "pay", "wage", or "compensation" to standardized `salary` values.
   - Cleanses string text like `"$120,000.50"` into numeric floats (`120000.5`).
   - Normalizes statuses (e.g. "Terminated", "Resigned", "Inactive" map to `"Terminated"`).
   - Filters out corrupt or blank dataset rows.
3. **Load**: Data is saved to local runtime state `_dataset`.
4. **Aggregation**: Key indicators are synthesized:
   - Attrition rates by dividing terminated headcount by historical total headcount.
   - Gender pay gap benchmarks.
5. **Context Vector Generation**: Aggregated stats are converted to a text summary vector. Raw employee names, IDs, or exact individual records are never appended, ensuring absolute privacy.
6. **AI Dispatch**: Only the summary vector is sent to the backend proxy.
