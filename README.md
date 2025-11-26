# InSim API - Direct HTTP Usage

Documentation for using the InSim API directly via HTTP requests without installing the Node.js module.

## 📡 Endpoints

- **Send SMS** : `https://www.insim.app/api/v1/sendsms`
- **Add Contacts** : `https://www.insim.app/api/v1/contact`
- **Click-to-Call** : `https://www.insim.app/api/v1/clicTocall`

## 📱 Send SMS

### curl Request

```bash
curl -X POST https://www.insim.app/api/v1/sendsms \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
      "login": "your-email@example.com",
      "accessKey": "your-access-key"
    },
    "messages": [
      {
        "phone_number": "+33612345678",
        "message": "Hello from InSim API [Lien_Tracking]",
        "url": "https://example.com/",
        "priorite": 1,
        "date_to_send": "2025-10-29 10:16:10"
      },
      {
        "phone_number": "+33698765432",
        "message": "Test from API",
        "url": "",
        "priorite": 1,
        "date_to_send": "2025-10-29 10:16:10"
      }
    ]
  }'
```

### Example with JSON File

Create a file `sendsms.json`:

```json
{
  "header": {
    "login": "your-email@example.com",
    "accessKey": "your-access-key"
  },
  "messages": [
    {
      "phone_number": "+33612345678",
      "message": "Hello from InSim API [Lien_Tracking]",
      "url": "https://example.com/",
      "priorite": 1,
      "date_to_send": "2025-10-29 10:16:10"
    },
    {
      "phone_number": "+33698765432",
      "message": "Test from API",
      "url": "",
      "priorite": 1,
      "date_to_send": "2025-10-29 10:16:10"
    }
  ]
}
```

Then execute:

```bash
curl -X POST https://www.insim.app/api/v1/sendsms \
  -H "Content-Type: application/json" \
  -d @sendsms.json
```

### Request Structure

**Header:**
- `login` (String): Your InSim account email
- `accessKey` (String): API access key

**Messages:**
- `phone_number` (String): Phone number in international format (e.g., `+33612345678`)
- `message` (String): Message content. Use `[Lien_Tracking]` to insert the tracking URL
- `url` (String): Optional URL to include in the message
- `priorite` (Number): Message priority (1 by default)
- `date_to_send` (String): Send date in format `"YYYY-MM-DD HH:mm:ss"` (optional)

### Response

```json
[
  {
    "id_sms_api": "FI5O7apqaaqcUmQ",
    "sms_per_message": 1,
    "user": "your-email@example.com",
    "sent_time": "2025-10-29T10:16:10.541Z",
    "phone_number": "+33612345678",
    "message": "Hello from InSim API https://arsms.co/oloe00en5QPi \n \nSent for free from PC via arsms.co/free",
    "sent": 1
  },
  {
    "id_sms_api": "ABC123xyz456",
    "sms_per_message": 1,
    "user": "your-email@example.com",
    "sent_time": "2025-10-29T10:16:10.541Z",
    "phone_number": "+33698765432",
    "message": "Test from API https://arsms.co/xyz789 \n \nSent for free from PC via arsms.co/free",
    "sent": 1
  }
]
```

**Response Fields:**
- `id_sms_api` (String): Unique SMS identifier
- `sms_per_message` (Number): Number of SMS needed to send the message
- `user` (String): Email of the user who sent the SMS
- `sent_time` (String): Send date and time in ISO 8601 format
- `phone_number` (String): Recipient phone number
- `message` (String): Sent message content (with tracking URL if provided)
- `sent` (Number): Send status (1 = sent, 0 = not sent)

## 👥 Add Contacts

### curl Request

```bash
curl -X POST https://www.insim.app/api/v1/contact \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
      "login": "your-email@example.com",
      "accessKey": "your-access-key"
    },
    "contacts": [
      {
        "firstname": "John",
        "lastname": "Doe",
        "phone_number": "+33612345678",
        "adress": "",
        "email": ""
      },
      {
        "firstname": "Jane",
        "lastname": "Smith",
        "phone_number": "+33698765432",
        "adress": "123 Peace Street, Paris",
        "email": "jane.smith@example.com"
      }
    ]
  }'
```

