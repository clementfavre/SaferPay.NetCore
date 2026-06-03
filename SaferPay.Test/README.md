# SaferPay.NetCore Test & Examples (Json Api V1.52)

This folder is the **interactive console playground** for [SaferPay.NetCore](../README.md),
built against the JSON API **v1.52** and targeting **.NET 8.0 / .NET 10.0**.

It is meant for exploring the library by hand, not as the automated test suite.
For unit and integration tests, see the [`SaferPay.Tests`](../SaferPay.Tests) project.

> For the full API reference, supported methods and usage snippets, read the
> [root README](../README.md). This page only documents how to run the test artifacts.

## Console playground (`SaferPay.Test`)

A small console app that wires up an `ISaferPayClient` against the SaferPay sandbox
and lets you trigger requests from a menu (Payment Page, Transaction, Secure Card Data, …).
The bundled `success.html` / `failed.html` pages stand in for return URLs.

Run it from the repository root:

```bash
dotnet run --project SaferPay.Test
```

Sandbox credentials live in [`TestConfig.cs`](TestConfig.cs) and point at the public
Viwo test account (`test.saferpay.com`). Replace them with your own to test a real account.

You can find test cards and usage details at
<https://docs.saferpay.com/home/integration-guide/testing-and-go-live#visa-and-v-pay>.

### Test pages

| Purpose | URL |
| --- | --- |
| Create test account | <https://test.saferpay.com/BO/SignUp> |
| Login test account | <https://test.saferpay.com/BO/Login> |

## Automated tests (`SaferPay.Tests`)

The [`SaferPay.Tests`](../SaferPay.Tests) xUnit project holds the real test suite:

+ **Unit** : request/response building, routing, extensions, settings, value types (offline).
+ **Integration** : exercises the live HTTP pipeline against the SaferPay sandbox.

Run everything from the repository root:

```bash
dotnet test SaferPay.Tests/SaferPay.Tests.csproj
```

Integration tests use the public Viwo sandbox account by default. Override or skip them
with environment variables:

| Variable | Purpose |
| --- | --- |
| `SAFERPAY_CUSTOMER_ID` | Override sandbox Customer Id |
| `SAFERPAY_TERMINAL_ID` | Override sandbox Terminal Id |
| `SAFERPAY_USERNAME` | Override sandbox API username |
| `SAFERPAY_PASSWORD` | Override sandbox API password |
| `SAFERPAY_SKIP_INTEGRATION` | Set to `1` to skip integration tests (fully offline run) |
