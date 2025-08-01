# feastdays 📅

A Python package for querying Catholic feast days celebrated in Opus Dei. This package provides easy access to feast day information including dates, descriptions, liturgical details, and comprehensive search capabilities.

## Features

- 🔍 Search feast days by date, title, tag, or liturgical type
- 📅 Get feast days for specific dates or today
- 🏷️ Rich metadata including liturgical colors, types, and classes
- 🔎 Case-sensitive and case-insensitive search options
- 📊 Statistical functions for calendar analysis
- 🎯 Type-safe API with full type hints
- 📚 Comprehensive docstrings and examples

## Installation

Install using pip:

```bash
pip install feastdays
```

## Quick Start

```python
from feastdays import get_feast_for_date, search_feasts_by_title, get_feast_for_today
from datetime import date

# Get feast days for a specific date (multiple formats supported)
feasts = get_feast_for_date("03-19")  # March 19 - St. Joseph
feasts = get_feast_for_date("Jan 9")  # January 9
feasts = get_feast_for_date(date(2024, 1, 9))  # Using date object
feasts = get_feast_for_date("9th January 2024")  # With year
for feast in feasts:
    print(f"{feast.title} - {feast.type}")

# Search by title
mary_feasts = search_feasts_by_title("mary")
print(f"Found {len(mary_feasts)} feasts related to Mary")

# Get today's feast days
today_feasts = get_feast_for_today()
if today_feasts:
    print(f"Today's feast: {today_feasts[0].title}")
```

## API Reference

### Core Functions

#### `get_feast_for_date(date_input)`

Get all feasts for a specific date. Supports multiple date formats:

**Parameters:**
- `date_input` (Union[str, date, datetime]): Date in various formats

**Supported Date Formats:**
- `datetime.date` or `datetime.datetime` objects
- `"MM-DD"` format: `"01-09"`, `"12-25"`
- Month name formats: `"Jan 9"`, `"January 9"`, `"January 9th"`
- Day-first formats: `"9 Jan"`, `"9 January"`, `"9th January"`
- With year: `"Jan 9 2024"`, `"9th January 2024"`, `"January 1st 2024"`

**Returns:** List[Feast]

**Examples:**
```python
from datetime import date, datetime
from feastdays import get_feast_for_date

# Using date objects
feasts = get_feast_for_date(date(2024, 1, 9))
feasts = get_feast_for_date(datetime.now())

# Using MM-DD format
feasts = get_feast_for_date("01-09")
feasts = get_feast_for_date("12-25")

# Using month name formats
feasts = get_feast_for_date("Jan 9")
feasts = get_feast_for_date("January 9")
feasts = get_feast_for_date("January 9th")

# Using day-first formats
feasts = get_feast_for_date("9 Jan")
feasts = get_feast_for_date("9 January")
feasts = get_feast_for_date("9th January")

# With year included
feasts = get_feast_for_date("Jan 9 2024")
feasts = get_feast_for_date("9th January 2024")
```

#### `get_feast_for_today()`

Get all feasts for today's date.

**Returns:** List[Feast]

**Example:**
```python
from feastdays import get_feast_for_today

feasts = get_feast_for_today()
for feast in feasts:
    print(f"{feast.title} ({feast.type})")
```

#### `search_feasts_by_title(keyword, case_sensitive=False)`

Search for feasts by title keyword.

**Parameters:**
- `keyword` (str): Search term to look for in feast titles
- `case_sensitive` (bool): Whether to perform case-sensitive search (default: False)

**Returns:** List[Feast]

**Example:**
```python
from feastdays import search_feasts_by_title

# Case-insensitive search
mary_feasts = search_feasts_by_title("mary")
print(f"Found {len(mary_feasts)} feasts related to Mary")

# Case-sensitive search
exact_feasts = search_feasts_by_title("Mary", case_sensitive=True)
```

#### `search_feasts_by_tag(tag, case_sensitive=False)`

Search for feasts by tag.

**Parameters:**
- `tag` (str): Tag to search for
- `case_sensitive` (bool): Whether to perform case-sensitive search (default: False)

**Returns:** List[Feast]

**Example:**
```python
from feastdays import search_feasts_by_tag

# Find all feasts of Evangelists
evangelist_feasts = search_feasts_by_tag("evangelist")
print(f"Found {len(evangelist_feasts)} feasts related to Evangelists")

# Find all Opus Dei specific celebrations
opus_dei_feasts = search_feasts_by_tag("opus dei")
for feast in opus_dei_feasts:
    print(f"{feast.date}: {feast.title}")
```

#### `search_feasts_by_type(feast_type, case_sensitive=False)`

Search for feasts by liturgical type.