### Request Structure

**Header:**
- `login` (String): Your InSim account email
- `accessKey` (String): API access key

**Contacts:**
- `firstname` (String): Contact first name
- `lastname` (String): Contact last name
- `phone_number` (String): Phone number in international format (e.g., `+33612345678`)
- `adress` (String, optional): Contact address
- `email` (String, optional): Contact email

### Response (Success)

```json
{
  "data": {
    "contact": [
      {
        "firstname": "John",
        "lastname": "Doe",
        "phonenumber": "+33612345678",
        "adress": "",
        "email": "",
        "result": "success"
      },
      {
        "firstname": "Jane",
        "lastname": "Smith",
        "phonenumber": "+33698765432",
        "adress": "123 Peace Street, Paris",
        "email": "jane.smith@example.com",
        "result": "success"
      }
    ]
  }
}
```

### Response (Error)

```json
{
  "data": {
    "contact": [
      {
        "first_name": "John",
        "last_name": "Doe",
        "phone_number": "+33INVALID",
        "adress": "",
        "email": "",
        "result": "failed",
        "errors": [
          "#001"
        ]
      }
    ]
  }
}
```

**Error Codes:**
- `#001`: Invalid phone number
- `#002`: Empty phone number
- `#003`: No phone number variable found
- `#004`: Invalid E-mail (Warning, does not block contact creation or update)

## 📞 Click-to-Call

### curl Request

```bash
curl -X POST https://www.insim.app/api/v1/clicTocall \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
      "login": "your-email@example.com",
      "accessKey": "your-access-key",
      "type": "clicToCall",
      "phone_number": "+33612345678"
    }
  }'
```

### Request Structure

**Header:**
- `login` (String): Your InSim account email
- `accessKey` (String): API access key
- `type` (String): Must be `"clicToCall"`
- `phone_number` (String): Phone number to call in international format (e.g., `+33612345678`)

### Response

```json
[
  {
    "info": "please make sure the phone is connected and inSIM is running",
    "result": "success",
    "errors": []
  }
]
```

**Response Fields:**
- `info` (String): Informative message
- `result` (String): Operation result (`"success"` or `"failed"`)
- `errors` (Array): Array of error codes (empty if success)

**Error Codes:**
- `#001`: Our servers are down
- `#002`: Phone not connected, inSIM not running on the phone

**Result Values:**
- `"success"`: Information successfully arrived at our servers
- `"failed"`: Request failed

## 🔧 Examples with Other Tools

### With HTTPie

```bash
http POST https://www.insim.app/api/v1/sendsms \
  Content-Type:application/json \
  header:='{"login":"your-email@example.com","accessKey":"your-access-key"}' \
  messages:='[{"phone_number":"+33612345678","message":"Test","url":"","priorite":1,"date_to_send":"2025-10-29 10:16:10"}]'
```

### With Postman

1. Method: `POST`
2. URL: `https://www.insim.app/api/v1/sendsms`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
```json
{
  "header": {
    "login": "your-email@example.com",
    "accessKey": "your-access-key"
  },
  "messages": [
    {
      "phone_number": "+33612345678",
      "message": "Test from Postman",
      "url": "",
      "priorite": 1,
      "date_to_send": "2025-10-29 10:16:10"
    }
  ]
}
```

### With Python (requests)

```python
import requests
import json

url = "https://www.insim.app/api/v1/sendsms"
headers = {"Content-Type": "application/json"}
data = {
    "header": {
        "login": "your-email@example.com",
        "accessKey": "your-access-key"
    },
    "messages": [
        {
            "phone_number": "+33612345678",
            "message": "Test from Python",
            "url": "",
            "priorite": 1,
            "date_to_send": "2025-10-29 10:16:10"
        }
    ]
}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

### With PHP

```php
<?php
$url = "https://www.insim.app/api/v1/sendsms";
$data = [
    "header" => [
        "login" => "your-email@example.com",
        "accessKey" => "your-access-key"
    ],
    "messages" => [
        [
            "phone_number" => "+33612345678",
            "message" => "Test from PHP",
            "url" => "",
            "priorite" => 1,
            "date_to_send" => "2025-10-29 10:16:10"
        ]
    ]
];

