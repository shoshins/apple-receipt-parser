# Project status
| Statistics        |
| ------------- |
| ![GitHub release](https://img.shields.io/github/v/release/shoshins/apple-receipt?label=Latest%20Release&sort=semver&style=for-the-badge)      |
| ![NuGet](https://img.shields.io/nuget/dt/Apple.Receipt.Models?label=.Models%20Downloads&style=for-the-badge)      |
| ![NuGet](https://img.shields.io/nuget/dt/Apple.Receipt.Parser?label=.Parser%20Downloads&style=for-the-badge)      |
| ![NuGet](https://img.shields.io/nuget/dt/Apple.Receipt.Verificator?label=.Verificator%20Downloads&style=for-the-badge)      |
| ![Build Status](https://img.shields.io/github/workflow/status/shoshins/apple-receipt/CI%20Build?label=Build%20Status&style=for-the-badge)      |
| ![Deploy Status](https://img.shields.io/github/workflow/status/shoshins/apple-receipt/Nuget%20Package%20Deploy?label=Deploy%20Status&style=for-the-badge)    |
| ![Code Quality](https://img.shields.io/sonar/quality_gate/apple-receipt?label=Sonar%20Quality%20Gate&server=https%3A%2F%2Fsonarcloud.io&style=for-the-badge) |
| ![Last Commit](https://img.shields.io/github/last-commit/shoshins/apple-receipt?label=Last%20Commit&style=for-the-badge)      |

# Nuget Packages Information

## Apple Receipt Models

### Description
Describes strongly-type representation of Apple Receipt Object.
[Apple Documentation](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ReceiptFields.html)

### Nuget information
[Link to package](https://www.nuget.org/packages/Apple.Receipt.Models/)

| Version | Downloads |
| ------------- | ------------- |
| ![Nuget](https://img.shields.io/nuget/v/Apple.Receipt.Models?style=for-the-badge) | ![NuGet](https://img.shields.io/nuget/dt/Apple.Receipt.Models?style=for-the-badge) |

### Installation:
* (Package manager): ```Install-Package Apple.Receipt.Models```
* (.Net CI): ```dotnet add package Apple.Receipt.Models```
* (Packet CLI): ```paket add Apple.Receipt.Models```

## Apple Receipt Parser

### Description
Parser for Apple Receipt that represented in base64 and encoded with ASN.1
[Anatomy of a Receipt payload encoded with ASN.1](https://www.objc.io/issues/17-security/receipt-validation/)

### Nuget information
[Link to package](https://www.nuget.org/packages/Apple.Receipt.Parser/)

| Version | Downloads |
| ------------- | ------------- |
| ![NuGet](https://img.shields.io/nuget/v/Apple.Receipt.Parser?style=for-the-badge) | ![NuGet](https://img.shields.io/nuget/dt/Apple.Receipt.Parser?style=for-the-badge) |

### Installation:
* (Package manager): ```Install-Package Apple.Receipt.Parser```
* (.Net CI): ```dotnet add package Apple.Receipt.Parser```
* (Packet CLI): ```paket add Apple.Receipt.Parser```

### How to use:
```cs

// Register DI services...
services.RegisterAppleReceiptParser();
...
// ... and resolve the service later.
IAppleReceiptParserService parserService;
...
// Get your base64 Apple Receipt
const string appleAppReceipt = "{receipt_base64_string}";
// Convert to Bytes
byte[] data = Convert.FromBase64String(appleAppReceipt);
// Get parsed receipt
AppleAppReceipt receipt = parserService.GetAppleReceiptFromBytes(data);
```

## Apple Receipt Verificator

### Description
Apple Receipt Validator using Apple App Store.
Two step verification: pre-validation that can be customized and App Store verification.
[Apple Receipt Validation with App Store documentation](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html)

### Nuget information
[Link to package](https://www.nuget.org/packages/Apple.Receipt.Verificator/)

| Version | Downloads |
| ------------- | ------------- |
| ![NuGet](https://img.shields.io/nuget/v/Apple.Receipt.Verificator?style=for-the-badge) | ![NuGet](https://img.shields.io/nuget/dt/Apple.Receipt.Verificator?style=for-the-badge) |

### Installation:
* (Package manager): ```Install-Package Apple.Receipt.Verificator```
* (.Net CI): ```dotnet add package Apple.Receipt.Verificator ```
* (Packet CLI): ```paket add Apple.Receipt.Verificator```

### How to use:
```cs
// (Optional) You can create implementation of custom validation process:
services.AddScoped<IAppleReceiptCustomVerificatorService, AppleReceiptCustomVerificatorService>();
...
// Fill settings:
services.RegisterAppleReceiptVerificator(x =>
{
    x.VerifyReceiptSharedSecret = "XXXX"; // Apple Shared Secret Key
    x.VerificationType = AppleReceiptVerificationType.Sandbox; // Verification Type: Sandbox / Production
    x.AllowedBundleIds = new[] {"com.mbaasy.ios.demo"}; // Array with allowed bundle ids
});
...
// ... and resolve the service later.
IAppleReceiptVerificatorService verificator;
...
AppleReceiptVerificationResult verificationResult = await verificator.VerifyAppleReceiptAsync(appleAppReceipt);

// OBSOLETE USAGE (Just for Backward Compatibity):
var verificationStatus = verificationResult.Status;
var verificationReceipt = verificationResult.Receipt;
var verificationMessage = verificationResult.Message;

// USAGE (Full Apple Response Info):
var verificationStatus = verificationResult.AppleVerificationResponse.StatusCode;
var verificationReceipt = verificationResult.AppleVerificationResponse.Receipt;
var verificationLatestReceiptInfo = verificationResult.AppleVerificationResponse.LatestReceiptInfo;
var verificationPendingRenewalInfo = verificationResult.AppleVerificationResponse.PendingRenewalInfo;
var verificationMessage = verificationResult.Message;

```
