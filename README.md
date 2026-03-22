# HLR HTTP(s) API

This page provides a detailed reference for all features available via the HTTP interface for HLR (Home Location Register) queries.

## Overview

The HTTP API allows you to integrate your system (client) with [EasySendSMS](https://easysendsms.com) using the HTTP or HTTPS protocols to request HLR lookups securely with SSL encryption.

The client issues either a GET or POST request to the EasySendSMS HTTP API, supplying the required parameters. Our system responds with a JSON-format HTTP response that indicates the status of the number, including details such as status, MCC, MNC, and whether the number is ported.

Using an HLR Lookup, you can clean your data lists or check the status of a phone number before sending an SMS. This can help save costs on delivery for both Bulk SMS campaigns and single SMS messages. HLR Lookup is commonly used by SMS Service Providers for Mobile Number Portability enquiries to obtain MCC and MNC (IMSI) information.

Our HLR Lookup tool also allows users to perform bulk checks of up to 30 numbers at once by separating them with commas. This feature saves time and enhances efficiency, making it ideal for businesses and individuals who need to verify multiple phone numbers quickly. By integrating our HLR API, you can seamlessly incorporate this functionality into your application, ensuring accurate and up-to-date information on number status.

> **Note:**
> - 🔐 **API Password:** If you have set a dedicated API password (different from your account password), make sure you are using the correct one in your request.
> - 🌐 **IP Whitelisting:** If API IP whitelisting is enabled, ensure your request is sent from an authorized IP address. Requests from non-whitelisted IPs will be rejected.
> - ⚠️ **URL Encoding Required:** All API parameters must be properly URL-encoded before sending the request. Special characters like `&`, `%`, `+`, or spaces can break the request if not encoded correctly.

## API Base URL

```http
https://api.easysendsms.app/hlr
```

**Method:** `GET`, `POST`

## Required Parameters

The following parameters must be included in your API request:

| Parameter | Description | Required |
| :--- | :--- | :--- |
| **Username** | Your EasySendSMS username. | Yes |
| **Password** | Your EasySendSMS account password or your API password (if set in your account settings). | Yes |
| **Number** | The subscriber's MSISDN (mobile number) to check. | Yes |

## HTTP Response

The HTTP response from our system will contain the following information in JSON format.

If the HLR query is successful, the response will be returned as follows:

**Example:**
```json
{
    "33644444444": {
        "number": "33644444444",
        "country": "France",
        "err_desc": "Dead",
        "operator": "Transatel",
        "type": "Mobile",
        "mccmnc": "",
        "roaming": "False",
        "err_code": "1",
        "status": "Undelivered",
        "ported": "False"
    }
}
```

If the HLR query fails, the response will return **ERROR: {Error description}**.

**Example:**
```json
{"error":"Authentication failed"}
```

## Example GET Requests

Below are examples of GET requests using the HTTP interface.  
*(Ensure all parameter values are URL-encoded)*

### Single HLR Query Example

**Request URL:**
```http
https://api.easysendsms.app/hlr?username=testuser&password=secret&number=33644444444
```

**Output:**
```json
{
    "33644444444": {
        "number": "33644444444",
        "country": "France",
        "err_desc": "Dead",
        "operator": "Transatel",
        "type": "Mobile",
        "mccmnc": "",
        "roaming": "False",
        "err_code": "1",
        "status": "Undelivered",
        "ported": "False"
    }
}
```

### Bulk HLR Query Example (Max 30 numbers)

**Request URL:**
```http
https://api.easysendsms.app/hlr?username=testuser&password=secret&number=33644444444,33644444445
```

**Output:**
```json
{
    "33644444444": {
        "number": "33644444444",
        "country": "France",
        "err_desc": "Dead",
        "operator": "Transatel",
        "type": "Mobile",
        "mccmnc": "",
        "roaming": "False",
        "err_code": "1",
        "status": "Undelivered",
        "ported": "False"
    },
    "33644444445": {
        "number": "33644444445",
        "country": "France",
        "err_desc": "Dead",
        "operator": "Transatel",
        "type": "Mobile",
        "mccmnc": "",
        "roaming": "False",
        "err_code": "1",
        "status": "Undelivered",
        "ported": "False"
    }
}
```

## HLR Response Fields

The following table outlines the possible fields returned in the HLR response:

| Field | Description |
| :--- | :--- |
| **number** | The subscriber's MSISDN to check. |
| **country** | The subscriber's network country. |
| **err_desc** | Description of the status of the number. Possible values include:<br>• Live<br>• Dead<br>• Inconclusive<br>• No Teleservice Provisioned<br>• Absent Subscriber |
| **operator** | The subscriber's mobile network operator. |
| **type** | The type of number (e.g., Mobile, Fixed). |
| **mccmnc** | The actual MCC and MNC of the subscriber. |
| **roaming** | Indicates whether the subscriber number is currently roaming. |
| **err_code** | Error code corresponding to the `err_desc`. Possible values include:<br>• `0` = Active mobile number<br>• `1` = Number decommissioned by the owning network<br>• `5` = Unable to retrieve a response from the network for this number<br>• `11` = Number unable to receive calls or SMS messages (e.g., data SIM)<br>• `27` = Absent Subscriber |
| **status** | The status of the number. This field is linked with `err_code` and `err_desc`. Possible values include:<br>• Delivered<br>• Undelivered |

## API Rate Limit

To maintain a high quality of service, EasySendSMS API enforces rate limits for its HLR API.

The default request rate limit is 30 requests per second per account per IP address. The API will reject requests exceeding this limit with a `429 Too Many Requests` HTTP status.

You can retry the request after 1 second.

## HLR API Error Codes

The following table outlines possible error codes you may encounter:

| Error Text | Description |
| :--- | :--- |
| **Invalid Number Parameter** | Invalid Number. This means the parameter was not provided or was left blank. |
| **Invalid Password Parameter** | Invalid password. This means the parameter was not provided or was left blank. |
| **Invalid Username Parameter** | Invalid username. This means the parameter was not provided or was left blank. |
| **Authentication failed** | Invalid username or password, or account not active or does not exist. |
| **Insufficient credits** | HLR lookup account has insufficient credits. |
| **Http Error Code: 429** | API calls quota exceeded! Maximum allowed is 30 requests per second. |

## HLR Code Examples

Below are examples of calling the HLR API using various programming languages.

### 🟢 .NET
[Direct Link to Example](#net)

```csharp
var options = new RestClientOptions("")
{
    MaxTimeout = -1,
};

var client = new RestClient(options);
var request = new RestRequest("https://api.easysendsms.app/hlr", Method.Post);
request.AddHeader("Content-Type", "application/x-www-form-urlencoded");
request.AddParameter("username", "username");
request.AddParameter("password", "password");
request.AddParameter("number", "12345678900");
RestResponse response = await client.ExecuteAsync(request);
Console.WriteLine(response.Content);
```

### 🐘 PHP
[Direct Link to Example](#php)

```php
$curl = curl_init();

curl_setopt_array($curl, array(
    CURLOPT_URL => 'https://api.easysendsms.app/hlr',
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_ENCODING => '',
    CURLOPT_MAXREDIRS => 10,
    CURLOPT_TIMEOUT => 0,
    CURLOPT_FOLLOWLOCATION => true,
    CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
    CURLOPT_CUSTOMREQUEST => 'POST',
    CURLOPT_POSTFIELDS => 'username=username&password=password&number=12345678900',
    CURLOPT_HTTPHEADER => array(
        'Content-Type: application/x-www-form-urlencoded'
    ),
));

$response = curl_exec($curl);
curl_close($curl);
echo $response;
```

### ☕ Java
[Direct Link to Example](#java)

```java
Unirest.setTimeouts(0, 0);
HttpResponse<String> response = Unirest.post("https://api.easysendsms.app/hlr")
    .header("Content-Type", "application/x-www-form-urlencoded")
    .field("username", "username")
    .field("password", "password")
    .field("number", "12345678900")
    .asString();
```

### 🐍 Python
[Direct Link to Example](#python)

```python
import http.client

conn = http.client.HTTPSConnection("api.easysendsms.app")
payload = 'username=username&password=password&number=12345678900'
headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
}
conn.request("POST", "/hlr", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```