$options = [
    'http' => [
        'header' => "Content-Type: application/json\r\n",
        'method' => 'POST',
        'content' => json_encode($data)
    ]
];

$context = stream_context_create($options);
$result = file_get_contents($url, false, $context);
echo $result;
?>
```

## 🔔 Webhooks / Callbacks

The InSim API system sends multiple webhooks to your configured callback URLs to notify you about various events in real-time.

### 🔥 Introduction

- **HTTP Method**: All webhooks are sent using **GET** requests
- **Data Encoding**: All data is encoded using **encodeURIComponent()** before being sent
- **Real-time Notifications**: Receive instant notifications about SMS, calls, link clicks, and more
- **No Installation Required**: You don't need to install any API module to receive webhooks

### How It Works

1. Configure your callback URLs in your InSim account settings
2. When an event occurs, InSim sends a GET request to your callback URL
3. The JSON payload is encoded and sent as a query parameter
4. Your server decodes the parameter and processes the webhook

### A. Incoming SMS - `incoming_sms`

Receive notifications when an SMS is received on your InSim number.

#### Payload JSON

```json
{
  "device_identification": "user@email.com",
  "title": "incoming_sms",
  "from": "+33612345678",
  "message": "Hello, this is a test message",
  "date": "2023-12-17T20:15:30.565Z"
}
```

#### GET Request Format

```
https://your-callback-url.com?message={ENCODED_JSON}
```

#### Encoding Example

```javascript
request(decodeURIComponent(callback_url) + '?message=' + encodeURIComponent(message));
```

#### Field Description

| Field | Type | Description |
|-------|------|-------------|
| `device_identification` | String | Email address of the InSim account that received the SMS |
| `title` | String | Always `"incoming_sms"` for incoming SMS events |
| `from` | String | Phone number of the sender in international format (e.g., `+33612345678`) |
| `message` | String | Content of the received SMS message |
| `date` | String | Date and time when the SMS was received (ISO 8601 format) |

---

### B. Call Events - `incoming_call` / `outgoing_call` / `missed_call`

Receive notifications about incoming, outgoing, or missed calls.

#### Payload JSON

**Outgoing Call:**
```json
{
  "device_identification": "user@email.com",
  "title": "outgoing_call",
  "phone_number": "+33612345678",
  "call_time": "2023-12-17 20:32:51",
  "duration": "0"
}
```

**Incoming Call:**
```json
{
  "device_identification": "user@email.com",
  "title": "incoming_call",
  "phone_number": "+33612345678",
  "call_time": "2023-12-17 20:32:51",
  "duration": "15:11:04"
}
```

**Missed Call:**
```json
{
  "device_identification": "user@email.com",
  "title": "missed_call",
  "phone_number": "+33612345678",
  "call_time": "2023-12-17 20:32:51",
  "duration": "0"
}
```

#### GET Request Format

```
https://your-callback-url.com?calls={ENCODED_JSON}
```

#### Encoding Example

```javascript
request(decodeURIComponent(call_url) + '?calls=' + encodeURIComponent(message));
```

#### Field Description

| Field | Type | Description |
|-------|------|-------------|
| `device_identification` | String | Email address of the InSim account |
| `title` | String | Call type: `"incoming_call"`, `"outgoing_call"`, or `"missed_call"` |
| `phone_number` | String | Phone number in international format (e.g., `+33612345678`) |
| `call_time` | String | Date and time of the call in format `"YYYY-MM-DD HH:mm:ss"` |
| `duration` | String | Call duration in format `"HH:mm:ss"` or `"0"` for missed calls |

#### Possible Title Values

- `"incoming_call"`: An incoming call was received
- `"outgoing_call"`: An outgoing call was made
- `"missed_call"`: A call was missed

---

### C. ShortURL Click Tracking - `clicked link`

Receive notifications when someone clicks on a tracking link included in your SMS.

#### Payload JSON

```json
{
  "id_sms_api": "Pf7v16XZ38oT02s",
  "title": "clicked link",
  "phone_number": "+33612345678",
  "link": "https://beautifier.io/",
  "date": "2022-11-12 17:04:13"
}
```

#### GET Request Format

```
https://your-callback-url.com?clics={ENCODED_JSON}
```

#### Encoding Example

```javascript
request(decodeURIComponent(link_url) + '?clics=' + encodeURIComponent(message));
```

#### Field Description

| Field | Type | Description |
|-------|------|-------------|
| `id_sms_api` | String | Unique identifier of the SMS that contained the link |
| `title` | String | Always `"clicked link"` for link click events |
| `phone_number` | String | Phone number of the person who clicked the link (international format, e.g., `+33612345678`) |
| `link` | String | The original URL that was clicked |
| `date` | String | Date and time when the link was clicked in format `"YYYY-MM-DD HH:mm:ss"` |

---

### D. Call Qualification - `call_qualification_from_mobile` / `call_qualification_from_interface`

Receive notifications when call qualifications are submitted from mobile app or web interface.

#### Payload JSON

**From Mobile:**
```json
{
  "device_identification": "user@email.com",
  "title": "call_qualification_from_mobile",
  "from": "+33612345678",
  "date": "2023-12-17 23:57:52",
  "qualification": "Customer interested in product X"
}
```

**From Interface:**
```json
{
  "device_identification": "user@email.com",
  "title": "call_qualification_from_interface",
  "from": "+33612345678",
  "date": "2023-12-17 23:57:52",
  "qualification": "Follow-up required"
}
```

#### GET Request Format

```
https://your-callback-url.com?qualification={ENCODED_JSON}
```

#### Encoding Example

```javascript
request(decodeURIComponent(qualification_url) + '?qualification=' + encodeURIComponent(message));
```

#### Field Description

| Field | Type | Description |
|-------|------|-------------|
| `device_identification` | String | Email address of the InSim account |
| `title` | String | Source of qualification: `"call_qualification_from_mobile"` or `"call_qualification_from_interface"` |
| `from` | String | Phone number in international format (e.g., `+33612345678`) |
| `date` | String | Date and time when the qualification was submitted in format `"YYYY-MM-DD HH:mm:ss"` |
| `qualification` | String | The qualification text/notes submitted by the user |

#### Possible Title Values

- `"call_qualification_from_mobile"`: Qualification submitted from the mobile app
- `"call_qualification_from_interface"`: Qualification submitted from the web interface

---

### E. Message Delivery Status (DLR) - `sent` / `received`

Receive notifications about SMS delivery status changes (Delivery Receipt).

#### Payload JSON

```json
{
  "user": "SENDER_LOGIN",
  "phone_number": "+33612345678",
  "status": "received",
  "date_status": "2019-08-09T12:50:54.211Z",
  "id_sms_api": "YOUR_ID_SMS"
}
```

#### GET Request Format

```
https://your-callback-url.com?status={ENCODED_JSON}
```

#### Encoding Example

```javascript
request(decodeURIComponent(status_url) + '?status=' + encodeURIComponent(message));
```

#### Field Description

| Field | Type | Description |
|-------|------|-------------|
| `user` | String | Email address of the account that sent the SMS |
| `phone_number` | String | Recipient phone number in international format (e.g., `+33612345678`) |
| `status` | String | Delivery status: `"sent"` or `"received"` |
| `date_status` | String | Date and time of the status change (ISO 8601 format) |
| `id_sms_api` | String | Unique identifier of the SMS (the one you provided when sending or the one generated by the system) |

#### Possible Status Values

- `"sent"`: SMS has been sent from the system
- `"received"`: SMS has been received by the recipient's device

---

## 📊 Webhook Parameters Summary

| Webhook Event | GET Parameter | Example URL Format |
|---------------|---------------|-------------------|
| Incoming SMS | `message` | `?message={json}` |
| Call Events | `calls` | `?calls={json}` |
| Link Click Tracking | `clics` | `?clics={json}` |
| Call Qualification | `qualification` | `?qualification={json}` |
| Delivery Status (DLR) | `status` | `?status={json}` |

---

## 📝 Technical Notes

### HTTP Method
- All webhooks are sent using **GET** requests
- No POST requests are used for webhooks

### Data Encoding
- All JSON payloads are encoded using **encodeURIComponent()** before being sent
- Your server must decode the parameter using `decodeURIComponent()` or equivalent
- The callback URL must accept a complete JSON object in a single GET parameter

### URL Structure
The webhook URL structure follows this pattern:
```
https://your-callback-url.com?{PARAMETER}={ENCODED_JSON}
```

Where:
- `{PARAMETER}` is one of: `message`, `calls`, `clics`, `qualification`, or `status`
- `{ENCODED_JSON}` is the JSON payload encoded with `encodeURIComponent()`

### Response Handling
- Your webhook endpoint should return a `200 OK` status code to acknowledge receipt
- If the webhook fails (non-200 response), InSim may retry the webhook
- It's recommended to respond quickly (within 5 seconds) to avoid timeouts

### Security Considerations
- Always use **HTTPS** for your callback URLs
- Validate and sanitize incoming data
- Implement proper error handling
- Consider implementing webhook signature verification if available

---

## 💻 Server-Side Implementation Examples

### PHP Example

```php
<?php
// Handle incoming SMS webhook
if (isset($_GET["message"])) {
    $message = json_decode($_GET["message"], true);
    
    // Process the incoming SMS
    $from = $message["from"];
    $text = $message["message"];
    $date = $message["date"];
    
    // Save to database, trigger actions, etc.
    // ...
    
    http_response_code(200);
    echo "OK";
}

