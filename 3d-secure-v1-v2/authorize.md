---
description: Authorize an authenticated transaction
---

# Authorize

If you processed authentication only through either [Secure Fields](securefields-1/) or [3D API](api-beta.md), this API can be called to authorize the amount against your acquiring contract. 

{% hint style="warning" %}
* The service requires HTTP basic authentication. The required credentials can be found in our dashboard. Please refer to [API authentication data](../guides/pci-proxy-dashboard/api-authentication-data.md) for more information. 
* Make sure to use our 3D Secure enabled test credit cards [here](testing-3d-secure.md).
{% endhint %}

### 1. Add acquirer to your account

{% hint style="warning" %}
For this feature, you need an existing acquiring contract.
{% endhint %}

| You can choose from a list of [**Supported Acquirer**](../resources/supported-acquirer.md) and contact us at [setup@pci-proxy.com](mailto:setup@pci-proxy.com) |
| :--- |


### 2. Authorize a stored card

{% api-method method="post" host="https://api.sandbox.datatrans.com" path="/v1/transactions/{transactionId}/authorize" %}
{% api-method-summary %}
Authorize
{% endapi-method-summary %}

{% api-method-description %}
Authorize transactions which are alrady authenticated.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="transactionId" type="integer" required=true %}
The `transactionId`received after authentication was done.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Basic MTAwMDAxMTAxMTpYMWVXNmkjJA==   
see Setup
{% endapi-method-parameter %}

{% api-method-parameter name="Content-Type" type="string" required=true %}
API consumes application/json; charset=UTF-8
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="amount" type="integer" required=false %}
The amount of the transaction in the currency’s smallest unit. For example use 1000 for EUR 10.00
{% endapi-method-parameter %}

{% api-method-parameter name="refno" type="string" required=true %}
The merchants reference number. It should be unique for each transaction.currency - ISO character code \(e.g. EUR\)
{% endapi-method-parameter %}

{% api-method-parameter name="3D" type="object" required=false %}
Refer to the official EMVCo 3D specification 2.1.0 for parameter requirements. https://www.emvco.com/emv-technologies/3d-secure/
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Transaction successfully authorized
{% endapi-method-response-example-description %}

```javascript
{
  "acquirerAuthorizationCode": "133707"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% hint style="warning" %}
In test mode, only [test credit cards](../test-card-data.md) are allowed.
{% endhint %}

### Examples

{% tabs %}
{% tab title="Reserve amount \(authorize\)" %}
To reserve an amount, simply send an authorization request with an amount \(`amount=1000`\):

{% code title="Reserve" %}
```javascript
curl -i -X POST https://api.sandbox.datatrans.com/v1/transactions/200427133705358516/authorize \
	-u '1000001112:pYE8lQ6NPb3kaEzQ' \
	-H 'Content-Type: application/json; charset=UTF-8' \
	-d '{
    "amount": 1000,
    "refno": "IZZC7CPiL"
}'
```
{% endcode %}

{% code title="Response \(successful\)" %}
```javascript
{
  "acquirerAuthorizationCode": "133707"
}
```
{% endcode %}
{% endtab %}
{% endtabs %}



