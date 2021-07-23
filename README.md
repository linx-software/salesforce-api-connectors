# salesforce-api-connectors

## Description


This sample contains multiple pre-built [Linx](https://linx.software) functions which allow you to quickly connect and make HTTP requests the [Salesforce REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm). These 'connector' functions have been built and tested to handle to the specifics of interacting with the various Salesforce API methods and objects. You can copy and use these functions in your own Linx Solution to accelerate development. 


The goal of this Linx Solution is to ultimately include custom built connector functions for all the methods of the Salesforce REST API. 

## Installation

### Retrieving your instance URL:
Requests made to the Salesforce API must use a unique URL which is based on their instance name. This name needs to be retrieved using the [Identity Url](https://help.salesforce.com/articleView?id=remoteaccess_using_openid.htm&type=0) from Salesforce. This identity url can be retrieved by [querying for user info](https://help.salesforce.com/articleView?id=sf.remoteaccess_using_userinfo_endpoint.htm&type=5) with the access token string. 

1. Install the Linx Designer. Download it [here.](https://linx.software)
2. Open the sample Solution '.lsoz' in your Linx Designer.
3. Open the Solution's settings and add your Salesforce instance name. For example, if my Salesforce instance URL is `https:/linx-dev-ed.salesforce.com/` then my instance name is "linx-dev-ed". 
   
   A function _GetIdentity_ has been included in the sample to retrieve this information if you already posses your access token, see [below](#access-token-generation) for info on access generation.. 

### Access token generation

Authentication of requests is achieved via access tokens, the functions take in the "access token" used in the request as an input parameter at runtime, which is then added to the header of the request. 

This "access token" needs to be passed in to each function by you, this could be retrieved from a database, file or external service. 

You're able to generate and retrieve your token from an external authentication service or, alternatively, you're able to host your own Linx authentication service with the [linx-oauth-token-service](https://github.com/linx-software/linx-oauth2-token-service) project on GitHub.


## Using the connector functions

This sample contains generic "connector" functions which can be imported and used in your own Linx Solution.
 
Each connector function in the Solution follows the below structure:
- Takes in an "access token" value as an input parameter.
- Takes in any data used for the request parameters such as query, path or requestBody values.
- Makes a HTTP request to the API and returns a string response.
- The response string is then de-serialized into the function result object type.

These functions do not persist any data and only return or send data that is received at runtime, therefore you must add your own data persistence layer if required.

### Testing the connectors

An automated testing function has been included in each sub-sections of the Solution Explorer within the sample Solution to test the relevant functionality. For example the 'Accounts' section contains all the neccessary methods for interacting with the 'Account' object on the Salesforce API.

To test out an area's functionality:
1. Located the Test folder of the sub-sections of in the Solution (i.e. Accounts > Tests).
2. Initalize the debugger on the _TestAllXXXRequests_ functions (i.e. _TestAllAccountRequests_).
3. Add your access token as an input parameter.
4. Start the debugger.
5. The function executes the following requests with test data (i.e _TestAllAccountRequests_):
   - Creates a new account
   - Retrieves the details of the new account
   - Retrieves a list of all of the accounts
   - Loops through the account list
   - Updates the relevant account
6. When the function completes, a test object will have been created and updated on the Salesforce API.



### Importing the connector functions into your own Solution

2. Right-click on the **SalesforceAPI** folder in the Solution Explorer.
3. Select **Copy**.
4. Open your own Solution.
5. Right-click on your Solution Explorer and click **Paste**.
4. A validation error will occur referencing the missing setting values.
5. Copy and paste the setting names and values from the Sample solution into your own Solution.
6. The validation messages will dissapear and you can now drag the required connector functions into your own custom functions.


## Contributing

For questions and issues please ask the [Linx community](https://linx/software/community) or use the [Slack channel](https://linxsoftware.slack.com/archives/C01FLBC1XNX). 

If you have any specific requests for sObjects that have not yet been implemented, get in touch with support@linx.software.

## License

[MIT](https://github.com/linx-software/template-repo/blob/main/LICENSE.txt)