// Handle call events webhook
if (isset($_GET["calls"])) {
    $call = json_decode($_GET["calls"], true);
    
    // Process the call event
    $type = $call["title"]; // incoming_call, outgoing_call, missed_call
    $phone = $call["phone_number"];
    $duration = $call["duration"];
    
    // Update CRM, log call, etc.
    // ...
    
    http_response_code(200);
    echo "OK";
}

// Handle link click webhook
if (isset($_GET["clics"])) {
    $click = json_decode($_GET["clics"], true);
    
    // Track the click
    $smsId = $click["id_sms_api"];
    $phone = $click["phone_number"];
    $link = $click["link"];
    
    // Update analytics, trigger conversions, etc.
    // ...
    
    http_response_code(200);
    echo "OK";
}

// Handle delivery status webhook
if (isset($_GET["status"])) {
    $status = json_decode($_GET["status"], true);
    
    // Update SMS delivery status
    $smsId = $status["id_sms_api"];
    $deliveryStatus = $status["status"]; // sent or received
    
    // Update database, send notifications, etc.
    // ...
    
    http_response_code(200);
    echo "OK";
}

// Handle call qualification webhook
if (isset($_GET["qualification"])) {
    $qualification = json_decode($_GET["qualification"], true);
    
    // Process qualification
    $phone = $qualification["from"];
    $notes = $qualification["qualification"];
    $source = $qualification["title"];
    
    // Update CRM, trigger workflows, etc.
    // ...
    
    http_response_code(200);
    echo "OK";
}
?>
```

### Node.js (Express) Example

```javascript
const express = require('express');
const app = express();

