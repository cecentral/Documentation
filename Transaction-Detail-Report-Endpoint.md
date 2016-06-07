# Transaction Detail Report Endpoint
```last revision: 04/27/2016```

All API endpoints can be accessed via the API subdomain of the site
- e.g., `http://api.bucme.org`

- followed by ```/reports/transactions```

## Authentication

This is a secured endpoint. Requests must provide either a valid ProjectCentral login cookie or an [`authorization token`](https://github.com/cecentral/documentation/blob/master/Token%20Based%20Authentication%20for%20API%20Endpoints.md) in the request header.

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
- `400 Bad Method`: Server does not understand request, malformed syntax.
- `401 Unauthorized`: No or invalid authentication credentials.
- `405 Method Not Allowed`: Reqest method not allowed for target resource.
- `406 Not Acceptable`: The response can't be formatted in an acceptable media type. In particular, the `Accept` header doesn't include `text/csv` or `application/json`.

## Examples

### Command line

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

###Console

The following request makes use of an `Authentication Token` to request a response in `JSON` for all transactions entered between 2016-04-06 at midnight and 2016-04-07 at midnight, 48 total hours of transactions. An example response is provided.

**Request**

- URL: http://api.domain.name/reports/transactions?startDate=20160406&endDate=20160407
- Method: `GET`
- Headers:

```
Authorization: Bearer ABC.XYZ.123
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
        ["id", "parent_id", "activity_id", "activity_code", "accounting_code", "amount", "doc_type_id", "transaction_type", "billing_credit_card_type", "transaction_number", "pl_account_id", "pl_account_name", "pl_account_type", "gl_account_id", "gl_account_name", "gl_account_number", "cost_center_id", "cost_center_name", "cost_center_number", "date1", "date2", "date3", "sap_doc_id", "ref1", "ref2", "notes", "timestamp", "vendor", "vendor_type"],
        {
            "id": "50057",
            "parent_id": null,
            "activity_id": "11678",
            "activity_code": "MLS16143",
            "accounting_code": "",
            "amount": "2.00",
            "doc_type_id": "2",
            "transaction_type": "Credit Card",
            "billing_credit_card_type": "American Express",
            "transaction_number": "11678X137272",
            "pl_account_id": "3",
            "pl_account_name": "Certificate\/Registration Fees",
            "pl_account_type": "Revenue",
            "gl_account_id": "77",
            "gl_account_name": "Revenue-Tuition",
            "gl_account_number": "408610",
            "cost_center_id": "6",
            "cost_center_name": "UK HealthCare CE",
            "cost_center_number": "1013204610",
            "date1": "2016-06-06",
            "date2": "2016-06-06",
            "date3": "0000-00-00",
            "sap_doc_id": null,
            "ref1": "XXXX0002",
            "ref2": null,
            "notes": "AUTOINSERT",
            "timestamp": "2016-06-06 16:20:35",
            "vendor": "Charles Combs",
            "vendor_type": null
        }, {
            "id": "50058",
            "parent_id": null,
            "activity_id": "11678",
            "activity_code": "MLS16143",
            "accounting_code": "",
            "amount": "2.00",
            "doc_type_id": "2",
            "transaction_type": "Credit Card",
            "billing_credit_card_type": "Discover",
            "transaction_number": "11678X133977",
            "pl_account_id": "3",
            "pl_account_name": "Certificate\/Registration Fees",
            "pl_account_type": "Revenue",
            "gl_account_id": "77",
            "gl_account_name": "Revenue-Tuition",
            "gl_account_number": "408610",
            "cost_center_id": "6",
            "cost_center_name": "UK HealthCare CE",
            "cost_center_number": "1013204610",
            "date1": "2016-06-06",
            "date2": "2016-06-06",
            "date3": "0000-00-00",
            "sap_doc_id": null,
            "ref1": "XXXX0012",
            "ref2": null,
            "notes": "AUTOINSERT",
            "timestamp": "2016-06-06 16:22:33",
            "vendor": "Seth Anderson",
            "vendor_type": null
        }, {
            "id": "50059",
            "parent_id": null,
            "activity_id": "11678",
            "activity_code": "MLS16143",
            "accounting_code": "",
            "amount": "2.00",
            "doc_type_id": "2",
            "transaction_type": "Credit Card",
            "billing_credit_card_type": "Visa",
            "transaction_number": "11678X5209",
            "pl_account_id": "3",
            "pl_account_name": "Certificate\/Registration Fees",
            "pl_account_type": "Revenue",
            "gl_account_id": "77",
            "gl_account_name": "Revenue-Tuition",
            "gl_account_number": "408610",
            "cost_center_id": "6",
            "cost_center_name": "UK HealthCare CE",
            "cost_center_number": "1013204610",
            "date1": "2016-06-06",
            "date2": "2016-06-06",
            "date3": "0000-00-00",
            "sap_doc_id": null,
            "ref1": "XXXX0027",
            "ref2": null,
            "notes": "AUTOINSERT",
            "timestamp": "2016-06-06 16:23:41",
            "vendor": "Candy Back",
            "vendor_type": null
        }]
}
```
