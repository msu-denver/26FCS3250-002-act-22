# Overview

In this activity you will build a modern API on top of a legacy system you cannot change.

# The Scenario

Bridgewater County Utilities is a fictional water and power provider. Its billing and service records live in a database built in the late 1990s. That database can't be touched. A vendor contract forbids schema changes, a nightly batch job depends on the exact table layout, and nobody fully understands the stored procedures. Leadership wants a modern "Customer 360" web portal that shows a customer's account, billing history, and service tickets in one place.

The rule is absolute: the legacy database is read-only, and its structure cannot be modified. No new columns, no indexes, no views, no cleanup. Every improvement has to happen in a layer built on top of it.

# Step 1: Data Archeology

Begin by exploring the legacy data source in [legacy/bridgewater.sql](legacy/bridgewater.sql) without writing any application code. Produce a data dictionary that covers what each column means and its valid values. 

Fill in the tables below, one per legacy table.

## CUSTMAST

| Column | Description | Valid Values |
|---|---|---|
| CUST_NO | | |
| CUST_NM | | |
| ADDR1 | | |
| CITY_NM | | |
| ZIP_CD | | |
| PHONE_NO | | |
| SVC_TYP | | |
| STAT_CD | | |
| OPEN_DT | | |

## BILLHIST

| Column | Description | Valid Values |
|---|---|---|
| CUST_NO | | |
| BILL_PRD | | |
| SVC_TYP | | |
| AMT_DUE | | |
| PD_FLG | | |
| PD_DT | | |

## CODETBL

| Column | Description | Valid Values |
|---|---|---|
| TBL_ID | | |
| CD_VAL | | |
| CD_DESC | | |

# Step 2: API Specification

The [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) (OAS) is a standard YAML or JSON format for describing REST APIs: their endpoints, parameters, responses, and errors. Because it is machine-readable, tools can use it to generate documentation, server code, clients, and tests.

In this activity you will follow a **contract-first** approach, specifying the API formally before writing any code. Designing the contract first makes you decide how the data *should* look instead of letting legacy names like CUSTMAST or PD_FLG leak into the API. 

Using GenAI, write an OpenAPI specification using the prompt in [prompts/bridgewater_oas.json](prompts/bridgewater_oas.json). When asked for the OAS version to use, pick 3.1.0. Also, when asked for the format of the specification, choose yaml. 

# Step 3: Implementation

Use GenAI again to generate the code for the API using Python's FastAPI and SQLite. You can ask: 

```
Generate the API from api/bridgewater.yaml in src using Python's FastAPI and SQLite, making sure that the database is created and populated using the SQL script in legacy/bridgewater.sql. 
```

# Step 4: Test the API

Create and activate a virtual environment and install requirements in `requirements.txt`. Then run the API using `uvicorn src.main:app`. 

Try the following endpoints: 

```
http://127.0.0.1:8000/v1/customers
http://127.0.0.1:8000/v1/customers/18489
http://127.0.0.1:8000/v1/customers/18489/bills
http://127.0.0.1:8000/v1/customers/18489/summary 
```