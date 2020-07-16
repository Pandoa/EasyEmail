# Easy Email - Quick start.
|Table of content|
|:---:|
|[Before you start](#before-you-start)|
|[Blueprint Examples](#blueprint-examples)|
|[C++ Examples](#markdown-header-c-examples)|
# Before you start
## Configuration
Easy Email allows you to connect to an SMTP server to send emails. You must use the server corresponding to the email address you want to use to send the email.
Here is a list of common SMTP servers:
|Mail extension|Server Type|Server Address|Security|Port|Requires Authentification|
|:---|:---:|---|---:|---:|:---:|
|@gmail.com|ESMTP|smtp.gmail.com|SSL/TLS|465|Yes|
|@gmail.com|ESMTP|smtp.gmail.com|StartTLS|587|Yes|
|@yahoo.com|ESMTP|smtp.mail.yahoo.com|SSL/TLS|465|Yes|
|@aol.com|ESMTP|smtp.aol.com|SSL/TLS|465|Yes|
|@hotmail.com|ESMTP|smtp.live.com|SSL/TLS|465|Yes|
|@outlook.com|ESMTP|smtp.live.com|StartTLS|587|Yes|

| :information_source: |  Gmail supports both SSL/TLS and StartTLS. You can just pick one of the two. |
|:--|:--|

| :exclamation: | Some SMTP server requires you to enable less secure apps to successfully connect.  |
|:--|:--|

## C++
### Includes
Easy Email has two files to include.
```cpp
#include "Email.h"        // Contains the UEmail class. 
#include "EmailLibrary.h" // Not required to send emails.
```
`Email.h` contains everything we need to send emails and `EmailLibrary.h` contains a few helper functions that you shouldn't need. They are used internally by `Email.h` for you but are available if needed.
### Functions
The functions are the same for Blueprints and C++. C++ however doesn't have the helper nodes that are Blueprints only.

## Additional notes



# Blueprint Examples
## Send an email with a Gmail account
# C++ Examples
