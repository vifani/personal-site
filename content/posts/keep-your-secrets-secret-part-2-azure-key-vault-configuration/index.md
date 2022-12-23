---
title: "Keep Your Secrets Secret: Part 2 - Azure Key Vault"
url: "keep-your-secrets-secret-part-2-azure-key-vault-configuration"
date: 2023-01-09T12:00:00.0000000Z
tags: ["Azure","DevOps","Secure DevOps","Secrets Management","Azure Key Vault","Developer Environment","Continuous Delivery","Continuos Integration","CI/CD","Configuration"]
draft: false
toc:
  enable: true
resources:
- name: "featured-image"
  src: "featured-image.png"
---
In this second post about secrets management I want to introduce a new place that we can use to safely store secrets: **Azure Key Vault**.

## Azure Key Vault ##
Azure Key Vault service is probably one of the safest place can be used to create, manage and store keys, secrets and certificates. And in fact is basically part of all solution architectures I have worked in the last 10 years.
Basically is a cloud service that encrypts all data stored using a software key (Standard tier) or a hardware security module(HSM)-protected key (Premium tier) and it helps to solve the following problems:
- Key Management: can be used to create and control encryption keys used by software solutions to encrypt/decrypt data
- Secrets Management: can be used to safely store credentials, subscription keys, shared keys, tokens, passwords and virtually any text based sensible information
- Certificate Management: can be used to provision, store, manage and deploy certificates used for almost all possible usages: TLS/SSL certificates, client credential authentication, TLS mutual authentication, and so on
Azure Key Vault is feature rich, helps to manage secrets/certificates expiration, to define access policies to carefully identity who/what can access keys/secrets/certificates, and can be easily integrated in a public or a private network using VNET integration and private endpoints. For more details about this amazing service take a look at the [official documentation](https://docs.microsoft.com/en-us/azure/key-vault/general/).

## Integration with the configuration ##
If we decide to adopt Azure Key Vault as secrets storage, we need to define also a strategy to access these secrets from inside our solution code. There are many ways to achieve this goal and in this article we will talk about Azure Key Vault **integration with the configuration**. The idea behind this approach is to use the .NET and .NET Framework configuration systems to access secrets from our code. In this way, simply switching the configuration providers configuration, we can combine this approach with the [User Secrets](https://vifani.com/keep-your-secrets-secret-user-secrets-part-1-developer-environment/) in order to use the Azure Key Vault as secrets storage when the solution runs in cloud environments and to use User Secrets as storage when the solution runs in the developer environment.

### Integration in .NET projects ###
.NET provides a configuration system based on providers and the Host builder default configuration automatically adds the following providers:
- Command-line configuration provider
- Environment Variables configuration provider
- User Secrets configuration provider (when app runs in Development environment)
- appsettings.Environment.json using JSON configuration provider
- appsettings.json using JSON configuration provider

In order to add the Key Vault configuration provider first we have to add the following NuGet packages:
- [Azure.Extensions.AspNetCore.Configuration.Secrets](https://www.nuget.org/packages/Azure.Extensions.AspNetCore.Configuration.Secrets)
- [Azure.Identity](https://www.nuget.org/packages/Azure.Identity)

Then we are ready to add the new provider to the builder. For ASP.NET projects the support is added as follows:
```
using Azure.Identity;

var builder = WebApplication.CreateBuilder(args);

if (builder.Environment.IsProduction())
{
    builder.Configuration.AddAzureKeyVault(
        new Uri($"https://{builder.Configuration["KeyVaultName"]}.vault.azure.net/"),
        new DefaultAzureCredential());
}
```

For .NET projects we can use the Host.CreateDefaultBuilder method as follows:
```
using Azure.Identity;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Configuration;

using IHost host = Host.CreateDefaultBuilder(args)
    .ConfigureAppConfiguration((hostBuilder, confBuilder) =>
    {
        if (hostBuilder.HostingEnvironment.IsProduction())
        {
            var config = confBuilder.Build();
            confBuilder.AddAzureKeyVault(
                new Uri($"https://{config["KeyVaultName"]}.vault.azure.net/"),
                new DefaultAzureCredential());
        }
    })
    .Build();
```

The shared code is a very simple example of Key Vault integration and considers the following assumptions:
- The Azure Key Vault configuration provider will be added only in environments different from developer environment. This is very important because if you expect to use Azure Key Vault as secrets storage also for the developer environments (it makes absolutely sense), you have to remove the if .IsProduction() line
- In the default configuration providers (usually in the appsettings.json file or in environment variables) a configuration called **KeyVaultName** is available in order to define the Azure Key Vault instance that will be used by the provider
- The authentication against the Azure Key Vault instance will be performed using the standard DefaultAzureCredential class. More details will follow

More details about the Key Vault configuration provider options can be found in the [official documentation](https://docs.microsoft.com/en-us/aspnet/core/security/key-vault-configuration)

### Integration in .NET Framework projects ###
Similarly to what we have done to integrate the User Secrets support in the .NET Framework configuration system, also to add the Key Vault as additional configuration provider we can leverage the [Microsoft.Configuration.ConfigurationBuilders](https://github.com/aspnet/MicrosoftConfigurationBuilders) project.
To achieve our goal we have to add a reference to the NuGet Package [Microsoft.Configuration.ConfigurationBuilders.Azure](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) and updating the web.config file as follows:
```
<Microsoft.Configuration.ConfigurationBuilders.SectionHandlers>
  <handlers>
    <add name="DefaultAppSettingsHandler" type="Microsoft.Configuration.ConfigurationBuilders.AppSettingsSectionHandler" />
    <add name="DefaultConnectionStringsHandler" type="Microsoft.Configuration.ConfigurationBuilders.ConnectionStringsSectionHandler" />
  </handlers>
</Microsoft.Configuration.ConfigurationBuilders.SectionHandlers
```
This first section adds the integration between the **appSettings** and **connectionStrings** sections and the configuration builders system.
```
<configBuilders>
  <builders>
    <add name="AzureKeyVault" vaultName="${KeyVaultName}" optional="true" type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Azure, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
  </builders>
</configBuilders>
```
This second section defines the configuration builders that we want to use. In this case we are adding only the Azure Key Vault support, and we specify the following parameters:
- **vaultName**: can be the name of the Key Vault we want to use, or a reference to a key of the appSettings using the annotation ${keyname}
- **optional**: we set it to true because we don't want the configuration system crashing if the Key Vault is not reachable for any reason
```
<appSettings configBuilders="AzureKeyVault">
  <add key="KeyVaultName" value="kv-contoso" />
</appSettings>
```
This last section links the Azure Key Vault configuration provider **appSettings** section and specify the value of the KeyVaultName key with the name of the Key Vault that will be used to load the configurations.

As seen for .NET configuration provider, also in .NET Framework Azure Configuration Builder the authentication against the Azure Key Vault instance will be performed using the standard DefaultAzureCredential class.

Microsoft Configuration Builders can be configured with different options in order to load configuration by multiple provider at one time. For example, I usually integrate it also with the EnvironmentConfigBuilder ([NuGet Package](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/)) in order to take the KeyVaultName from a configuration local to the environment (such as a ConfigMap in Kubernetes or an AppSetting in Azure App Service).

For more details about this library, take a look at the [official documentation](https://github.com/aspnet/MicrosoftConfigurationBuilders/blob/main/docs/KeyValueConfigBuilders.md).


### DefaultAzureCredential class ###
We have mentioned a couple of times that in order to load the configuration/secrets from an Azure Key Vault instance, we use the DefaultAzureCredential class for the authentication. What does it mean?

The DefaultAzureCredential class is part of the [Azure.Identity SDK](https://github.com/Azure/azure-sdk-for-net/tree/main/sdk/identity/Azure.Identity) and simplify the authentication implementation against many Azure services, Key Vault included. This is the standard authentication flow provided:

![DefaultAzureCredential Class Auth Flow](DefaultAzureCredentialAuthFlow.svg "DefaultAzureCredential Class Auth Flow")

As you can see, the flow is
1. **Environmental variables** (that means using client id and client secrets/certificate specified in well-known environment variables)
2. **Managed Identity** - If the application is deployed to an Azure host with Managed Identity enabled, the DefaultAzureCredential will authenticate with that account
![Azure Authentication in Visual Studio](VsLoginDialog.png "Azure Authentication in Visual Studio")
3. **Visual Studio** - If the developer has authenticated via Visual Studio, the DefaultAzureCredential will authenticate with that account.
4. **Visual Studio Code** - Currently excluded by default as SDK authentication via Visual Studio Code is broken due to issue #27263. The VisualStudioCodeCredential will be re-enabled in the DefaultAzureCredential flow once a fix is in place. Issue #30525 tracks this. In the meantime Visual Studio Code users can authenticate their development environment using the Azure CLI.
5. **Azure CLI** - If the developer has authenticated an account via the Azure CLI az login command, the DefaultAzureCredential will authenticate with that account.
6. **Azure PowerShell** - If the developer has authenticated an account via the Azure PowerShell Connect-AzAccount command, the DefaultAzureCredential will authenticate with that account.
7. **Interactive browser** - If enabled, the DefaultAzureCredential will interactively authenticate the developer via the current system's default browser. By default, this credential type is disabled.

In our use cases, I suggest taking the following paths: 
- when we work locally on developers machines (and we want to use Key Vault also in this scenario), we can use the Visual Studio / Visual Studio Code integration
- when the code is deployed on Azure, I suggest leveraging and enable the Managed Identity authentication (how to do this depends on the specific Azure service)
- when the code is not deployed on Azure, I suggest using the Environment variables integration defining the right variables and a Service Principal for the authentication

For more details on this topic, take a look at the [official documentation](https://github.com/Azure/azure-sdk-for-net/tree/main/sdk/identity/Azure.Identity).

## Conclusion
In this second article of the series **Keep Your Secrets Secret**, we have seen together our we can integrate safely the Azure Key Vault service in the configuration system of .NET and .NET Framework. 

The main advantages of this approach are that is quick and easy to implement (it requires only few code changes in most .NET projects) and can be used both in developer and production environments.