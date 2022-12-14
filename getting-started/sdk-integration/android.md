---
description: This guide describes how to add and use Apphud SDK to your Android app.
---

# Android

## Features

Currently we support the following features for Android:

* Only Google Play Store is currently supported
* Auto-renewing subscriptions validation and renewals tracking
* One time purchases validation
* Refunds / Revokes tracking
* Grace Period and On Hold states are supported
* Fraud detection (basically verifying that purchase is valid and doesn't belong to another user)
* Promo-Codes are supported
* Introductory Prices are supported
* Integrations with 3rd party services
* Cross Platform is supported
* If purchase is made through SDK's billing method, then it will be acknowledged / consumed by SDK.

## Requirements

Apphud SDK requires minimum Android 4.1 and supports only Google Play store.

[Google Play Service Account Credentials](../creating-app.md#google-play-service-credentials) file is required to be uploaded to Apphud.

{% hint style="warning" %}
When adding new subscriptions in Google Play Console, do not add more than one base plans inside existing subscription. Create a separate subscription with a new Product ID instead. [Google Play Product Setup More information](../product-hub/google-play-subscriptions-setup.md)
{% endhint %}

## Installation

Add the following lines to your `build.gradle` file:

```
repositories {
    google()
    mavenCentral() // if using Maven Central
}

allprojects {
    repositories {
      ...
      mavenCentral() // if using Maven Central
      maven { url 'https://jitpack.io' } // if using Jitpack
    }
  }
```

```
dependencies {
  // if using Maven Central
  implementation 'com.apphud:ApphudSDK-Android:{LATEST_VERSION_NUMBER}'

  // if using Jitpack
  implementation 'com.github.apphud:ApphudSDK-Android:{LATEST_VERSION_NUMBER}'
}
```

{% hint style="danger" %}
Replace_`LATEST_VERSION_NUMBER`_with the latest version from our [Github Tags page](https://github.com/apphud/ApphudSDK-Android/tags).
{% endhint %}

## Initialize SDK

To initialize Apphud SDK you will need API Key. It is a unique identifier of your Apphud application. You can get it in your Apphud application settings under `General` tab.

Basic initialization looks like this:

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
Apphud.start(this, "API_KEY")
```
{% endtab %}

{% tab title="Java" %}
```java
Apphud.start(this, "API_KEY");
```
{% endtab %}
{% endtabs %}

For using Apphud SDK in Observer mode, follow [this](../observer-mode.md) link.

### Initialize with Custom User ID

There is additional parameter which sets custom user id:

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
Apphud.start(this, "API_KEY", "custom_user_id")
```
{% endtab %}

{% tab title="Java" %}
```java
Apphud.start(this, "API_KEY", "custom_user_id");
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
More information regarding User ID uniqueness and User merging can be found [here](common.md).
{% endhint %}

## Configure Real-time Developer Notifications

Make sure you have correctly set up Google Real-time Developer Notifications. It helps to detect in-app purchase events in real-time. Read more [here](../creating-app.md#google-real-time-developer-notifications).

## Handle In-App Purchases

You are not required to use Apphud purchase method. If you purchase products by yourself, then you should sync purchases with Apphud. So there are two options:

### Purchase using Apphud Billing Client

To make a purchase call:

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
Apphud.purchase(activity, skuDetails) { _ -> }
```
{% endtab %}

{% tab title="Java" %}
```java
Apphud.purchase(activity, skuDetails, purchases -> { return null;});
```
{% endtab %}
{% endtabs %}

At the launch of the app Apphud automatically fetches SKU Details for products that are added in Apphud dashboard. You can set `ApphudListener` and implement the following method:

```kotlin
fun apphudFetchSkuDetailsProducts(details: List<SkuDetails>)
```

 Store array of `SkuDetails` somewhere in your app.

{% hint style="success" %}
If using Apphud Billing Client all purchases will be acknowledged or consumed  automatically.
{% endhint %}

### Observer Mode

Follow [this](https://docs.apphud.com/getting-started/observer-mode) linlk.

### Check Subscription Status

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
Apphud.hasActiveSubscription()
```
{% endtab %}

{% tab title="Java" %}
```java
Apphud.hasActiveSubscription();
```
{% endtab %}
{% endtabs %}

Returns `true` if user has active subscription. Use this method to determine whether to unlock premium functionality to the user.&#x20;

{% hint style="info" %}
This method works regardless of which billing you are using for purchases.
{% endhint %}

## Migrate existing purchases

Please read [here](../data-migration-guide.md).

## Match User IDs for integrations

Some integrations require you to match user ids between Apphud and corresponding integration. Please see documentation for your integration for details.

## User Properties

Please follow [this](../user-properties.md) guide.
