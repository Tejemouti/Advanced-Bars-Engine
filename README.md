# Market Microstructure Bars Engine

> **A high-performance Python engine for generating institutional-grade financial bars from raw tick data.**

Transform millions of raw market ticks into **Time**, **Daily**, **Tick**, **Volume**, and **Dollar Bars** through a single unified API optimized for quantitative finance research.

---

## Overview

Market microstructure research begins with one fundamental task: transforming raw tick data into structured financial bars.

Traditional implementations often rely on separate scripts for each aggregation method, leading to duplicated logic, inconsistent behavior, and difficult maintenance.

**Market Microstructure Bars Engine** solves this problem with a unified, extensible architecture capable of generating multiple bar types from the same API while remaining efficient enough for large-scale quantitative datasets.

Designed for quantitative researchers, algorithmic traders, and financial engineers.

---

## Features

- **Unified Factory API**
  - Generate every supported bar type through a single entry point.

- **Multiple Aggregation Methods**
  - Time Bars
  - Daily Bars
  - Tick Bars
  - Volume Bars
  - Dollar Bars

- **Numba Accelerated**
  - JIT-compiled core algorithms for processing millions of ticks with minimal overhead.

- **Institutional Data Integrity**
  - Preserves chronological ordering and prevents lookahead bias.

- **CME Session Alignment**
  - Daily bars follow actual CME trading sessions rather than calendar days.

- **Scalable Storage**
  - Optional Hive-style partitioned Parquet output for efficient data pipelines.

- **Modular Architecture**
  - Easily extend the engine with new market microstructure bar types.

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

| Bar Type | Parameter |
|----------|-----------|
| **Time** | Any valid pandas frequency string (e.g. `"30s"`, `"1min"`, `"5min"`, `"15min"`, `"1H"`, `"4H"`, `"1D"`, etc.) |
| **Daily** | `"daily"` (CME session aligned) |
| **Tick** | Any positive integer representing the number of trades per bar |
| **Volume** | Any positive integer representing cumulative traded volume per bar |
| **Dollar** | Any positive numeric value representing cumulative traded dollar value per bar |

> **Note:** Time bars accept any frequency string supported by **pandas `resample()`**.

---

## Expected Input

The engine expects tick-level market data containing at least the following fields:

| Column | Description |
|---------|-------------|
| timestamp | Tick timestamp |
| price | Executed trade price |
| volume | Trade size |

Additional columns may be preserved depending on the selected configuration.

---

## Output

Generated bars follow the standard **OHLCV** structure:

| Column |
|---------|
| timestamp |
| open |
| high |
| low |
| close |
| volume |

Additional statistics may be included depending on the selected aggregation method.

---

## Performance

The engine is designed for efficient sequential processing of large tick datasets.

Core aggregation algorithms are accelerated using **Numba**, providing substantial performance improvements over pure Python implementations.

Performance benchmarks will be published in future releases.

---

## Why This Project?

Unlike traditional implementations that require separate scripts for every aggregation method, this engine provides a single reusable framework capable of generating multiple market microstructure bar types while maintaining consistent behavior across the entire preprocessing pipeline.

The objective is simple:

- Faster preprocessing
- Cleaner architecture
- Reliable data generation
- Institutional-grade research workflows

---

## Roadmap

Future extensions include:

- Imbalance Bars
- Run Bars
- Information-Driven Bars
- VWAP Bars
- Custom Aggregation Plugins
- GPU Acceleration
- Parallel Processing
- Streaming / Real-Time Bar Generation

---

## Contributing

Contributions are welcome.

Whether you're improving performance, adding new aggregation methods, fixing bugs, or extending the architecture, feel free to open an Issue or submit a Pull Request.

---

## License

This project is released under the **MIT License**.

---

## Author

Developed as part of a high-performance quantitative finance toolkit focused on scalable market microstructure research.

---

**Built for quantitative researchers, algorithmic traders, and financial engineers.**
