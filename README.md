# Account-Reparenting
Part of a technical challenge from Braintree.

## Usage Instructions
1. Create an External Data Source called Braintree
2. Create an External ID field on Account called Braintree_ID using the new External Data Source
3. Insert RESTAccountReparent.apxc into your Salesforce codebase
4. POST to the Rest Endpoint
  * URL: https://na24.salesforce.com/services/apexrest/RESTAccountReparent/
  * BODY: Braintree_ID=MyBraintree_ID&Name=MyAccountName

## Design
Reparenting is achieved through a single endpoint by means of a nested if structure. There are four possible responses, three of which are successful and one where no parent accounts can be found for a name. POST was chosen as new accounts can be made through this endpoint.

## Testing
To test this endpoint, you need at least 5 different endpoint calls to get 100% code coverage. These tests run through the code in the order of the return statements:

1. Call the endpoint with Braintree_ID null and Name null
  * Error message returned
2. Insert an Account with Braintree ID “1” and Name "A", Insert an Account with Braintree ID “2” and Name "B", call the endpoint using Braintree ID “1” and Name "B"
  * B will become the parent of A
3. Insert an Account with Braintree ID “1” and Name "A", call the endpoint using Braintree ID “1” and Name "B"
  * B will not be found
4. Insert an Account with Braintree ID “1” and Name “A”, call the endpoint with Braintree ID "2" and Name “A”
  * A new Account will be made with Name "A" and have parent to the original account "A".
5. Call the end point with Braintree ID “1” and Name “A”
  * A new Account will be made with Braintree ID “1” and Name “A”
