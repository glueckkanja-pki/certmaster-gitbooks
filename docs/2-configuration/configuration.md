---
category: Configuration
title: Configuration Overview
order: 1
---

The website and service components load their configuration from `%ProgramData%\CertificateMaster`. The policy module loads its configuration from `%ProgramData%\GKPolicyModule`. Which files reside in this location?

appsettings.json (website and service)
--------------------------------------

See [main article](../appsettings) for detailled information.

Configuration of most website and service related settings in JSON format, especially

- Connection to the SQL database
- CAs
- SMTP relay
- Categories of certificates with
 - path to a policy engine configuration XML file (like configuration.xml)
 - certificate templates recognized by the system
 - form fields displayed on certificate requests
- Additional logging

LANGUAGE.json (website)
-----------------------

These files must be added for any display langauge that the website should support. Its name is the two-letter-short name for the language like de for German and en for English with the extension .json.

The content is in standard JSON format and contains string values for all placeholders that should contain customized strings to display on the website.

**Note:** Even if you do not want to customize the appearance of the website, an empty language configuration is necessary for each language you want to support.

log4net.config (all components)
-------------------------------

This is a [standard log4net configuration file]([https://logging.apache.org/log4net/release/manual/configuration.html): Between `<log4net>` start and end tags, you configure appenders ([see examples for appenders](https://logging.apache.org/log4net/release/config-examples.html)) and loggers, especially the root logger. You can also just copy the default log4net configuration.

Policy engine configuration file (all components)
-------------------------------------------------

Multiple of these XML files can be configured for the website and service in `appsettings.json`. For the policy module, the name is always `configuration.xml`.

Between the `<gk-policy-configuration>` tags, there are three kind of tags:

- Each `<cert-template-configuration>` configures one ruleset for a set of templates.
- `<messages>` specify possibly outcomes of the evaluation that shall map to fixed HRESULT codes.
- `<rules>` specify validation rules.

#### Rules

Each rule can result in "accept", "deny", "pending", or "nothing". A list of rules means all rules will be checked. "deny" has precedence over "pending", "pending" has precendence over "accept", and "accept" has precedence over "nothing". If a template check results in nothing (i.e. all rules return nothing), it will default to "deny".

The following rules are predefined:

- `accept`
- `deny`
- `pending`

These rules return what they say without further checks.

Other rules need to be defined in the rules specification with the `<rules>` tags. Each rule has a type that specifies how it checks a request, a name by which it is referenced, and a specification of the results of the evaluation. Most types of rules also have subject-parts that they check for conformity. The following types exist:

##### rule-subject-regex

##### rule-subject-all-parts

##### rule-subject-count

##### rule-subject-setter

##### rule-key-algorithm

##### rule-key-length

Email templates (website and service)
-------------------------------------

The subdirectory `Templates` contains templates for emails that Certificate Master sends out. Currently, there are the two use cases `AcceptManualRequest` and `RejectManualRequest`. These determine the content of email for certificate requests sent to requesters after a Certificate Officer accepted or rejected the request, respectively. For each use case, there exist at least the following three files:

| File name               | Content                                                  |
|-------------------------|----------------------------------------------------------|
| **USECASE.subject.txt** | This is the text in the subject line of the email        |
| **USECASE.txt**         | The email body when displayed in text format             |
| **USECASE.html**        | The email body when HTML format is allowed               |

#### Images in HTML mails

You may include images in HTML mails. The image file must reside in the same directory as the other template files or in a subdirectory. You can reference them with an image tag.

Example: `<img src="logo.png" />` in AcceptManualRequest.html and a file logo.png next to it.

#### Placeholders

You can insert placeholders into the templates for the plain and HTML body of the email. These placeholders will be replaces by actual variables when the email is sent. Surround the placeholder names with curly braces in the template. The following placeholders are possible:

| Placeholder name        | Meaning                                                  |
|-------------------------|----------------------------------------------------------|
| DisplayName             | Name of the requester                                    |
| Mail                    | The email address of the requester                       |
| URL                     | Download link for the issued certificate                 |
| Subject                 | Subject field of the issued certificate                  |