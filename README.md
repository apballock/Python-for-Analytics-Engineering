# Python for Analytics Engineering

**Ana Ballock** · [LinkedIn](https://linkedin.com/in/anaballock) · [GitHub](https://github.com/apballock)

---

Applied Python project covering data structure manipulation, business rule automation, validation systems, and reusable analytical workflows — built around scenarios from the Financial Sample dataset to mirror real analytics engineering tasks.

---

## Skills Demonstrated

Python for analytics · Data structure manipulation · Business logic automation · Input validation · Error handling · Functional programming · Reporting automation · Modular code design

---

## Project Structure

```text
├── Level 1 – Data Structures
│   ├── Lists
│   ├── Tuples
│   ├── Sets
│   └── Dictionaries
├── Level 2 – Control Flow
│   ├── Conditional Statements
│   ├── Loops & List Comprehension
│   ├── While Loops & Validation
│   └── Error Handling
└── Level 3 – Functions
    ├── Core Functions & KPI Calculations
    ├── Functions with Data Structures
    └── Currency Converter Mini-Project
```

---

## Level 1 — Data Structures

Built familiarity with Python's core data structures by mapping each to its natural business use case, then manipulating real product and sales data from the Financial Sample dataset.

| Structure | Analytics Use Case |
|---|---|
| List | Ordered sequences — ranked products, transaction history, monthly KPIs |
| Tuple | Immutable reference values — currency codes, geographic coordinates, config |
| Set | Deduplication and overlap analysis — unique customers, segment comparison |
| Dictionary | Structured records — JSON-like product rows, API response parsing |

**Lists** were used to manage and sort product collections dynamically, practicing slicing, membership testing, and in-place modification — operations that appear constantly in pre-processing workflows.

**Tuples** enforced immutability on stable reference data. When modification was required, the tuple-to-list-to-tuple conversion pattern was applied — a common pattern in ETL preprocessing where configuration values need occasional updates without exposing them to free mutation.

**Sets** performed segment deduplication and intersection analysis. The operation `profitable & high_volume` identified segments meeting both criteria simultaneously, which directly mirrors audience overlap analysis in marketing and sales analytics.

**Dictionaries** modeled structured product records with key-value pairs, and lists of dictionaries were used to simulate tabular datasets — a structure nearly identical to what `pandas` uses internally, and what most API responses return in production.

---

## Level 2 — Control Flow

Implemented business logic programmatically across four control flow patterns, each grounded in a realistic analytical scenario.

**Conditional statements** powered a profitability classification system using tiered `if/elif/else` logic — structurally equivalent to the `IIF` and `SWITCH` functions used in DAX, and the `CASE WHEN` pattern in SQL. Rule ordering was treated as a deliberate design decision, not an afterthought: conditions evaluated sequentially means priority must be assigned intentionally.

**Loops and list comprehension** automated bulk calculations across product and sales collections. List comprehensions were used for concise filtering:

```python
high_performers = [p for p, s in sales.items() if s > 3000]
```

This pattern is functionally equivalent to a filtered column in Power Query or a `WHERE` clause in SQL — and significantly faster to write than equivalent loop constructions.

**While loops with validation** built an input validation layer that rejected text values, negative numbers, and out-of-range percentages before allowing data to progress through the workflow. Defensive input handling at this layer prevents silent data quality failures downstream — a principle that applies equally to ETL pipelines and user-facing dashboards.

**Error handling** implemented structured `try/except/else/finally` blocks to manage file loading failures, missing data, and invalid inputs without crashing execution. In production analytics pipelines, unhandled exceptions that silently interrupt processing are more dangerous than known errors — graceful failure with clear messaging is always preferable.

```python
try:
    df = pd.read_csv("financial_sample.csv")
except FileNotFoundError:
    print("Source file unavailable — check pipeline inputs")
finally:
    log_execution_status()
```

---

## Level 3 — Functions

Refactored all prior logic into reusable, parameterized functions — moving from scripts that solve one problem to components that can be composed into larger workflows.

**Core functions** encapsulated KPI calculations (profit margin, revenue summary, segment classification) with division-by-zero protection and multi-value returns. A single `summarize_performance()` call returns min, max, and average simultaneously — reducing redundant computation and keeping analytical code readable.

**Functions with data structures** combined the prior levels into a lightweight analytics engine: filtering segments, aggregating totals, ranking products, and generating a formatted performance report — all through composable function calls. The `generate_report()` function demonstrated how modular design enables a full analytical workflow to be triggered from a single entry point, which is the architectural pattern behind most ETL orchestration tools.

**Currency Converter mini-project** applied production-grade design principles to a realistic financial transformation scenario:

```python
def convert_currency(amount: float, rate: float) -> float:
```

Type hints were used throughout — improving readability, enabling IDE autocomplete, and making function contracts explicit. Responsibilities were separated cleanly: one function handles the math, another manages exchange rate logic and exception routing. Invalid currency pairs raise `KeyError`; negative values raise `ValueError` — both caught and handled explicitly rather than allowed to propagate silently.

This architecture mirrors how currency normalization is handled in international sales reporting pipelines, where upstream data arrives in mixed currencies and must be standardized before reaching any BI layer.

---

## Key Takeaways

- Python data structures mapped directly to analytics engineering use cases — lists as sequences, dicts as records, sets for deduplication, tuples for immutable config
- Control flow logic implemented as business rules, not syntax exercises — classification systems, validation gates, bulk transformations
- Error handling designed for pipeline reliability, not just crash prevention
- Functions built for composability and reuse, following single-responsibility design
- Mini-project demonstrated production patterns — type hints, input validation, modular architecture — applied to a real financial transformation workflow
- Full progression from raw data manipulation to automated reporting, establishing the foundation for pandas, SQL integration, and ETL development
