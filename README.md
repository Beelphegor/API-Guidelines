# Contributions

If you have a way to improve or advance this document, please fork the repo, make your changes, and submit a pull request. Upon review and approval, your PR will be merged. 

# Requests
## Versioning:

The API might change between releases. In the event that there are changes to the requests or responses, the old AND new API’s will be available (while Rewardle agrees to support those old versions). The version number will in the first route segment, after the TLD:

Example: `GET /v1/products`

## Resource Names:
Resource names should represent collections, not commands. For example, in the case of a “product” collection:

Example | Translation
--- | ---
`GET /v1/products` | give me all the products
`GET /v1/products/123` | give me the product with this id
`POST /v1/products` | here’s a new product to create
`POST /v1/products/654/images` | here’s a new image to add to the product with this id
`PUT /v1/products/432` | please update the product with this id
`PUT /v1/products/432/name` | please modify the name of the product with this id
`DELETE /v1/products/543` | please delete the product with this id

## Paging/Sorting:

For GET calls that return a collection, the following options will be available via query string arguments:

Argument | Description
--- | ---
`limit` | The limit of items per page.
`page` | The page number that should be returned based on the total number of items and the limit of items per page.
`desc` | Sorting the entire collection in descending order by one property name.
`asc` | Sorting the entire collection in ascending order by one property name.

Example: `GET /v1/products?limit=2&page=1&desc=name`

## Request Headers:

Requests made to the API will accept the following headers to provide more information about the request or requestor.

Header | Description
--- | ---
`X-DeviceId` | The id of the device making the request.


# Responses

## Successful Status Codes:

Code | Description
--- | ---
`200` | Should be used for successful responses where there is a response payload of some sort. (ex GET)
`202` | Should be used in the case that a successful response does not carry a payload (ex: POST, PUT, PATCH, DELETE).

## Failure Status Codes:

Code | Description
--- | ---
`403` | Forbidden means the user is already logged in but doesn’t have permission to access the requested resource.
`401` | Unauthorized means that the user is not logged in yet and tried to access an area that requires authentication.
`404` | Not found means the user tried to access a resource that doesn’t exist. This can apply to mal-formed urls but also to well-formed urls but the resource doesn’t exist in the database.
`400` | Bad request means the request failed some level of validation of the input submitted. No work has been done. Response should include some common structure: `{ Failures: [{ Property: ‘Name’, FailureType: ‘Missing', Message: 'The name was missing.' }] }`
`500` | Unhandled exception (should be rare). Response should include some common structure: `{ Message: ‘Something horrible happened. The swat team has been notified.’ }` (The exception should be logged and the dev team be notified.)

## Arrays:

Responses that return an array of items should contain a common structure. Given the following request to `GET /v1/items?limit=2&page=1&desc=name`:

```
{
	Page: 1, // the page number
	Items: 2, // the number of items in the array
	TotalPages: 5, 	// the total number of pages based on the 
			// totalItems and the items per page (limit)
	TotalItems: 10, // the number of items in the collection
	Items: [{
		name: ‘some item’
		}, {
			name: ‘another item’
		}]
}
```

## Data Types:

Type | Description
--- | ---
`string` | Needs descr
`int` | Needs descr
`datetime` | Javascript Date format as suggested in http://stackoverflow.com/questions/10286204/the-right-json-date-format Example: `2012-04-23T18:25:43.511Z`

## Common Headers:

All responses should include the following headers:

Header | Description
--- | ---
`X-Duration` | How long the request took to complete.
`X-Time` | When the request was executed on the server.
