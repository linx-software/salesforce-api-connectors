# salesforce

## Description

Managing many of the existing manual tasks on Salesforce such as updating customers between systems or recieving quotes, can be automated with Linx and API requests. 

This sample demonstrates automating some account related tasks on Salesforce.

The sample also contains some generically designed functions that connect and make requests to the Salesforce API. You can copy and use these functions in your own Linx Solution to integrate with several 3rd-party systems such as Xero, Quickbooks or SageX3 for accouting or something like DEARSystems for inventory management which can all be done via API.


Features:
- Syncing accounts from a csv file.
- Deleting all accounts.
- Retrieving and writing out accounts to a csv file.


## Installation


### Create a new connected app on Salesforce:

1. Register a new connected app on [Salesforce.com](https://login.salesforce.com/)
1. Enable the OAuth 2.0 settings.
   - Configure the Callback URL to be: `http://localhost:8080/salesforce/oauth/token`
   - Select the scopes:
      - `Full access (full)`
      - `Perform requests on your behalf at any time (refresh_token, offline_access)`
1. Save your connected app.
1. Generate your **Consumer Secret**.
1. Copy the **Consumer Key** and **Consumer Secret**.

### Configure the Solution's $.Settings:

1. Install Linx Designer. Download it [here](https://linx.software/).
1. Open the sample Solution (.lsoz) in your Linx Designer.
1. Edit the $.Settings values:

   - `linx_database_conn_string`: Configure this setting value with the connection string to your DB instance.
   - `salesforce_app_consumer_key`: Your connected app’s **Consumer Key**
   - `salesforce_app_consumer_secret`: Your connected app’s **Consumer Secret**
   - `salesforce_api_version`: Current version of the API, at the time of writing it is `v50.0`, and may change at a later stage.

1. Save the Solution.

### Generate access tokens:

1. Start the debugger on the RESTHost service in the Linx Designer.
2. Make a request in your browser to `http://localhost:8080/salesforce/oauth/authorize`
3. You will be redirected to the Salesforce OAuth 2.0 access consent screen.
4. Authorize the connected application.
5. View success message.



## Using the sample

### Syncing Accounts from a csv file

Keep your accounts on Salesforce up to date with a csv file.

This process creates new accounts or updates existing account objects on the Salesforce API based on data imported from a csv file.

The logic is as follows:
1. A local directory is scanned and returns matching csv files which contain customer account details.
2. Each file is then imported line by line and the details of each account are then matched on the Salesforce API.
3. If an account does not exist then a new account is created using the details retrieved from the file.
4. If an account already exist, then the latest account details are retrieve from the API, compared to the file record based on the _last modified date_ and if the file record is more recent then the account is updated on the Salesforce API.
5. Once the import process has completed, the file is moved to a backup location and a record of all the affected accountIds are logged to a local file.

To sync a csv file with your instance of Salesforce, you can use the provided sample csv file as a template for the schema for your data.

1. Generate the acess token.
1. Add your csv file to the directory   `C:\Linx\Salesforce\Accounts\Upload\`
2. Run the the _AddUpdateAccountsFromCsv_ function.

### Delete all accounts

Remove all the accounts from an instance of Salesforce.

All the account objects are retrieved from the Salesforce API. For each account, a DELETE request is made to the API which removes it from your instance of Salesforce.

To remove all the accounts on your instance of Salesforce follow the below steps:
1. Generate the the access token.
2. Run the _DeleteAllAccounts_ function.

### Export accounts into a csv file

Retrieve all the accounts from Salesforce and write them into a csv file. 

This can be used in situations where you need to work with offline or internal documents. This file can then also be synced to something like googlesheets which can be accessed by several users without Salesforce access.

To write out all the accounts on your instance of Salesforce to a csv file:
1. Run the _WriteAccountsToCsv_ function.
2. A file containing the accounts will be written to the directory: `C:\Linx\Salesforce\Accounts\Upload\`

_If a file already exists with the same name then the data will get overwritten._

## Contributing

For questions please ask the [Linx community](https://linx/software/community) or use the [Slack channel](https://linxsoftware.slack.com/archives/C01FLBC1XNX). 

## License

[MIT](https://github.com/linx-software/template-repo/blob/main/LICENSE.txt)