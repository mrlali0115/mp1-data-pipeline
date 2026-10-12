# MP1 Data Pipeline

This project involves a command line data processing pipeline that facilitates reading a CSV file, validation and cleaning, and outputs the cleaned data in the form of a new CSV file. Each cleaning operation is defined in the `config/config.yaml` file, therefore the same script can be reused for other datasets simply by changing the configuration. The working of the pipeline is in a fixed order where `pipeline.py` first checks the input file and configuration file availability, and then processes the input. The steps in the pipeline are coordinated by `pipeline.py`, which also manages errors. Throughout this project, reusable modules defined in the `src/` package are utilized. The task of `src/data_loaders.py` is to read CSV, JSON, and YAML files, while `src/data_validator.py` performs the validation and formatting processes of the files read. The processing of the data is specified as per `src/data_processor.py`, which involves removal of duplicates, dropping of rows with null values, removal of outliers, and the generation of a cleaning report. Finally, `src/data_output.py` is responsible for saving the cleaned data in the output folder, and lastly logging and checking for necessary files is done by `src/utils.py`.

## Example command

```bash
python pipeline.py --input fixtures/sample_data.csv --output output/clean.csv --config config/config.yaml --verbose
```

## Example output

```
16:05:40 DEBUG    __main__ — Arguments parsed: input=fixtures/sample_data.csv, config=config/config.yaml, output=output/clean.csv
16:05:40 INFO     src.utils — Input file validated: fixtures/sample_data.csv
16:05:40 INFO     src.utils — Input file validated: config/config.yaml
16:05:40 INFO     src.data_loaders — Loaded CSV file: fixtures/sample_data.csv (100 rows)
16:05:40 INFO     src.data_loaders — Loaded YAML file: config/config.yaml
16:05:40 WARNING  src.data_validator — Invalid numeric value in rating at row 94: not_available
16:05:40 WARNING  src.data_validator — Invalid numeric value in rating at row 95: error
16:05:40 WARNING  src.data_validator — Removed 2 rows with invalid numeric values in rating
16:05:40 DEBUG    src.data_validator — Validation: 100 -> 98 rows
16:05:40 INFO     __main__ — Validation complete: 100 -> 98 rows
16:05:40 DEBUG    src.data_processor — remove_duplicates: 98 → 96 rows
16:05:40 DEBUG    src.data_processor — handle_missing: 96 → 94 rows
16:05:40 DEBUG    src.data_processor — remove_outliers: method=iqr, threshold=1.5
16:05:40 DEBUG    src.data_processor — rating: lower=43.625, upper=106.625, removed=2
16:05:40 INFO     __main__ — Processing complete: 98 → 92 rows
16:05:40 DEBUG    src.data_output — Saved 92 rows to output/clean.csv
16:05:40 INFO     __main__ — Saved cleaned data to output/clean.csv
Cleaning report:
{'rows_before': 98, 'rows_after': 92, 'rows_removed': 6, 'columns_before': 5, 'columns_after': 5, 'columns_removed': 0}
```