**Parameters:**
- `feast_type` (str): Type to search for (e.g., "Solemnity", "Memorial", "Feast")
- `case_sensitive` (bool): Whether to perform case-sensitive search (default: False)

**Returns:** List[Feast]

**Example:**
```python
from feastdays import search_feasts_by_type

# Find all Solemnities
solemnities = search_feasts_by_type("Solemnity")

# Find all Memorials (case-insensitive)
memorials = search_feasts_by_type("memorial")
```

### Utility Functions

#### `list_all_feasts()`

Get all feasts in the calendar in chronological order.

**Returns:** List[Feast]

**Example:**
```python
from feastdays import list_all_feasts

all_feasts = list_all_feasts()
print(f"Total feasts in calendar: {len(all_feasts)}")
```

#### `get_dates_with_feasts()`

Get all dates that have feast days.

**Returns:** List[str] (MM-DD format)

**Example:**
```python
from feastdays import get_dates_with_feasts

feast_dates = get_dates_with_feasts()
print(f"Calendar has feasts on {len(feast_dates)} days")
```

#### `get_feast_count()`

Get total number of individual feasts in the calendar.

**Returns:** int

**Example:**
```python
from feastdays import get_feast_count

total_feasts = get_feast_count()
print(f"Total feast entries: {total_feasts}")
```

## Data Model

### `Feast` Class

Represents a Catholic feast day with the following attributes:

```python
@dataclass
class Feast:
    date: str          # Date string (e.g., "January 9")
    title: str         # Feast title
    description: str   # Detailed description
    color: str         # Liturgical color (White, Red, etc.)
    type: str          # Type (Solemnity, Memorial, etc.)
    class_: str        # Classification (A, B, C, D, E)
    tags: List[str]    # Associated tags
```

**Methods:**
- `to_dict()`: Convert to dictionary
- `from_dict(data)`: Create from dictionary (class method)

**Example:**
```python
feast = get_feast_for_date("Jan 9")[0]
print(f"Title: {feast.title}")
print(f"Type: {feast.type}")
print(f"Color: {feast.color}")
print(f"Class: {feast.class_}")
print(f"Tags: {', '.join(feast.tags)}")
```

## Advanced Examples

### Search Combinations

```python
from feastdays import search_feasts_by_tag, search_feasts_by_type

# Find all Marian solemnities
marian_feasts = search_feasts_by_tag("mary")
solemnities = search_feasts_by_type("Solemnity")
marian_solemnities = [f for f in marian_feasts if f in solemnities]

# Get all Opus Dei specific celebrations
opus_dei_feasts = search_feasts_by_tag("opus dei")
for feast in opus_dei_feasts:
    print(f"{feast.date}: {feast.title}")
```

### Calendar Statistics

```python
from feastdays import get_feast_count, get_dates_with_feasts, list_all_feasts

# Basic statistics
total_feasts = get_feast_count()
total_dates = len(get_dates_with_feasts())
print(f"Total feasts: {total_feasts}")
print(f"Days with feasts: {total_dates}")

# Feast type distribution
all_feasts = list_all_feasts()
types = {}
for feast in all_feasts:
    feast_type = feast.type or "Unspecified"
    types[feast_type] = types.get(feast_type, 0) + 1

for feast_type, count in sorted(types.items()):
    print(f"{feast_type}: {count}")
```

## Error Handling

The library raises specific exceptions for different error conditions:

```python
from feastdays import get_feast_for_date

try:
    feasts = get_feast_for_date("invalid date")
except ValueError as e:
    print(f"Date parsing error: {e}")

try:
    feasts = get_feast_for_date("Jan 9")
except FileNotFoundError as e:
    print(f"Data file not found: {e}")
except json.JSONDecodeError as e:
    print(f"Invalid JSON data: {e}")
```

## Date Format Reference

| Format Type | Examples |
|-------------|----------|
| Date Objects | `date(2024, 1, 9)`, `datetime.now()` |
| MM-DD | `"01-09"`, `"12-25"` |
| Month Day | `"Jan 9"`, `"January 9"`, `"January 9th"` |
| Day Month | `"9 Jan"`, `"9 January"`, `"9th January"` |
| With Year | `"Jan 9 2024"`, `"9th January 2024"`, `"January 1st 2024"` |

**Note:** The library automatically handles ordinal suffixes (st, nd, rd, th) and is case-insensitive for month names.

## Data Source

The feast day data is maintained in `feastdays/data/feast_days.json` and includes:

- Universal Catholic feast days
- Opus Dei specific celebrations
- Saints particularly venerated in Opus Dei
- Liturgical seasons and special observances

## License

MIT License - see LICENSE file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