// Handle incoming SMS webhook
app.get('/webhook', (req, res) => {
  if (req.query.message) {
    const message = JSON.parse(decodeURIComponent(req.query.message));
    
    console.log('Incoming SMS from:', message.from);
    console.log('Message:', message.message);
    
    // Process the SMS
    // Save to database, trigger actions, etc.
    
    res.status(200).send('OK');
  }
  
  // Handle call events
  if (req.query.calls) {
    const call = JSON.parse(decodeURIComponent(req.query.calls));
    
    console.log('Call Type:', call.title);
    console.log('Phone:', call.phone_number);
    
    // Process the call event
    // Update CRM, log call, etc.
    
    res.status(200).send('OK');
  }
  
  // Handle link clicks
  if (req.query.clics) {
    const click = JSON.parse(decodeURIComponent(req.query.clics));
    
    console.log('Link clicked by:', click.phone_number);
    console.log('Link:', click.link);
    
    // Track the click
    // Update analytics, etc.
    
    res.status(200).send('OK');
  }
  
  // Handle delivery status
  if (req.query.status) {
    const status = JSON.parse(decodeURIComponent(req.query.status));
    
    console.log('SMS Status:', status.status);
    console.log('SMS ID:', status.id_sms_api);
    
    // Update delivery status
    // Update database, etc.
    
    res.status(200).send('OK');
  }
  
  // Handle call qualification
  if (req.query.qualification) {
    const qualification = JSON.parse(decodeURIComponent(req.query.qualification));
    
    console.log('Qualification:', qualification.qualification);
    console.log('Source:', qualification.title);
    
    // Process qualification
    // Update CRM, etc.
    
    res.status(200).send('OK');
  }
});

