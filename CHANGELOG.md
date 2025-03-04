# Version 0.12.0-alpha.1

## Breaking Changes
- Updated to a newer version of the Stripe API.

## Features
- The `app-info` can now be set/customized with the `Client::set_app_info` method.

# Version 0.11.0 (September 11, 2019)

## Features
- The `stripe-version` header can now be set/customized with the `Client::with_headers` method.

## Improvements
- The `Default` trait is now derived for all structs that have only defaultable parameters.
- `Webhook::construct_event` takes payload, signature, and secret args by ref (`&str`) instead of value.
- The `payment_method` and `confirmation_method` fields were added to `PaymentIntentCreateParams`.
- The `payment_method_details` structure now supports the `card` data field.

## Fixes
- Boolean fields that may be `null`, are now `Option` instead of `serde(default)`. 
- The `Customer` variant was added to the `EventObject` enum. 
- The `PaymentIntentCaptureParams` are now correctly `pub`.
- Fixed parsing a negative customer `account_balance`.

# Version 0.10.5 (July 25, 2019)

## Breaking Changes
- The `PaymentSourceParams` struct was edit to only include a `TokenId` and `SourceId` as per improvements to the API.
- The `charge.source` field to create a `Charge` was fixed to accept the new struct `ChargeSourceParams`.

# Version 0.10.4 (July 12, 2019)

## Improvements
- Added `id` field to `Event`

## Fixes
- Fixes a regression deriving `Eq` and `PartialEq` for `PaymentIntent` enums.

# Version 0.10.3 (June 21, 2019)

## Fixes
- Fixed deserialization of `CheckoutSessionCompleted` events.
- Added missing `Invoice` events.

# Version 0.10.2 (June 13, 2019)

## Fixes
- Fixed the `InvoiceLineItemId` type to correctly handle subscription items.

# Version 0.10.0 / 0.10.1 (June 4, 2019)

This version uses code generation to generate the stripe API according to the openapi spec.

This contains major breaking changes from previous versions, both
because many structures were out of date and because some changes
were made to make things easier to generate.

