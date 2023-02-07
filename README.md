# HLR-HTTP-s-API-
With an HLR Lookup you can clean your data lists, or check the status of a phone before sending an SMS.

This page provides a reference for all features available to you via the HTTP interface for HLR query.

The HTTP-API allows you to integrate your system (client) to EasySendSMS using the HTTP protocol to request HLR. HTTPS is also supported for secure transactions using SSL encryption.

The Client issues either a GET or POST request to the EasySendSMS HTTP API supplying a list of required parameters. Our system issues back a HTTP response as Json format which indicates the status of number such the status, MCC, MNC, Ported.

With an HLR Lookup you can clean your data lists, or check the status of a phone before sending an SMS. This can save money on delivery for both Bulk SMS campaigns as well as Singles SMS messages.
HLR Lookup is more commonly used by SMS Service Providers for Mobile Number Portability enquiries to obtain MCC and MNC (IMSI) information.

## API Base URL:
https://api.easysendsms.app/hlr

| Parameter | Description | Presence |
| --------- | ----------- | -------- |
Username | Your EasySendSMS username	| Mandatory
Password	| Your EasySendSMS account password or your API password (if you set at your account settings)	| Mandatory
Number | Subscriber msisdn to check. | Mandatory

## HTTP Response
The HTTP response from our system contains the following:

## Response Json Format
If the HLR query has been sent successfully the status code will return as below:

**Example:**
                                                                  
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
                                                                    
                                                                
If HLR query incorrect it will return ERROR: {Error code}

**Example:**

    Authentication failed
    
**Examples:**

Below are example for (Get Method) request when using the HTTP interface.

**Example Query Single HLR**

    https://api.easysendsms.app/hlr?username=testuser&password=secret&number=33644444444

## Output:
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
    
**Example to do bulk HLR query (Max 30 numbers per request)**

    https://api.easysendsms.app/hlr?username=testuser&password=secret&number=33644444444,33644444444
    
Output:

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
                                                                
                                                                
## API rate limit:
To maintain a high quality of service to all customers, EasySendSMS API applies rate limits for its HLR API.
The default request rate limit is 30 request per second per account per IP address.
The API will reject all requests exceeding this rate limit with 429 Too Many Requests HTTP Status.

You can retry the request after 1 second.

## HLR Response

| Value | Description |
| --------- | ----------- |
number |	Subscriber msisdn to check.
country	| The Subscriber network country.
err_desc |	If have error otherwise will Show Live if the number active. error description can beon of the folowing: <br/>Live    <br/>Dead    <br/>Inconclusive No Teleservice Provisioned <br/>Absent Subscriber
operator |	the Subscriber mobile network operator.
type | number type like Mobile,Fixed.
mccmnc | actual MCC and MNC of a subscriber.
roaming |	if the subscriber number currently roaming.
error_code | anything marked as Live in error_desc will always be Error Code 1, error code can be on of the folowing:  <br/>0=Active mobile number    <br/>1=Number decommissioned by the owning network    <br/>5=We are unable to retrieve a response from the network for this numb    <br/>11=This number is not able to receive calls or SMS messages. This is usually a number relating to a data SIM  <br/>27=There are a number of ways a number can be returned as Absent Subscriber Please see our Absent Subscriber information page for further details.
status | number status. error_code, error_desc and Status are all linked and you can use any of these fields.<br/>Status can be one of the folowing state:  <br/>1-Delivered <br/>2-Undelivered

## API Error Codes:


| Error Text | Description |
| --------- | ----------- |
Invalid Number Parameter |	Invalid Number. This means the parameter was not provided or left blank.
Invalid Password Parameter |	Invalid password. This means the parameter was not provided or left blank.
Invalid Username Parameter |	Invalid username. This means the parameter was not provided or left blank.
Authentication failed |	Invalid username or password, or account not active or exists.
Http Error Code: 429 |	API calls quota exceeded! maximum admitted 30 per 1s/.

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
