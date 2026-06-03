# Changelog

All notable changes to SaferPay.NetCore are documented here.

## v1.52.02
+ Multi-targeted the library to `.NET 8.0` and `.NET 10.0`
+ Added the `SaferPay.Tests` xUnit project (unit + sandbox integration coverage)
+ Documented the `Transaction/QueryPaymentMeans` and `Transaction/AdjustAmount` methods

## v1.52.01
+ Updated library target framework to `.NET 8.0`
+ Removed unsupported `.NET 6.0` and legacy test runtime targets
+ Improved overall package compatibility with current and future .NET runtimes
+ Fixed recursive `Dispose()` implementation causing `StackOverflowException`
+ Fixed missing `_jsonSerializerSettings` initialization in the 4-argument constructor
+ Improved `RestClient` lifecycle management to prevent socket/resource leaks under load
+ Fixed exception rethrow handling to preserve original stack traces
+ Normalized `CreditCardExpiration` year handling across constructor, setters and parsing
+ Fixed `CreditCardExpiration.Parse()` producing invalid `MMYYYY` values instead of `MMYY`
+ Improved `CreditCardExpiration.ToString()` formatting consistency
+ Updated package metadata and NuGet packaging configuration
+ Improved GitHub Actions build, validation and NuGet publishing workflow
+ Updated package validation and build pipeline for `.NET 8`
+ General reliability, maintainability and runtime compatibility improvements

## v1.52
+ Updated to use the latest version of the JSON API, `v1.52`
+ Added `Saferpay Management API`.
+ Added `PaymentPage GetConfigurations` method to Saferpay Management API
+ Added `WITH_SUCCESSFUL_THREE_DS_CHALLENGE` as a valid value for the field Condition
+ Added `MastercardTLID` to the `IssuerReference` container in the response
+ removed `PayerId` from the `PayPal` container

## v1.51
+ Updated to use the latest version of the JSON API, `v1.51`
+ added `PAYPAL` as valid value for field Type in `Alias/Insert`
+ added `ONLINE_CHALLENGED` as valid value for field Type of container `Check` in `Alias/Insert`
+ removed `OK_AUTHENTICATED` as valid value from field `Result` of container `CheckResult`.
+ removed `INVOICE` as valid value from `PaymentMethods` in `PaymentPage/Initialize`
+ added fields `Authenticated` and `AuthenticationType` to container `AuthenticationResult`. Removed field `Result` from container in return.
+ added field `FundingSource` to container `Card`
+ field `CountryCode` in container `ForeignRetailer` is now mandatory.

## v1.50
+ Updated to use the latest version of the JSON API, `v1.50`
+ Added value `ONLINE_STRONG` to Type in the `Check` container and added new container `ExternalThreeDS` in `Alias/InsertDirect`
+ Added `GIFTCARD` as valid value for the field `PaymentMethods`
+ Introduced a new function to provide Dynamic Currency Conversion (`DCC`) inquiry details for your customer: `Transaction\DccInquiry`
+ The payment methods `GIROPAY`, `PAYDIREKT`, `SOFORT` and `WLCRYPTOPAYMENTS` are no longer supported.
+ `Transaction/AuthorizeDirect` is extended with the new subcontainer `DCC`, which references the response from `Transaction/DccInquiry` and payer's decision whether he accepts or declines `DCC` offer
+ Added `WERO` as valid value for the field `PaymentMethods`
+ Added `HolderName` and `IBAN` to the BankAccount container in `PaymentPage/Assert`
+ `Transaction/RefundDirect` is extended with the new subcontainer `BankAccount`. This is a required container for PostFinance Instant Payout

## v1.46
+ Updated to use the latest version of the JSON API, `v1.46`
+ Added new subcontainer `ExternalThreeDS` to container `Authentication`. This affects the following requests: `Transaction/AuthorizeDirect`
+ Updated `AuthorizeDirect` method to use the new `ExternalThreeDS` subcontainer.

## v1.45.01
+ Added `REKA` as alternative payment method to `PaymentPagePaymentMethods`.
