# Dummy data generator for BigQuery

Generate dummy/mock data for BigQuery using json schema file.

Generated dummy data file can be loaded to BigQuery.

It requires `Python >=2.6, !=3.0.*, !=3.1.*`

## How to use

Clone this repository.

```
git clone git@github.com:tokencard/mock-data-generator-bigquery.git
```

Install the dependencies
```
pip install six
```
Run generator.

```
python dummy_bq.py <table_schema.json>
```

Then, dummy data file named "output.json" is created.

```
cat output.json
```

## Arguments

|option|description|default|
|:---|:---|:---|
|-o|output file name|output.json|
|-l|number of lines(rows) of dummy file|1000|


## Json Schema file

Json schema file for BigQuery defines field name, data type and mode.

|attribute|description|remark|
|:---|:---|:---|
|name|field name||
|type|data type|INTEGER, STRING, FLOAT, TIMESTAMP, RECORD is supported|
|mode|write mode|REQUIRED, NULLABLE, REPEATED is supported|

## Loading data into Bigquery (example)
```
    ## This loads data to:
    ### Project: development-65e9a64cd95055da
    ### Dataset: kraken_test_ledger_history
    ### Table: contis_scheme_transactions

    bq load --location europe-west2 --noreplace --source_format=NEWLINE_DELIMITED_JSON --project_id development-65e9a64cd95055da kraken_test_ledger_history.contis_scheme_transactions output.json
```

## Examples

BigQuery schema file (examples/flat.json)

```json
[
    {
        "name": "hogehoge",
        "type": "INTEGER",
        "mode": "REQUIRED"
    },
    {
        "name": "pokopoko",
        "type": "STRING",
        "mode": "NULLABLE"
    },
    {
        "name": "created_at",
        "type": "TIMESTAMP",
        "mode": "REQUIRED"
    }
]

```

Generate 10 lines dummy data to output.json

```
python dummy_bq.py examples/flat.json -l 10
```

```json
{"hogehoge": 9997, "created_at": "2016-12-10 18:13:19"}
{"hogehoge": 2531, "created_at": "2016-12-10 23:53:18"}
{"hogehoge": 4029, "created_at": "2016-12-11 13:53:50"}
{"hogehoge": 1084, "created_at": "2016-12-11 14:41:07", "pokopoko": "5G4QCT4HZJ48"}
{"hogehoge": 3386, "created_at": "2016-12-11 09:16:28", "pokopoko": "9W37EJ1QC0BY"}
{"hogehoge": 8680, "created_at": "2016-12-11 12:52:20", "pokopoko": "KLP2XNJ0T8ZU"}
{"hogehoge": 4594, "created_at": "2016-12-10 20:13:28"}
{"hogehoge": 5584, "created_at": "2016-12-11 06:08:55"}
{"hogehoge": 6726, "created_at": "2016-12-11 09:05:07"}
{"hogehoge": 754, "created_at": "2016-12-10 22:48:56"}
```

Above data can be uploaded to BigQuery using console or cli.
