# Transaction Detail Report Endpoint
```last revision: 04/27/2016```

All API endpoints can be accessed via the API subdomain of the site
- e.g., `http://api.bucme.org`

- followed by ```/reports/transactions```

## Authentication

Requester **must** either have a `ProjectCentral login cookie` present or **shall** send an [`authorization token`](https://github.com/cecentral/documentation/blob/master/Token%20Based%20Authentication%20for%20API%20Endpoints.md) in the request header.

## Supported Methods

 - `GET`

## GET

### Parameters

- `startDate:` _Optional_. The date of the earliest transaction record to include. 
- `endDate:` _Optional_. The date of the latest transmission record to include.

The date used by these filtering parameters corresponds to the `date1` field in the result set.

The `startDate` and `endDate` parameters are optional. When omitted, the endpoint uses the following defaults:

- If `startDate` is specified but `endDate` is not, `endDate` defaults to the current date.
- If `endDate` is specified, but `startDate` is not, `startDate` defaults to 1 year before `endDate`.
- If neither `startDate` nor `endDate` is specified, then:
    - `endDate` defaults to the current date
    - `startDate` defaults to one day ago.

In all cases, `startDate` and/or `endDate` will match transactions that were entered at any point on that day. So `startDate=20160415` will match a transaction entered into the transaction log at any time between 2016-04-15 at midnight up until just before midnight of 2016-04-16.

### Supported Response Media Types

- CSV (`text/csv`), default
- JSON (`application/json`)

### Response Codes

- `200 OK`: Report details successfully retrieved.
- `400 Bad Method`:
- `401 Unauthorized`: No or invalid authentication credentials.
- `405 Method Not Allowed`:
- `406 Not Acceptable`: The response can't be formatted in an acceptable media type. In particular, the `Accept` header doesn't include `text/csv` or `application/json`.

### Examples

 The following requests make use of an `Authorization Header` bearing an `Authorization Token`.  This method does not require the presence of a `ProjectCentral login cookie`.
 
 
 _Here we are requesting a response in `JSON` for all transactions entered between 2016-04-06 at midnight and 2016-04-07 at midnight, 48 total hours of transactions._
```
curl -X GET
     -H "Authorization: Bearer ABC.XYZ.123"
     -H "Accept: application/json"
     "http://api.domain.name/reports/transactions?startDate=20160406&endDate=20160407"
```

 _Here we are requesting a response in `CSV` for all transactions entered between 2016-03-06 at midnight and the current date and time._
```
curl -X GET
     -H "Authorization: Bearer ABC.XYZ.123"
     -H "Accept: text/csv"
     "http://api.domain.name/reports/transactions?startDate=20160306"
```


 The following requests require the presence of a `ProjectCentral login cookie`.


 _Here we are requesting a response in `CSV` for all transactions entered one year prior to midnight of 2016-04-07._
```
curl -X GET
     "http://api.domain.name/reports/transactions?endDate=20160407"
```

 _Here we are requesting a response in `JSON` for all transactions within the default time frame of the previous 48 hrs._
```
curl -X GET
     -H "Accept: application/json"
     "http://api.domain.name/reports/transactions"
```

**Request**

- URL: http://api.domain.name/reports/transactions?startDate=20160406&endDate=20160407
- Method: `GET`
- Headers:

```
Accept: application/json
```
 
- Request Body: Empty

**Response**

- Status Code: 200
- Relevant Headers:

```
Content-Disposition: inline;filename="transaction-20160406-20160418.json"
Content-Type: application/json
```

- Response Body: 

```
{
    "content": [
        ["id", "parent_id", "activity_id", "doc_type_id", "pl_id", "gl_id", "cost_id", "amount", "transaction_number", "date1", "date2", "date3", "sap_doc_id", "ref1", "ref2", "notes", "timestamp", "activity_code", "type", "vendor", "pl_name", "pl_type", "gl_name", "gl_num", "accounting_code"],
        {
            "id": "49810",
            "parent_id": null,
            "activity_id": "11641",
            "doc_type_id": "2",
            "pl_id": "3",
            "gl_id": "77",
            "cost_id": "6",
            "amount": "50.00",
            "transaction_number": "11641X136340",
            "date1": "2016-04-08",
            "date2": "2016-04-08",
            "date3": "0000-00-00",
            "sap_doc_id": null,
            "ref1": "XXXX0002",
            "ref2": null,
            "notes": "AUTOINSERT",
            "timestamp": "2016-04-08 08:19:02",
            "activity_code": "PLS16125",
            "type": null,
            "vendor": "Charles Combs",
            "pl_name": "Certificate\/Registration Fees",
            "pl_type": "Revenue",
            "gl_name": "Revenue-Tuition",
            "gl_num": "408610",
            "accounting_code": "",
            "billing_credit_card_type": ""
        }]
}
```
