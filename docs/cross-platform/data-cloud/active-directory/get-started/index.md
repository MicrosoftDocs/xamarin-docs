---
title: "Azure Active Directory"
description: "This document describes the steps that must be followed to allow a mobile app to authenticate with Azure Active Directory."
ms.service: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
no-loc: [Objective-C]
---

# Azure Active Directory

_Register an app to use Azure Active Directory_

Azure Active Directory allows developers to secure
  resources such as files, links, and Web APIs
  using the same organizational account that
  employees use to sign in to their systems or check their emails.

Developing mobile applications which can authenticate
  with Azure Active Directory involves three steps.
  The first two steps are generally the same, regardless
  of what services you plan to use. The third step is different
  for each service-type:

  1. [Registering with Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) on the *windowsazure.com* portal, then
  2. [Configure services](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Develop mobile apps using the services.

Examples of different services you can access include:

- [Graph API](~/cross-platform/data-cloud/active-directory/graph.md)
- Web API
- Office365

## Conclusion

Using the steps above you can authenticate your mobile apps against
  Azure Active Directory. The [Microsoft
  Authentication Library (MSAL)](/azure/active-directory/develop/msal-overview) makes it much
  easier with fewer lines of code, while keeping most
  of the code the same and thus making it shareable across platforms.

## Related Links

- [Microsoft NativeClient sample](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
