# Salesforce integration

## Overview

The provided sample includes:

- Generating Salesforce API access tokens via OAuth 2.0
- Authenticating and connecting to the Salesforce API via Linx
- Template requests: Retrieving and sending data to the Salesforce API via HTTP requests.
- Using the template requests with custom logic.

---

### Additional resources

- [Salesforce-Linx integration guide](https://community.linx.software/community/t/integrating-with-salesforce/494)
- [Salesforce API authentication documentation](https://help.salesforce.com/articleView?id=sf.remoteaccess_oauth_web_server_flow.htm&type=5)

---

## Dependencies

### Database

The solution uses a SQL server database to store the authentication tokens in a table (`tblToken`) with the below structure:

```sql
IF OBJECT_ID(N'dbo.tblToken', N'U') IS  NULL
BEGIN
   CREATE TABLE [dbo].[tblToken](
   [ID] INT IDENTITY(1,1) NOT NULL,
   [platform] VARCHAR(500) NULL,
   [platform_id] VARCHAR(500) NULL,
   [state] VARCHAR(500) NULL,
   [code_verifier] VARCHAR(500) NULL,
   [code_challenge] VARCHAR(500) NULL,
   [token_object] VARCHAR(8000) NULL,
   [token_type]  VARCHAR(500) NULL,
   [access_token] VARCHAR(8000) NULL,
   [refresh_token] VARCHAR(8000) NULL,
   [expires_in] INT NULL,
   [expiry_time] DATETIME NULL,
   [last_updated] DATETIME NULL
) ON [PRIMARY]
END
```

---

### Pre-requisites

- Linx Designer
- Salesforce account

### Linx Designer

This solution was developed in the Linx Designer `v5.20.2.0`

### Salesforce API

The version of the Salesforce API at the time of writing is `v50.0`.

---

## Setting up the sample

Create a new connected app on Salesforce:

1. Register a new connected app on Salesforce
1. Enable the OAuth 2.0 settings.
   1. Configure the Callback URL to be: `http://localhost:8080/salesforce/oauth/token`
   1. Select the scopes:
      - `Full access (full)`
      - `Perform requests on your behalf at any time (refresh_token, offline_access)`
1. Save your connected app.
1. Generate your **Consumer Secret**.
1. Copy the **Consumer Key** and **Consumer Secret**.

Configure the Solution's $.Settings:

1. Open the [sample Solution](Solution.lsoz) in your Linx Designer.
1. Edit the $.Settings values:

   - `linx_database_conn_string`: Configure this setting value with the connection string to your DB instance.
   - `salesforce_app_consumer_key`: Your connected app’s **Consumer Key**
   - `salesforce_app_consumer_secret`: Your connected app’s **Consumer Secret**
   - `salesforce_api_version`: Current version of the API, at the time of writing it is `v50.0`, and may change at a later stage.

1. Save the Solution.

Generate access tokens:

1. Start the debugger on the RESTHost service in the Linx Designer.
2. Make a request in your browser to `http://localhost:8080/salesforce/oauth/authorize`
3. You will be redirected to the Salesforce OAuth 2.0 access consent screen.
4. Authorize the connected application.
5. View success message.

---

## Using the sample

### Authentication

Requests are authenticated via a Bearer access token included in the `Authorization` header of each request.
The token is retrieved from the database before each request is made.

The token is generated using the above [OAuth 2.0 flow](https://help.salesforce.com/articleView?id=sf.remoteaccess_oauth_web_server_flow.htm&type=5). The token needs to be refreshed on a constant basis to keep it active which can be achieved by triggering the refresh token generation flow on a timer.

### Making requests

Requests are made to a custom URL which is built up from your instance URL. This base URL is returned from the database and used in each requests as the base.

---

## Template HTTP requests

The following custom functions wrap the different types of HTTP calls into their own re-usable functions which can be thought of as 'custom Salesforce connectors'.

---

### Account

Location: [Solution] > [Project: Salesforce] > [Folder: Requests]

#### GetAccounts

Retrieves all the existing Account Ids from Salesforce and returns the list of objects as the result.

#### GetAccount

Retrieves the detailed information associated with a specific account based on the `Id` input parmeter. This can be used to check modification history as well as used to update the sObject.

#### CreateAccount

Create a new account based on the Account details passed in as the input parameter.

#### UpdateAccount

Modify an existing account based on the Account details passed in as the input parameter.

#### DeleteAccount

Remove an existing account based on an `Id` input parameter.

---

### Template Functions

The following custom functions use the request wrapper functions.

---

#### AddUpdateAccountFromCSV

This function creates new accounts or updates existing account objects on the Salesforce API.

> A file containing testing data related to customers has been added to the repo to test: [mock_customers.csv](mock_customers.csv).

The function scans a local directory and returns the matching CSV files for the filter.

The CSV file is imported and then the details are passed into the wrapper function which searches for a matching account on the Salesforce API.

If an account does not exist then a new account is created using a wrapper function using the details retrieved from the text file.

If an account already exist, then the latest account details are retrieve from the API, compared to the file record based on the last modified date and if the file record is more recent then the account is updated on the Salesforce API.

Once the import process has completed, the file is moved to a backup location and a record of all the affected accountIds are logged to a local file.

---

#### DeleteAllAccounts

This function retrieves all the account objects from the Salesforce API. For each account, a DELETE request is made to the API which removes it.

> ⚠️ WARNING: This will remove all the accounts on your instance of Salesforce and should only be used during development.

---

#### WriteAccountsToCSV

This function retrieves all the accounts from the Salesforce API and logs them to a local CSV file.
