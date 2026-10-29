# HelloID-Conn-SA-Full-Microsoft-EntraID-AccountDelete

| :information_source: Information |
| :--- |
| This repository contains connector and configuration code only. The implementer is responsible for obtaining the required connection details, certificates, and permissions. Contact the client's application manager to coordinate connector requirements. |

## Description

This delegated form is designed for use with HelloID Service Automation (SA). It can be imported into HelloID and customized according to your requirements.

The form deletes selected disabled users from Microsoft Entra ID. The following options are available:

1. Search for and select a disabled Entra ID user.
2. Confirm the selected user.
3. Delete the user from Entra ID.

## Getting started

### Requirements

#### App Registration and certificate setup

Configure a Microsoft Entra ID App Registration for application (app-only) access to Microsoft Graph. Configure a certificate for authentication.

For guidance, see Microsoft's documentation on [app-only authentication with a certificate](https://learn.microsoft.com/en-us/powershell/exchange/app-only-auth-powershell-v2?view=exchange-ps#set-up-app-only-authentication).

#### HelloID-specific configuration

Grant admin consent for the Microsoft Graph application permission:

- `User.ReadWrite.All` to list users and delete the selected user.

This permission grants broad access to user profiles in the tenant. Review the permission with your Entra ID administrator before granting consent.

### Connection settings

Configure these HelloID global variables when importing and setting up the delegated form:

| Setting                          | Description                        | Mandatory |
| -------------------------------- | ---------------------------------- | --------- |
| `EntraIdTenantId`                | The Microsoft Entra ID tenant ID   | Yes       |
| `EntraIdAppId`                   | The App Registration client ID     | Yes       |
| `EntraIdCertificateBase64String` | The Base64-encoded app certificate | Yes       |
| `EntraIdCertificatePassword`     | The certificate password           | Yes       |

### Setup options

Use [All-in-one setup/createform.ps1](All-in-one%20setup/createform.ps1) to create the delegated form, data source, task, and required global variables through the HelloID API. The script assumes these resources do not already exist; it does not provide resource versioning or source control.

Follow the [HelloID Service Automation GitHub resources documentation](https://docs.helloid.com/hc/en-us/articles/360017556559-Service-automation-GitHub-resources) to run the setup script.

## Remarks

### User lookup and deletion

- The data source retrieves users from Microsoft Graph and returns only accounts where `accountEnabled` is `false`.
- The form displays the user's display name, UserPrincipalName, account status, job title, department, and company.
- The task deletes the selected account using its UserPrincipalName. Deletion is permanent from the tenant's active users and can only be reversed by restoring the user during Microsoft's retention period.

### Certificate-based authentication

The connector uses the configured Base64-encoded certificate and certificate password to authenticate the app with Microsoft Graph. The tenant ID and client ID identify the Entra ID tenant and App Registration.

## Development resources

### API endpoints

| Endpoint           | Description                                                              |
| ------------------ | ------------------------------------------------------------------------ |
| `/v1.0/users`      | Search and retrieve users; the data source filters for disabled accounts |
| `/v1.0/users/{id}` | Delete the selected user                                                 |

### API documentation

- [List users](https://learn.microsoft.com/en-us/graph/api/user-list)
- [Delete user](https://learn.microsoft.com/en-us/graph/api/user-delete)

## Getting help

For more information on delegated forms, see the [HelloID documentation](https://docs.helloid.com/en/service-automation/delegated-forms.html). You can also ask questions in the [HelloID forum](https://forum.helloid.com/forum/helloid-connectors/service-automation/214-helloid-sa-azure-ad-remove-inactive-user).

## HelloID docs

The official HelloID documentation can be found at [docs.helloid.com](https://docs.helloid.com/).