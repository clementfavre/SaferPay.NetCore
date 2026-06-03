# SaferPay.NetCore Json Api V1.52

This repository is an implementation of the [SaferPay.Net](https://github.com/bmbsqd/saferpay-net) library, with updates to use **.NET 8.0 / .NET 10.0** and **RestSharp** instead of HttpClient. All methods have been extended with sync and async calls.

The implementation is based on the latest version of the JSON API, **v1.52**, which can be found at the following URL: http://saferpay.github.io/jsonapi/#ChapterTransaction

You can find Test Cards and explanation of usage at: https://docs.saferpay.com/home/integration-guide/testing-and-go-live#visa-and-v-pay

### Test Pages

**Create Test Account**
```
https://test.saferpay.com/BO/SignUp
```

**Login Test Account**
```
https://test.saferpay.com/BO/Login
```

### What's New
+ Multi-targets `.NET 8.0` and `.NET 10.0`
+ HttpClient has been replaced by `RestSharp`
+ Updated to use the latest version of the JSON API, `v1.52`
+ Replaced `BaseUri` with `SandBox` mode. BaseUri is now generated based on SandBox mode for testing or live environments.
+ Updated and improved constructors for easier usage.
+ Added descriptions to Properties based on the API documentation.
+ Converted string properties to Enum values.
+ Added Examples and Test Console App in the Solution.
+ Added Interface Channels for ease of usage.
+ Added Extensions for the most used methods for direct use in the client.
+ Added `IsSuccess` and `Error` properties in ResultObject.
+ Updated all enum values, models, and interfaces.

### Methods
Implemented all methods:

+ **Payment Page Methods:** `Initialize`, `Assert`
+ **Transaction Methods:** `Initialize`, `Authorize`, `QueryPaymentMeans`, `AdjustAmount`, `AuthorizeDirect`, `AuthorizeReferenced`, `Capture`, `MultipartCapture`, `AssertCapture`, `MultipartFinalize`, `Refund`, `AssertRefund`, `RefundDirect`, `Cancel`, `RedirectPayment`, `AssertRedirectPayment`, `Inquire`, `AlternativePayment`, `QueryAlternativePayment`, `DccInquiry`
+ **Secure Card Data:** `Insert`, `AssertInsert`, `InsertDirect`, `Update`, `Delete`, `Inquire`
+ **Batch:** `Close`
+ **Omni Channel:** `InsertAlias`, `AcquireTransaction`
+ **Saferpay Management API:** `Licensing CustomerLicense`, `PaymentPageConfig GetConfigurations`, `SaferpayFieldsAccessToken CreateAccessToken`, `SaferpayFieldsAccessToken DeleteAccessToken`, `SecurePayGate Create SingleUsePaymentLink`, `SecurePayGate SingleUsePaymentLink`, `SecurePayGate Delete SingleUsePaymentLink`, `Terminal GetTerminal`, `Terminals GetTerminals`, `TransactionReporting GetTransactions`

### Global Settings and Usage (With Client Extensions)

Define Settings;
```csharp
SaferPay.Config.Settings.Default.Username = "ApiUserName";
SaferPay.Config.Settings.Default.Password = "ApiPassword";
SaferPay.Config.Settings.Default.TerminalId = "TerminalId";
SaferPay.Config.Settings.Default.CustomerId = "CustomerId";
SaferPay.Config.Settings.Default.SandBox = true;
```

Get Client Instance;
```csharp
ISaferPayClient Client = SaferPay.Config.Settings.Client();
```

Initialize request for Payment Page;
```csharp
string OrderID = "123456";

InitializePaymentPageRequest req = new InitializePaymentPageRequest();
req.TerminalId = TestConfig.TerminalId;
req.Payment = new Payment(123.45M, "TRY", OrderID);
req.ReturnUrl = $"{TestConfig.WebPage}payment-page?orderId={OrderID}";
```

Call Api Async;
```csharp
var result = await Client.InitializePaymentPageAsync(req);
if (result != null && result.IsSuccess)
{
    // Success
    Console.Write("Response Successful : ");
    Console.WriteLine(result.Json());
}
else if (result != null)
{
    // Failed
    Console.Write("Response Failed : ");
    Console.WriteLine(result.Error.Json());
}
else
{
    // Error
    Console.Write("Error !");
}
```

Call Api Sync;
```csharp
var result = Client.InitializePaymentPage(req);
if (result != null && result.IsSuccess)
{
    // Success
    Console.Write("Response Successful : ");
    Console.WriteLine(result.Json());
} else if(result != null)
{
    // Failed
    Console.Write("Response Failed : ");
    Console.WriteLine(result.Error.Json());
} else
{
    // Error
    Console.Write("Error !");
}
```
  

### Basic Usage With Interface Channels


Initialize the ApiClient;
```csharp
ISaferPayClient Client = new SaferPayClient("CustomerId", "TerminalId", "UserName", "PassWord", true);
```

Get Interface Channel to use, example based on Transaction;
```csharp
ITransaction payment = Client.Transaction;
```

Created Credit Card request;
```csharp
string OrderID = "123456";

InitializeRequest req = new InitializeRequest(TestConfig.TerminalId, 123.45M, "TRY", OrderID, $"{TestConfig.WebPage}transaction?orderId={OrderID}").SetCard("9010004150000009", 12, 30, "123", "Card Holder Name");
```

Call Api Async;
```csharp
var result = await payment.InitializeAsync(req);
if (result != null && result.IsSuccess)
{
    // Success
    Console.Write("Response Successful : ");
    Console.WriteLine(result.Json());
}
else if (result != null)
{
    // Failed
    Console.Write("Response Failed : ");
    Console.WriteLine(result.Error.Json());
}
else
{
    // Error
    Console.Write("Error !");
}
```

Call Api Sync;
```csharp
var result = payment.Initialize(req);
if (result != null && result.IsSuccess)
{
    // Success
    Console.Write("Response Successful : ");
    Console.WriteLine(result.Json());
} else if(result != null)
{
    // Failed
    Console.Write("Response Failed : ");
    Console.WriteLine(result.Error.Json());
} else
{
    // Error
    Console.Write("Error !");
}
```

### Tests

The solution ships with two projects under the repository root:

+ **`SaferPay.Tests`** : automated xUnit suite (`Unit` + sandbox `Integration` tests). Run it with:
  ```bash
  dotnet test SaferPay.Tests/SaferPay.Tests.csproj
  ```
  Integration tests run against `test.saferpay.com` using the public Viwo sandbox account by default. Override it with the `SAFERPAY_CUSTOMER_ID`, `SAFERPAY_TERMINAL_ID`, `SAFERPAY_USERNAME` and `SAFERPAY_PASSWORD` environment variables, or set `SAFERPAY_SKIP_INTEGRATION=1` for a fully offline run.
+ **`SaferPay.Test`** : interactive console playground that demonstrates the API end to end. See [`SaferPay.Test/README.md`](SaferPay.Test/README.md).

### Changelog

See [CHANGELOG.md](CHANGELOG.md) for the full version history.