## New Features
- Many new APIs have now been implemented (mostly CRUD); some requests are still missing
  because not all requests are automatically implemented.  All previously implemented
  requests are still implemented (even if they couldn't be code-generated).

- Many requests now take the `expand` parameter which controls whether more data should
  be returned for `Expandable<T>` fields.  Previously these fields were just id types.

## Improvements
- Fields and requests have descriptive documentation rather than just referring to the stripe API docs.
- Ids and fields are more strictly typed (newtypes vs raw strings).
- Ids no longer need to be allocated and are cheaply clonable (in _almost_ all cases).
- Enum types now implement `Display` and `AsRef<str>`.

# Version 0.9.2 (April 7, 2019)

## Fixes / Improvements
- Add missing `product` to `Plan`.
- Add missing `receipt_url` to `Charge`.
- The deprecated `closed` field in `Invoice` is now optional.
- Fix allowing the `description` field in `Payout` to be optional.
- Fix allowing a `SubscriptionItem` quantity to be `None` (e.g. when using metered pricing API).
- Fix signature checking for received Webhooks.
- Allow the timestamp to be manually specified when verifying webhook signature.

# Version 0.9.1 (Feb 4, 2019)

## Fixes
- Detach source data structure is a new `DetachedSource` enum
- The `object` field was removed from `PaymentIntent`, `Refund` and `Payout`.
- Fields in `PaymentIntent` were updated to correctly be optional


# Version 0.9.0 (Jan 17, 2019)

## New Features
- Added an `async` feature flag which removes `stripe::Client` and adds `stripe::async::Client`.

## Breaking Changes
- The `stripe::Params` typed was renamed to `Headers` to avoid confusion
  with other `FooParams` types and the `params` module.

# Version 0.8.0 (Jan 15, 2019)

## New Features
- Added `verify_bank_account` to `Customer` resource.
- Added BankAccount as a variant of `PaymentSource`.
- Add the [PaymentIntents](https://stripe.com/docs/payments/payment-intents) resources, apis and events.

## Breaking Changes
- Minimum Rust version required is 1.31.1.
- Moved `retrieve_source` and `detach_source` to `Customer` resource from `Source`.
- The `Error::Conversion` enum variant has been replaced by `Error::Serialize` and `Error::Deserialize`.
- See also __Fixes / Improvements__.

## Fixes / Improvements
- TokenId has become an enum to account for different Token options
- Updated fields on `BankAccount` resource so all are correct.
- The `Payout` and `Refund` types now use enums instead of Strings.
- The `WebhookError` type is now externally public.

# Version 0.7.2

## Fixes

- Fix sending `metadata` (and other nested params) as part of a POST request;
  e.g. `Customer::create`, `Customer::update`, etc.

# Version 0.7.0 and 0.7.1

## Fixes

- The type of `Customer.default_source` has changed from `SourceId` to `PaymentSourceId`.
- The type of `Customer.sources` has changed from `List<Source>` to `List<PaymentSource>`.

# Version 0.6.0

## New Features / Improvements

- The `List<T>` type has new `next` and `get_all` methods for pagination.
- Struct types more consistently implement `Clone`.
- Enum types more consistently implement `Copy`, `Eq`, and `Hash`.
- More source details (e.g. `ach_credit_transfer`, etc) were added to `Source`.
- The blocking api is now backed by `reqwest` instead of `hyper:0.10` so that
  the library is no longer pinned to outdated dependencies.
- Added the "webhooks" feature flag for processing events (enabled by default)
  to convert `hmac` and `sha2` to optional dependencies.

## Breaking Changes

- (Mostly from 0.5.x series) `Charge`, `Card`, and `Source` have been updated
  to use new enum values rather than `String`.
- The `SourceType` enum used by **ChargeListParams** has been renamed to
  `SourceFilterType` to remove a conflict with the `SourceType` enum used by
  the type field of **Source**.

## Fixes

- All `Address` fields have been updated to be optional.
- The `Client-Id` param has been added to params to make support for
  impersonating **Connect Accounts** compatible with newer stripe api versions.

# Version 0.5.3

## Changes

- Updated `Charge` to use enum types rather than `&str` where possible.

# Version 0.5.2

## Changes

- Fixed many issues with the `Source` object.
- Updated `Card` to use enum types rather than `&str` where possible.
- Added many new fields to `Card`.

# Version 0.5.1

## Changes

- Bumped dependency versions

# Version 0.5.0 (Sep 26, 2018)

## New Features

- Implemented the [sources resource](https://stripe.com/docs/api#sources) and
  added the `Source` type .

## Breaking Changes

- The `CustomerSourceParams` struct was renamed to `PaymentSourceParams`.
- The `Source` enum was renamed to `PaymentSource` (there is a new `Source` struct).
- Source and token fields (like `token: &str`) now use `source: SourceId` or `token: TokenId` instead.

## General Improvements

- Added missing `default_source` field to `Customer`.
- The `Deserialize` and `Serialize` traits have now been implemented for all param and resource structs.
- The `Copy`, `Clone`, `Eq`, `PartialEq`, and `Hash` traits have been derived for flat enum types.

# Version 0.4.7 (Jun 11, 2018)

## Changes

- Fix, adds all missing error code variants to `stripe::error::ErrorCode`.

# Version 0.4.6 (Jun 9, 2018)

## Changes

- Fixed field `nickname` of `Plan` can be null.

# Version 0.4.5 (Feb 20, 2018)

## Changes

- Fixed field `nickname` of `Plan` should be `name`

# Version 0.4.4 (Jan 4, 2018)

## Breaking Changes

- The `balance_transaction` field of `Charge` is now `Option<_>`.

# Version 0.4.2 (August 20, 2017)

## Changes

- Added basic crate documentation
- Added the `invoice.upcoming` event

# Version 0.4.1 (August 8, 2017)

## Changes

- Add RangeQuery struct for created/date filters in list requests
- Implement `Customer::list` request (ie. "GET /customers"); Thanks @pocket7878
- Add missing params to `InvoiceListParams` for `Invoice::list`

# Version 0.4.0 (August 2, 2017)

## Breaking Changes

- Upgrade Serde to v1.0

# Version 0.3.3 (August 1, 2017)

## Changes

- Add support for authentication using the Stripe-Account header
- Implement `Event` struct for implementing a Stripe Webhook (only some events)
- Implement `Invoice::update` request (ie. POST "/invoices/{invoice_id}")
- Implement `InvoiceItem::create` request (ie. POST "/invoiceitems")
- Fix `Subscription::cancel` request (previously used wrong method/path)

# Version 0.3.2 (June 17, 2017)

## Changes

- Implemented the `Invoice::list` API (ie. "/invoices")
- Fixed a request encoding error for `Invoice::pay` (ie. "/invoices/{invoice_id}/pay")
- Fixed fields on `Subscription` that could be null but were missing `Optional<..>`
- Added missing `last4` field to `Card` resource
- Changed the repository URL

# Version 0.3.1 (Mar 31, 2017)

## Changes

- Made rust docs slightly easier to read
- Fixed incorrect method name for `Plan` resource
- Use `native-tls` by default and add feature-flag supporting `openssl`

# Version 0.3.0 (Mar 28, 2017)

## New Features

- Implemented the [charges resource](https://stripe.com/docs/api#charges) and
  added the `Charge` type.

## Breaking Changes

- The `Resource::get` requests have been renamed to `Resource::retrieve`.
- The `Subscription::cancel` request now expects `CancelParams` instead of
  expecting a one-off function argument.
- Removed the `blame` method and `Blame` type from errors.
- The `Error::Decode` and `Error::Encode` variants have been combined into a
  single `Error::Conversion` variant.

## General Improvements

- The `Error` type now has easier to read error messages and will parses JSON
  errors returned by Stripe into a structured error type (`RequestError`).
- Added Travis CI for automated testing on GitHub.
- Added reference documentation to implemented resources (but still no docs at
  crate/module level nor a "Getting started" guide)

# Version 0.2.0 (Mar 17, 2017)

## New Features

- Added `Client` which is created once with a stripe private key and is
  intended to be re-used for multiple requests. It implements `Sync` so you
  can share it among multiple threads.
- Added new strongly-typed `Currency` type following the example of
  https://github.com/stripe/stripe-go.
- `Customer::create`/`CustomerParams` now support using `CardParams` as a
  source instead of just tokens.
- Implemented the [sources API](https://stripe.com/docs/api#sources) in the `Source` type.

## Breaking Changes

- All `Params` types now use `&str` fields instead of `String`s.
- Requests used to require a `stripe_key: &str` as their final argument but
  now use a `&Client` as the first argument instead.
- Stripe tokens used to be directly used as a source in `CustomerParams` but
  now must be used with `CustomerSourceParams::Token("tok_xyzABC123")`.

## General Improvements

- Added remaining fields to `Params` types after switching from
  `serde_urlencoded` to `serde_qs` to support nested params.
- Added the `create_customer` example
- Types that implement `Deserialize` now also implement `Debug`.
- Types that implement `Serialize` now also implement `Default`.
