# API

The API offers one endpoint:

## GET /customers

This endpoint accepts four query parameters, as listed below.

### Query parameters

- `page` (optional; current page - default is 1)
- `limit` (optional; number of customers to be shown per page - default is 10)
- `size` (optional; filter customers by size: 'Small', 'Medium', 'Enterprise', 'Large Enterprise', 'Very Large Enterprise' - default is 'All')
- `industry` (optional; filter customers by business industry: 'Logistics', 'Retail', 'Technology', 'HR', 'Finance' - default is 'All')

Below is an example of how the request URL would look like if you wanted to retrieve ten medium-sized customers from the technology industry on page two:

`http://localhost:3001/customers?page=2&limit=10&size=Medium&industry=technology`

### Response body

If there are customers in the database, the following JSON structure should be returned after a successful request.

```json
{
  "customers": [
    {
      "id": 1,
      "name": "Thompson, Zboncak and Mueller",
      "employees": 850,
      "contactInfo": null,
      "size": "Medium",
      "industry": "Technology",
      "address": {
        "street": "988 Kimberly Fort Apt. 921",
        "city": "Lake Tracy",
        "state": "Connecticut",
        "zipCode": "07115",
        "country": "United States of America"
      }
    },
    {
      "id": 2,
      "name": "Americas Inc.",
      "employees": 200,
      "contactInfo": {
        "name": "John Smith",
        "email": "jsmith@americasinc.com"
      },
      "size": "Medium",
      "industry": "Technology",
      "address": {
        "street": "5099 Murray Inlet",
        "city": "South Tiffany",
        "state": "Kentucky",
        "zipCode": "08496",
        "country": "United States of America"
      }
    },
    ...
  ],
  "pageInfo": {
    "currentPage": 2,
    "totalPages": 20,
    "totalCustomers": 199
  }
}

```
**Notes:**

- The **contactInfo** and **address** values are `null` when the customer doesn't have contact or address information in the database;
- The `size` attribute in the response for each customer is dynamically determined based on the number of employees according to the specified criteria:
  - **Small** when the **Number of employees** is less than 100;
  - **Medium** when it is greater or equal to 100 and less than 1000;
  - **Enterprise** when it is greater than or equal to 1000 and less than 10000;
  - **Large Enterprise** when it is greater than or equal to 10000 and less than 50000;
  - **Very Large Enterprise** otherwise;
- The `industry` attribute corresponds to the business sector the company belongs to and can be one of the following: **Logistics**, **Retail**, **Technology**, **HR**, **Finance**;
- The API responds with a `400 Bad Request` status code for invalid request parameters, such as negative or non-number values for `page` or `limit`, or unsupported values for `size` or `industry`.