app.listen(3000, () => {
  console.log('Webhook server listening on port 3000');
});
```

### Python (Flask) Example

```python
from flask import Flask, request, jsonify
import json
from urllib.parse import unquote

app = Flask(__name__)

@app.route('/webhook', methods=['GET'])
def webhook():
    # Handle incoming SMS webhook
    if 'message' in request.args:
        message = json.loads(unquote(request.args.get('message')))
        
        print(f"Incoming SMS from: {message['from']}")
        print(f"Message: {message['message']}")
        
        # Process the SMS
        # Save to database, trigger actions, etc.
        
        return 'OK', 200
    
    # Handle call events
    if 'calls' in request.args:
        call = json.loads(unquote(request.args.get('calls')))
        
        print(f"Call Type: {call['title']}")
        print(f"Phone: {call['phone_number']}")
        
        # Process the call event
        # Update CRM, log call, etc.
        
        return 'OK', 200
    
    # Handle link clicks
    if 'clics' in request.args:
        click = json.loads(unquote(request.args.get('clics')))
        
        print(f"Link clicked by: {click['phone_number']}")
        print(f"Link: {click['link']}")
        
        # Track the click
        # Update analytics, etc.
        
        return 'OK', 200
    
    # Handle delivery status
    if 'status' in request.args:
        status = json.loads(unquote(request.args.get('status')))
        
        print(f"SMS Status: {status['status']}")
        print(f"SMS ID: {status['id_sms_api']}")
        
        # Update delivery status
        # Update database, etc.
        
        return 'OK', 200
    
    # Handle call qualification
    if 'qualification' in request.args:
        qualification = json.loads(unquote(request.args.get('qualification')))
        
        print(f"Qualification: {qualification['qualification']}")
        print(f"Source: {qualification['title']}")
        
        # Process qualification
        # Update CRM, etc.
        
        return 'OK', 200
    
    return 'No webhook data', 400

if __name__ == '__main__':
    app.run(port=3000)
```

## 📝 Important Notes

- All phone numbers must be in international format with the `+` prefix (e.g., `+33612345678` for France)
- Date format for `date_to_send` is: `"YYYY-MM-DD HH:mm:ss"` (e.g., `"2025-10-29 10:16:10"`)
- The `[Lien_Tracking]` field in the message will be replaced by the automatically generated tracking URL
- Make sure your API access key is valid and active
- In case of error, check the error codes returned in the response

## 🔗 Links

- [Node.js Module Documentation](https://github.com/ArdaryinSIM/insim-api)
- [InSim Website](https://ardary-insim.com/)

## 📄 License

MIT

## 👤 Author

ArdaryinSIM

