

# HLR HTTP(s) API (Get, Post Method)

This page provides a reference for all features available to you via the HTTP interface for HLR query.

## Overview

The [HTTP-API](https://easysendsms.com) allows you to integrate your system (client) to [EasySendSMS](https://easysendsms.com) using the HTTP protocol to request HLR. HTTPS is also supported for secure transactions using SSL encryption.

The Client issues either a GET or POST request to the [EasySendSMS HTTP API](https://easysendsms.com) supplying a list of required parameters. Our system issues back an HTTP response in JSON format which indicates the status of the number, such as the status, MCC, MNC, and Ported.

With an HLR Lookup, you can clean your data lists or check the status of a phone before sending an SMS. This can save money on delivery for both Bulk SMS campaigns as well as Single SMS messages. HLR Lookup is more commonly used by SMS Service Providers for Mobile Number Portability enquiries to obtain MCC and MNC (IMSI) information.

Our [HLR Lookup tool](https://easysendsms.com) allows users to perform bulk checking of up to 30 numbers at once by separating them with commas. This feature saves time and enhances efficiency, making it ideal for businesses and individuals who need to verify multiple phone numbers quickly. By integrating our HLR API, you can seamlessly incorporate this functionality into your application, ensuring accurate and up-to-date information on number status.

## Important Notes

- If you have set a dedicated API Password different from the account password, make sure to use it.
- Invalid Number: If one or more provided numbers are invalid (less than 9 digits, have invalid characters..etc), the process will continue for valid numbers, and invalid numbers will be ignored. No balance will be deducted for invalid numbers.


## API Base URL

```
https://api.easysendsms.app/hlr
```

## API Parameters

| Parameter | Description                                                                 | Presence   |
|-----------|-----------------------------------------------------------------------------|------------|
| Username  | Your [EasySendSMS](https://easysendsms.com) username                        | Mandatory  |
| Password  | Your [EasySendSMS](https://easysendsms.com) account password or your API password (if you set it in your account settings) | Mandatory  |
| Number    | Subscriber MSISDN to check.                                                 | Mandatory  |

## HTTP Response

The HTTP response from our system contains the following:

### Response JSON Format

If the HLR query has been sent successfully, the status code will return as below:

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

If the HLR query is incorrect, it will return `ERROR: {Error description}`

**Example:**

```json
{"error":"Authentication failed"}
```

### Example Queries

**Single HLR Query Example:**

- **Username**: testuser
- **Password**: secret
- **Number**: 33644444444

**Request URL:**

```
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

**Bulk HLR Query Example (Max 30 numbers per request):**

**Request URL:**

```
https://api.easysendsms.app/hlr?username=testuser&password=secret&number=33644444444,33644444444
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

## API Rate Limit

To maintain a high quality of service to all customers, the [EasySendSMS API](https://easysendsms.com) applies rate limits for its HLR API. The default request rate limit is 30 requests per second per account per IP address. The API will reject all requests exceeding this rate limit with a `429 Too Many Requests` HTTP Status.

You can retry the request after 1 second.

## HLR Response

| Value     | Description                                                                                 |
|-----------|---------------------------------------------------------------------------------------------|
| number    | Subscriber MSISDN to check.                                                                  |
| country   | The Subscriber's network country.                                                            |
| err_desc  | If there is an error, otherwise will show "Live" if the number is active. Error description can be one of the following: Live, Dead, Inconclusive, No Teleservice Provisioned, Absent Subscriber |
| operator  | The Subscriber's mobile network operator.                                                    |
| type      | Number type like Mobile, Fixed.                                                              |
| mccmnc    | Actual MCC and MNC of a subscriber.                                                          |
| roaming   | Indicates if the subscriber number is currently roaming.                                     |
| err_code  | Anything marked as Live in `err_desc` will always have an Error Code of 1. Error code can be one of the following: 0=Active mobile number, 1=Number decommissioned by the owning network, 5=Unable to retrieve a response from the network, 11=This number cannot receive calls or SMS messages, 27=Absent Subscriber. |
| status    | Number status.                                                                               |

`error_code`, `err_desc`, and `status` are all linked, and you can use any of these fields. `status` can be one of the following states:

- Delivered
- Undelivered

## API Error Codes

| Error Text                     | Description                                                                                       |
|--------------------------------|---------------------------------------------------------------------------------------------------|
| Invalid Number Parameter       | Invalid Number. This means the parameter was not provided or left blank.                          |
| Invalid Password Parameter     | Invalid password. This means the parameter was not provided or left blank.                       |
| Invalid Username Parameter     | Invalid username. This means the parameter was not provided or left blank.                       |
| Authentication failed          | Invalid username or password, or account not active or existing.                                 |
| Insufficient credits           | HLR lookup account has insufficient credits.                                                      |
| Http Error Code: 429           | API calls quota exceeded! Maximum allowed is 30 per second.                                       |



### Call The API By Code

## .NET:
```
        var options = new RestClientOptions("")
        {
        MaxTimeout = -1,
        };
        var client = new RestClient(options);
        var request = new RestRequest("https://api.easysendsms.app/hlr", Method.Post);
        request.AddHeader("Content-Type", "application/x-www-form-urlencoded");
        request.AddHeader("Cookie", "ASPSESSIONIDASCQBARR=NKOHDCHDOFEOOALJIGDGGPAM; ASPSESSIONIDQSSDABDR=GBMPKKKDOBIPGBAMEHKNDAIE;        
        ASPSESSIONIDSSRDDBDQ=BDOPELKDNCOINCJKFIADJAKA");
        request.AddParameter("username", "username");
        request.AddParameter("password", "password");
        request.AddParameter("number", "12345678900");
        RestResponse response = await client.ExecuteAsync(request);
        Console.WriteLine(response.Content);
```
## PHP:
```
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
            'Content-Type: application/x-www-form-urlencoded',
            'Cookie: ASPSESSIONIDASCQBARR=NKOHDCHDOFEOOALJIGDGGPAM; ASPSESSIONIDQSSDABDR=GBMPKKKDOBIPGBAMEHKNDAIE; ASPSESSIONIDSSRDDBDQ=BDOPELKDNCOINCJKFIADJAKA'
        ),
        ));

        $response = curl_exec($curl);

        curl_close($curl);
        echo $response;
    
```
## JAVA:
```
        Unirest.setTimeouts(0, 0);
        HttpResponse response = Unirest.post("https://api.easysendsms.app/hlr")
        .header("Content-Type", "application/x-www-form-urlencoded")
        .header("Cookie", "ASPSESSIONIDASCQBARR=NKOHDCHDOFEOOALJIGDGGPAM; ASPSESSIONIDQSSDABDR=GBMPKKKDOBIPGBAMEHKNDAIE; 
        ASPSESSIONIDSSRDDBDQ=BDOPELKDNCOINCJKFIADJAKA")
        .field("username", "username")
        .field("password", "password")
        .field("number", "12345678900")
        .asString();
    
```
