# One Billion Rows: Data Processing Challenge with Python

## Introduction

The goal of this project is to process a massive data file containing 1 billion rows (~14GB) and calculate statistics (including aggregation and sorting, which are heavy operations) using Python.

Inspired by [The One Billion Row Challenge](https://github.com/gunnarmorling/1brc), originally proposed for Java.

The data file contains temperature measurements from weather stations. Each record follows the format `<string: station name>;<double: measurement>`, with temperature presented to one decimal place.

Ten example lines:

```
Hamburg;12.0
Bulawayo;8.9
Palembang;38.8
St. Johns;15.2
Cracow;12.6
Bridgetown;26.9
Istanbul;6.2
Roseau;34.4
Conakry;31.2
Istanbul;23.0
```

The challenge: read the file and calculate the minimum, mean (rounded to one decimal), and maximum temperature per station, displayed in a table sorted by station name.

| station      | min_temperature | mean_temperature | max_temperature |
|--------------|-----------------|------------------|-----------------|
| Abha         | -31.1           | 18.0             | 66.5            |
| Abidjan      | -25.9           | 26.0             | 74.6            |
| Abéché       | -19.8           | 29.4             | 79.9            |
| Accra        | -24.8           | 26.4             | 76.3            |
| Addis Ababa  | -31.8           | 16.0             | 63.9            |
| Adelaide     | -31.8           | 17.3             | 71.5            |
| Aden         | -19.6           | 29.1             | 78.3            |
| Ahvaz        | -24.0           | 25.4             | 72.6            |
| Albuquerque  | -35.0           | 14.0             | 61.9            |
| Alexandra    | -40.1           | 11.0             | 67.9            |
| ...          | ...             | ...              | ...             |
| Yangon       | -23.6           | 27.5             | 77.3            |
| Yaoundé      | -26.2           | 23.8             | 73.4            |
| Yellowknife  | -53.4           | -4.3             | 46.7            |
| Yerevan      | -38.6           | 12.4             | 62.8            |
| Yinchuan     | -45.2           | 9.0              | 56.9            |
| Zagreb       | -39.2           | 10.7             | 58.1            |
| Zanzibar City| -26.5           | 26.0             | 75.2            |
| Zürich       | -42.0           | 9.3              | 63.6            |
| Ürümqi       | -42.1           | 7.4              | 56.7            |
| İzmir        | -34.4           | 17.9             | 67.9            |

## Dependencies

- Polars: `0.20.3`
- DuckDB: `0.10.0`
- Dask[complete]: `^2024.2.0`

## Results

Tested on an Apple M1 laptop with 8GB RAM. Implementations used pure Python, Pandas, Dask, Polars, and DuckDB.

| Implementation     | Time        |
|--------------------|-------------|
| Bash + awk         | 25 min      |
| Python             | 20 min      |
| Python + Pandas    | 263 sec     |
| Python + Dask      | 155.62 sec  |
| Python + Polars    | 33.86 sec   |
| Python + DuckDB    | 14.98 sec   |

Thanks to [Koen Vossen](https://github.com/koenvo) for the Polars implementation and [Arthur Julião](https://github.com/ArthurJ) for the Python and Bash implementations.

## Conclusion

Pure Python and Pandas need chunking tricks to handle this volume. Dask, Polars, and DuckDB handle it natively through streaming batch processing, which is why they need far less code. DuckDB won — by a lot — due to its query execution strategy.

DuckDB also wins on 1 million rows. It's just the best tool for this kind of work.

## How to Run

1. Clone the repository
2. Run `uv sync` at the project root — uv installs all dependencies and creates the virtual environment automatically
3. Run `python src/create_measurements.py` to generate the test file (takes ~10 minutes, go make coffee)
4. Run whichever scripts you want:
   - `python src/using_python.py`
   - `python src/using_pandas.py`
   - `python src/using_dask.py`
   - `python src/using_polars.py`
   - `python src/using_duckdb.py`

## Bonus: Running the Bash Script

You need a Unix-like environment (Linux or macOS). The script uses `wc`, `head`, `awk`, and `sort`, which come pre-installed. `pv` (Pipe Viewer) is optional — it shows a progress bar but the script runs fine without it.

### Installing pv (optional)

On Ubuntu/Debian:
```bash
sudo apt-get install pv
```

On macOS:
```bash
brew install pv
```

### Running the script

Give it execute permission:
```bash
chmod +x process_measurements.sh
```

Then run it:
```bash
./src/using_bash_and_awk.sh 1000
```

The argument controls how many lines to process. 1000 is a good starting point for testing.

## Next steps

This project is part of *Jornada de Dados*.

[![Image](https://github.com/lvgalvao/data-engineering-roadmap/raw/main/pics/jornada.png)](https://www.jornadadedados2024.com.br/workshops)

[![Image](https://raw.githubusercontent.com/lvgalvao/data-engineering-roadmap/main/pics/lista_de_espera.png)](https://forms.gle/hJMtRDP3MPBUGvwS7?orbt_src=orbt-vst-1RWyYmpICDu9gPknLgaD)