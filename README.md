# cpi-partner-directory

## Create Service Credential

- In the BTP cockpit choose **Services -> Service Marketplace**
- Choose the service **Process Integration Runtime**
- Choose **Create** and create a service with the following details
  - Plan: **api**
  - Roles: **AuthGroup_TenantPartnerDirectoryConfigurator**
  - Grant-types: **Client Credentials**

## Setup Postman Environment

- Download the files

  - [CPI Tenant Template.postman_environment](<./CPI Tenant Template.postman_environment.json>)
  - [CPI Partner Directory.postman_collection.json](<./CPI Partner Directory.postman_collection.json>)

- Import the files into postman
- Set the **CPI Tenant Template** as the environment and provide the values from the service instance.

## Establish Authentication

- Establish authentication by running the request **Get Token**. This will set the environment variable `cpi_access_token` which is used to authenticate all other requests.

## Creating Partner and Parameters

- A Pid or Partner ID is created by creating a String Parameter. Using the noted request `Create String Parameters` create the following parameters

| Parameter                    | Request                  | Possible values                                         | Description                                                                                                                                                                                                                                                                         |
| ---------------------------- | ------------------------ | ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Pid                          | Create String Parameters |                                                         | The AS2 Partner ID of the customer                                                                                                                                                                                                                                                  |
| AS2_inbound_decrypt_message  | Create String Parameters | true, false                                             | does the incoming message require signature verification does the incoming message required decryption                                                                                                                                                                              |
| AS2_inbound_verify_signature | Create String Parameters | notRequired, trustedCertificate, trustedRootCertificate |
| AS2_sender_public_key        | Create Binary Parameters |                                                         | If parameter AS2_inbound_verify_signature is set to trustedCertificate or trustedRootCertificate in the Partner Directory, make sure that the Mendelson Public Key or the Root Certificate of it are uploaded as base64-encoded parameter SenderPublicKey in the Partner Directory. |
| Outbound_ProcessDirect_URL   | Create String Parameters |                                                         | this is the process direct url of the flow that will process the message                                                                                                                                                                                                            |

Example String request

```
{
    "Pid": "MyPID",
    "Id": "AS2_Inbound_Decrypt_Message",
    "Value": "true"
}
```

Example Binary request

```
{
    "Pid": "MyPID",
    "Id": "AS2_sender_public_key",
    "ContentType": "crt",
    "Value": "MIIDkDCCAnigAwIBAgIOAO...VNxcW4SwvUHIbSg=="
}
```

To convert a crt file to base64 use

```
openssl x509 -inform der -in key.cer -out key64.cer
```

After creating each parameter using the `Get String Parameters` request with the filter `?$filter=Pid eq 'MyPID'` to verify
