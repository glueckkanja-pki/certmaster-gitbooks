---
category: Configuration
title: appsettings.json
order: 2
---

# Appsettings

Configuration of most website and service related settings in JSON format. As a hierarchical data format, JSON allows to nest configuration data. In the topmost layer, the following settings are available:

* **PublicUrl**: URL to the service itself, used in links.
* **ConnectionStrings**: Currently contains only **DefaultConnection**, defines where the Certificate Master database resides.
* **Client**: Structure describing customer-specific tailoring, see [Section Client](appsettings.md#ClientConfig)
* **FileStorage**: Directory to store temporary files
* **Authorization**: DEPRECATED, moved to categories in Certificates
* [**CA**: Structure of Certification Authorities to be managed](appsettings.md#CAConfig)
* **StartPage**: Contains a section **Links** with a structured set of sections and named links. The same variables must be defined in each [LANGUAGE.json](https://github.com/glueckkanja-pki/certmaster-gitbooks/tree/43c614d05e1dc450020bcaaf35bffddbb53a56dc/docs/configuration/README.md#languagejson-website) to define what appears here as the text for the links.
* [**Mail**: Structure describing an SMTP service to send out mails](appsettings.md#SMTPConfig)
* **Certificates**: A list of certificate categories. Each [certificate category is itself a structure](appsettings.md#CertificateCategoryConfig)
* **CertificateLogging**: The substructure **Loggers** contains a list of [structures describing a logging channel](appsettings.md#LoggerConfig). This is an addition to the [log4net logging](https://github.com/glueckkanja-pki/certmaster-gitbooks/tree/43c614d05e1dc450020bcaaf35bffddbb53a56dc/docs/configuration/README.md#log4netconfig-all-components)
* **Logging**: More [logging configuration](appsettings.md#LoggingConfig)

## [Section Client](appsettings.md)

Structure describing customer-specific tailoring

* **DisplayName**: Readable name of the customer
* **AssemblyName**: DLL with customer specific tailoring code
* **LogoFile**: Relative to appsettings.json, a picture file of the customer's logo; displayed in the web application.
* **Custom**: Addtional configuration specific to the code in the customer's DLL.

## [Section CA](appsettings.md)

Structure of Certification Authorities to be managed

* **Identifier**: Reference to this CA in other sections
* **DisplayName**: Name of the CA as shown in the web application
* **Config**: Configuration string for Active Directory Certificate Services, i.e. _DNS Name\CA Name_. This is what `certutil -getconfig` prints out on the CA.
* **TimeZone**: The time zone of the CA server, as sometimes dates are returned in local time from the CA and must be converted.

## [Section Mail](appsettings.md)

Structure describing an SMTP service to send out mails

* **Host**: The DNS name of SMTP service
* **Port**: The port the SMTP service uses \(often 587 or 25\)
* **User**: If the SMTP service requires authentication, this is the username
* **Password**: If the SMTP service requires authentication, this is the password
* **From**: The sender name of the email

## [Section Certificate Category](appsettings.md)

Structure describing properties of category of certificates.

* **Authorization**: Defines who has which access to this category of certificates, detailed in [Section Authorization](appsettings.md#AuthorizationConfig)
* **ValidationConfiguration**: Path of the [policy engine configuration file](https://github.com/glueckkanja-pki/certmaster-gitbooks/tree/43c614d05e1dc450020bcaaf35bffddbb53a56dc/docs/configuration/README.md#policy-engine-configuration-file-all-components).
* **AddCnToSan**: Boolean value \(true/false\); if true, the value of the Field CN is automatically added as an additional DNS SAN entry for incoming requests.
* **Templates**: List of [structures describing a Microsoft AD Certificate template](appsettings.md#TemplateConfig)
* **Fields**: List of [structures, each of which describes a form field in the certificate request dialog](appsettings.md#FieldConfig)

### [Section Authorization](appsettings.md)

* **Module**: Name of the class in the DLL defined in the Client/AssemblyName section. This must inherit from `CertificateMaster.Common.Authorization.AuthorizationModule`.
* **Parameters**: Additional module-specific configuration.

You may use the pre-configured `BasicAuthorizationModule` that comes with Certificate Master without the need to develop a custom DLL. If you use `BasicAuthorizationModule` or a custom class that inherits from `BasicAuthorizationModule`, you can configure the following parameters:

* **RequesterGroup**: They can request certificates and manage the certificates they have requested.
* **AdminGroup** \(required\): They can do everything.
* **TrustcenterAdminGroup**: They can accept and reject pending certificate requests and they can revoke all certificates.
* **ViewAllGroup**: They can view all certificates, but without additional membership they cannot revoke them.

### [Template configuration](appsettings.md)

* **Identifier**: The name of the template \(machine readable variant\)
* **DisplayName**: The name displayed to the users of the web application. Can differ from what is configured in AD.
* **TargetCA**: List of strings that are identifiers of CAs configured in [the corresponding CA section](appsettings.md#CAConfig). Requesters may choose among those CAs to submit requests to.
* **Requestable**: Boolean \(true/false\). If true, requesters can choose this template for requests. If false, certificates of this template are displayed in the list, but new certificates of the template cannot be requested.
* **Manual**: DEPRECATED

### [Field configuration](appsettings.md)

* **Identifier**: Unique name identifying this field.
* **DisplayName**: Reference to a variable defined in [LANGUAGE.json](https://github.com/glueckkanja-pki/certmaster-gitbooks/tree/43c614d05e1dc450020bcaaf35bffddbb53a56dc/docs/configuration/README.md#languagejson-website), what name of the form field is shown to the requester?
* **Required**: Boolean \(true/false\). Can the requester leave the field empty \(false\) or must there be a value provided \(true\)?
* **DefaultValue** \(optional\): The field is pre-populated with this value.
* **ForceDefaultValue**: Boolean \(true/false\). The user cannot change the default value \(true\) or is it just a suggestion \(false\)?

The settings Required and ForceDefaultValue apply in addition to the validation defined in the [policy engine configuration file](https://github.com/glueckkanja-pki/certmaster-gitbooks/tree/43c614d05e1dc450020bcaaf35bffddbb53a56dc/docs/configuration/README.md#policy-engine-configuration-file-all-components). Use the configuration here where possible to provide a direct feedback to requesters. The policy engine will display an error message describing the problems with the request only after the requester has submitted the request, which is less user-friendly. It is possible to implement more sophisticated rules with the policy engine, though.

## [Section Logger](appsettings.md)

Description of a logging sink.

* **Type**: Possible values: _EventLogLogger_, _DatabaseLogger_, and _FileLogger_
* **Config**: If necessary, additional configuration depending on the type. A FileLogger needs a **FileName** with the path to the log file.

## [Section Logging](appsettings.md)

* **IncludeScopes**
* **LogLevel**: Subsetting **Default**

