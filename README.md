# InSim API - Direct HTTP Usage

Documentation for using the InSim API directly via HTTP requests without installing the Node.js module.

## 📡 Endpoints

- **Send SMS** : `https://www.insim.app/api/v1/sendsms.php`
- **Add Contacts** : `https://www.insim.app/api/v1/contact.php`
- **Click-to-Call** : `https://www.insim.app/api/v1/clic.php`

## 📱 Send SMS

### curl Request

```bash
curl -X POST https://www.insim.app/api/v1/sendsms.php \
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
curl -X POST https://www.insim.app/api/v1/sendsms.php \
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
curl -X POST https://www.insim.app/api/v1/contact.php \
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
curl -X POST https://www.insim.app/api/v1/clic.php \
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
http POST https://www.insim.app/api/v1/sendsms.php \
  Content-Type:application/json \
  header:='{"login":"your-email@example.com","accessKey":"your-access-key"}' \
  messages:='[{"phone_number":"+33612345678","message":"Test","url":"","priorite":1,"date_to_send":"2025-10-29 10:16:10"}]'
```

### With Postman

1. Method: `POST`
2. URL: `https://www.insim.app/api/v1/sendsms.php`
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

url = "https://www.insim.app/api/v1/sendsms.php"
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
$url = "https://www.insim.app/api/v1/sendsms.php";
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

