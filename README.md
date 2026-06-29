# Market Microstructure Bars Engine

> **A high-performance engine for generating institutional-grade financial bars from raw tick data.**

Transform millions of raw market ticks into **Time**, **Daily**, **Tick**, **Volume**, and **Dollar Bars** through a single unified API optimized for quantitative research.

---

## Overview

Market microstructure research often starts with one repetitive task: transforming raw tick data into structured bars.

Most implementations solve this using separate scripts for each aggregation type, resulting in duplicated logic, inconsistent behavior, and difficult maintenance.

**Market Microstructure Bars Engine** solves this problem with a single, extensible architecture capable of generating multiple bar types while remaining fast enough for large-scale quantitative datasets.

Designed for researchers, quantitative developers, and algorithmic traders.

---

## Features

* **Unified Factory API**

  * One function generates every supported bar type.

* **Multiple Aggregation Methods**

  * Time Bars
  * Daily Bars
  * Tick Bars
  * Volume Bars
  * Dollar Bars

* **Numba Accelerated**

  * JIT-compiled core loops for processing millions of ticks efficiently.

* **Institutional Data Integrity**

  * Built to preserve chronological ordering and avoid lookahead bias.

* **CME Session Alignment**

  * Daily bars follow actual CME trading sessions instead of calendar days.

* **Scalable Storage**

  * Optional Hive-style partitioned Parquet output for big-data workflows.

* **Modular Architecture**

  * Easy to extend with new bar construction methods.

---

## Architecture

```text
              Raw Tick Data
                    │
                    ▼
          +-------------------+
          |    make_bars()    |
          +-------------------+
                    │
      ┌─────────────┼─────────────┐
      ▼             ▼             ▼
 Time Bars     Tick Bars     Volume Bars
      │             │             │
      └─────────────┼─────────────┘
                    ▼
             Dollar Bars
                    │
                    ▼
              Daily Bars
```

---

## Quick Start

```python
from bars_engine_unified import make_bars

# Dollar Bars
bars = make_bars(
    df,
    bar_type="dollar",
    param=50000
)

# 5-Minute Time Bars
bars = make_bars(
    df,
    bar_type="time",
    param="5min"
)

# Tick Bars
bars = make_bars(
    df,
    bar_type="tick",
    param=1000
)
```

---

## Supported Bar Types

| Bar Type | Parameter Example                     |
| -------- | ------------------------------------- |
| Time     | `"1min"`, `"5min"`, `"15min"`, `"1H"` |
| Daily    | `"daily"`                             |
| Tick     | `1000`                                |
| Volume   | `5000`                                |
| Dollar   | `50000`                               |

---

## Expected Input

The engine expects tick-level market data containing at least the following fields:

| Column    | Description          |
| --------- | -------------------- |
| timestamp | Tick timestamp       |
| price     | Executed trade price |
| volume    | Trade size           |

Additional columns may be preserved depending on the configuration.

---

## Output

Generated bars follow the standard OHLCV structure:

| Column    |
| --------- |
| timestamp |
| open      |
| high      |
| low       |
| close     |
| volume    |

Additional statistics can be included depending on the selected bar type.

---

## Performance

The engine is designed for sequential processing of large tick datasets.

Core aggregation routines are compiled using **Numba**, significantly reducing execution time compared to pure Python implementations.

Benchmark results will be added in future releases.

---

## Why This Project?

Unlike traditional implementations that require separate scripts for every aggregation method, this engine provides a single, reusable framework capable of producing multiple market microstructure bar types while maintaining consistent behavior across the entire pipeline.

The goal is to make quantitative data preprocessing:

* Faster
* More reliable
* Easier to maintain
* Ready for institutional research

---

## Roadmap

Future extensions include:

* Imbalance Bars
* Run Bars
* Information Driven Bars
* VWAP Bars
* Custom Aggregation Plugins
* GPU Acceleration
* Parallel Processing

---

## Contributing

Contributions are welcome.

Whether you're improving performance, adding new bar types, or extending the architecture, feel free to open an issue or submit a pull request.

---

## License

MIT License

---

**Built for high-performance quantitative finance research.**
