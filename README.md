
# [Easy Email](https://www.unrealengine.com/marketplace/en-US/product/easy-email) - Quick start.
|Table of content|
|:---:|
|[Before you start](#before-you-start)|
|[Blueprint Example](#blueprint-example)|
|[C++ Example](#c-example)|
|[Support](#support)|
## Before you start
### Configuration
Easy Email allows you to connect to an SMTP server to send emails. You must use the server corresponding to the email address you want to use to send the email.
Here is a list of common SMTP servers:
|Email extension|Server Type|Server Address|Security|Port|Requires Authentication|
|:---|:---:|---|---:|---:|:---:|
|@gmail.com|ESMTP|smtp.gmail.com|SSL/TLS|465|Yes|
|@gmail.com|ESMTP|smtp.gmail.com|StartTLS|587|Yes|
|@yahoo.com|ESMTP|smtp.mail.yahoo.com|SSL/TLS|465|Yes|
|@aol.com|ESMTP|smtp.aol.com|SSL/TLS|465|Yes|
|@hotmail.com|ESMTP|smtp.live.com|SSL/TLS|465|Yes|
|@outlook.com|ESMTP|smtp.live.com|StartTLS|587|Yes|

| :information_source: |  Gmail supports both SSL/TLS and StartTLS. You can just pick one of the two. |
|:--|:--|

| :warning: | Some SMTP servers require you to enable less secure apps to successfully connect. For Gmail, it's [Here](https://myaccount.google.com/lesssecureapps).|
|:--|:--|

### C++
#### Includes
Easy Email has two files to include.
```cpp
#include "Email.h"        // Contains the UEmail class. 
#include "EmailLibrary.h" // Not required to send emails.
```
`Email.h` contains everything we need to send emails and `EmailLibrary.h` contains a few helper functions that you shouldn't need. They are used internally by `Email.h` for you but are available if needed.
#### Functions
The functions are the same for Blueprints and C++. C++ however doesn't have the helper nodes that are Blueprints only.

### Additional notes
#### Additional logging
If you want additional logs, you have to enable `Verbose` and/or `VeryVerbose` logs.
To do so, open `MyGame/Config/DefaultEngine.ini` and add the following:
```ini
[Core.Log]
LogEasyEmail=VeryVerbose
```

#### Passwords
If the SMTP server requires authentication, you should not send passwords without using an encrypted connection (SSL/TLS or StartTLS). 
| :information_source: |  Passwords aren't printed to logs. |
|:--|:--|

#### Attachments
If you add an attachment as a file to an email, the file  is only loaded outside of the game thread just before sending the email  and is then cached within the email. If you let the MIME-Type of the attachment empty, Easy Email will try to match the file's extension with its default MIME-Type and use `application/octet-stream` if none is found or the extension is unknown.

## Blueprint Example
### Send an email with a Gmail account
Sending an email is really easy and straightforward with the helper nodes:

![Send email Gmail example](https://github.com/Pandoa/EasyEmail/blob/master/Images/SendEmailGmail.png?raw=true)
### Send an email with a custom SMTP server
An helper node for custom SMTP servers exists too:

![Send Email with Custom SMTP](https://github.com/Pandoa/EasyEmail/blob/master/Images/SendEmailCustom.png?raw=true)

| :information_source: |  All helper nodes use `UTF-8` and `text/plain` for the content encoding. If you want to use another charset or MIME-Type, you can't use the helper node at the moment and have to customize your email as showed below. |
|:--|:--|
### Customized email
If you need to customize the email, the same functionalities are offered for Blueprints and C++. However, it is longer and more complicated than the helper nodes:

![Send Email Customized](https://github.com/Pandoa/EasyEmail/blob/master/Images/SendEmailWithoutHelper.png?raw=true)
## C++ Example
### Send an email with a Gmail account
##### MyClass.h
```cpp
#pragma once

#include "Email.h"
#include "CoreMinimal.h"
#include "MyClass.generated.h"

UCLASS()
class UMyClass : public UObject
{
    GENERATED_BODY()
public:
    void SendEmail();
    
    UFUNCTION()
    void OnEmailError(const int32 ErrorCode)
    {
        // If the email failed to send, additional Error / Warning are available
        // in the output log.
        UE_LOG(LogTemp, Error, TEXT("Failed to send email. Code: %d."), ErrorCode);
    }
    UFUNCTION()
    void OnEmailSent()
    {
        UE_LOG(LogTemp, Log, TEXT("Email sent.");
    }
private:
    UPROPERTY()
    UEmail* Email;
};
```
##### MyClass.cpp
```cpp
#include "MyClass.h"

void UMyClass::SendEmail()
{
    Email = UEmail::CreateEmail();
    
    // Server config
    Email->SetServerAddress (TEXT("smtp.gmail.com"));
    Email->SetServerPort    (465);
    Email->SetConnectionType(ESmtpConnectionType::SSL);
    Email->SetServerType    (ESmtpServerType::ESMTP);
    
    // Username is by default your account's email address
    // Note that storing your password in plain text is generally 
    // not a good idea and is used here as an example.
    Email->SetCredentials(TEXT("MyUsername"), TEXT("MyPassword"));
    
    // Email targets
    Email->SetSender         (TEXT("myemail@gmail.com"));
    Email->AddReceiver       (TEXT("target@gmail.com"));
    Email->AddBlindCopyCarbon(TEXT("bcc@gmail.com"));
    Email->AddCopyCarbon     (TEXT("cc@gmail.com"));
    
    // Email content
    Email->SetSubject       (TEXT("Hello From Unreal"));
    Email->SetContent       (TEXT("Hello there, this is the Easy Email Plugin."));
    Email->SetContentCharset(EEmailCharset::utf_8);
    
    // Attachments
    Email->AddFileAsAttachment(TEXT("MyLogo.png"),    TEXT("C:/Users/Me/Logo.png"));                                     // Auto-detect MIME-Type
    Email->AddFileAsAttachment(TEXT("MyRawData.bin"), TEXT("C:/Users/Me/binary.bin"), TEXT("application/octet-stream")); // Explicit MIME-Type
    
    // Callbacks
    Email->OnEmailError.AddDynamic(this, &UMyClass::OnEmailError);
    Email->OnEmailSent .AddDynamic(this, &UMyClass::OnEmailSent);
    
    // Finally send the email
    Email->Send();
}
```

## Support
If you need help, have a feature request or experience troubles, please contact us at [pandores.marketplace@gmail.com](mailto:pandores.marketplace+EasyEmail@gmail.com?subject=Easy%20Email%20-%20).
