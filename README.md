# InSim API - Utilisation directe (HTTP)

Documentation pour utiliser l'API InSim directement via des requêtes HTTP sans installer le module Node.js.

## 📡 Endpoints

- **Envoi de SMS** : `https://www.insim.app/api/v1/sendsms.php`
- **Ajout de contacts** : `https://www.insim.app/api/v1/contact.php`
- **Clic-to-Call** : `https://www.insim.app/api/v1/clic.php`

## 📱 Envoi de SMS

### Requête curl

```bash
curl -X POST https://www.insim.app/api/v1/sendsms.php \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
      "login": "votre-email@example.com",
      "accessKey": "votre-clé-d'\''accès"
    },
    "messages": [
      {
        "phone_number": "+33612345678",
        "message": "Bonjour depuis l'\''API InSim [Lien_Tracking]",
        "url": "https://example.com/",
        "priorite": 1,
        "date_to_send": "2025-10-29 10:16:10"
      },
      {
        "phone_number": "+33698765432",
        "message": "Test depuis l'\''API",
        "url": "",
        "priorite": 1,
        "date_to_send": "2025-10-29 10:16:10"
      }
    ]
  }'
```

### Exemple avec un fichier JSON

Créez un fichier `sendsms.json` :

```json
{
  "header": {
    "login": "votre-email@example.com",
    "accessKey": "votre-clé-d'accès"
  },
  "messages": [
    {
      "phone_number": "+33612345678",
      "message": "Bonjour depuis l'API InSim [Lien_Tracking]",
      "url": "https://example.com/",
      "priorite": 1,
      "date_to_send": "2025-10-29 10:16:10"
    },
    {
      "phone_number": "+33698765432",
      "message": "Test depuis l'API",
      "url": "",
      "priorite": 1,
      "date_to_send": "2025-10-29 10:16:10"
    }
  ]
}
```

Puis exécutez :

```bash
curl -X POST https://www.insim.app/api/v1/sendsms.php \
  -H "Content-Type: application/json" \
  -d @sendsms.json
```

### Structure de la requête

**Header :**
- `login` (String) : Email de votre compte InSim
- `accessKey` (String) : Clé d'accès API

**Messages :**
- `phone_number` (String) : Numéro de téléphone au format international (ex: `+33612345678`)
- `message` (String) : Contenu du message. Utilisez `[Lien_Tracking]` pour insérer l'URL de tracking
- `url` (String) : URL optionnelle à inclure dans le message
- `priorite` (Number) : Priorité du message (1 par défaut)
- `date_to_send` (String) : Date d'envoi au format `"YYYY-MM-DD HH:mm:ss"` (optionnel)

### Réponse

```json
[
  {
    "id_sms_api": "FI5O7apqaaqcUmQ",
    "sms_per_message": 1,
    "user": "votre-email@example.com",
    "sent_time": "2025-10-29T10:16:10.541Z",
    "phone_number": "+33612345678",
    "message": "Bonjour depuis l'API InSim https://arsms.co/oloe00en5QPi \n \nSent for free from PC via arsms.co/free",
    "sent": 1
  },
  {
    "id_sms_api": "ABC123xyz456",
    "sms_per_message": 1,
    "user": "votre-email@example.com",
    "sent_time": "2025-10-29T10:16:10.541Z",
    "phone_number": "+33698765432",
    "message": "Test depuis l'API https://arsms.co/xyz789 \n \nSent for free from PC via arsms.co/free",
    "sent": 1
  }
]
```

**Champs de la réponse :**
- `id_sms_api` (String) : Identifiant unique du SMS
- `sms_per_message` (Number) : Nombre de SMS nécessaires pour envoyer le message
- `user` (String) : Email de l'utilisateur qui a envoyé le SMS
- `sent_time` (String) : Date et heure d'envoi au format ISO 8601
- `phone_number` (String) : Numéro de téléphone du destinataire
- `message` (String) : Contenu du message envoyé (avec URL de tracking si fournie)
- `sent` (Number) : Statut d'envoi (1 = envoyé, 0 = non envoyé)

## 👥 Ajout de contacts

### Requête curl

```bash
curl -X POST https://www.insim.app/api/v1/contact.php \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
      "login": "votre-email@example.com",
      "accessKey": "votre-clé-d'\''accès"
    },
    "contacts": [
      {
        "firstname": "Jean",
        "lastname": "Dupont",
        "phone_number": "+33612345678",
        "adress": "",
        "email": ""
      },
      {
        "firstname": "Marie",
        "lastname": "Martin",
        "phone_number": "+33698765432",
        "adress": "123 Rue de la Paix, Paris",
        "email": "marie.martin@example.com"
      }
    ]
  }'
```

### Structure de la requête

**Header :**
- `login` (String) : Email de votre compte InSim
- `accessKey` (String) : Clé d'accès API

**Contacts :**
- `firstname` (String) : Prénom du contact
- `lastname` (String) : Nom du contact
- `phone_number` (String) : Numéro de téléphone au format international (ex: `+33612345678`)
- `adress` (String, optionnel) : Adresse du contact
- `email` (String, optionnel) : Email du contact

### Réponse (succès)

```json
{
  "data": {
    "contact": [
      {
        "firstname": "Jean",
        "lastname": "Dupont",
        "phonenumber": "+33612345678",
        "adress": "",
        "email": "",
        "result": "success"
      },
      {
        "firstname": "Marie",
        "lastname": "Martin",
        "phonenumber": "+33698765432",
        "adress": "123 Rue de la Paix, Paris",
        "email": "marie.martin@example.com",
        "result": "success"
      }
    ]
  }
}
```

### Réponse (erreur)

```json
{
  "data": {
    "contact": [
      {
        "first_name": "Jean",
        "last_name": "Dupont",
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

**Codes d'erreur :**
- `#001` : Invalid phone number
- `#002` : Empty phone number
- `#003` : No phone number variable found
- `#004` : Invalid E-mail (Warning, ne bloque pas la création ou mise à jour du contact)

## 📞 Clic-to-Call

### Requête curl

```bash
curl -X POST https://www.insim.app/api/v1/clic.php \
  -H "Content-Type: application/json" \
  -d '{
    "header": {
      "login": "votre-email@example.com",
      "accessKey": "votre-clé-d'\''accès",
      "type": "clicToCall",
      "phone_number": "+33612345678"
    }
  }'
```

### Structure de la requête

**Header :**
- `login` (String) : Email de votre compte InSim
- `accessKey` (String) : Clé d'accès API
- `type` (String) : Doit être `"clicToCall"`
- `phone_number` (String) : Numéro de téléphone à appeler au format international (ex: `+33612345678`)

### Réponse

```json
[
  {
    "info": "please make sure the phone is connected and inSIM is running",
    "result": "success",
    "errors": []
  }
]
```

**Champs de la réponse :**
- `info` (String) : Message informatif
- `result` (String) : Résultat de l'opération (`"success"` ou `"failed"`)
- `errors` (Array) : Tableau des codes d'erreur (vide si succès)

**Codes d'erreur :**
- `#001` : Our servers are down
- `#002` : Phone not connected, inSIM not running on the phone

**Valeurs de résultat :**
- `"success"` : L'information est arrivée avec succès sur nos serveurs
- `"failed"` : La requête a échoué

## 🔧 Exemples avec d'autres outils

### Avec HTTPie

```bash
http POST https://www.insim.app/api/v1/sendsms.php \
  Content-Type:application/json \
  header:='{"login":"votre-email@example.com","accessKey":"votre-clé-d'\''accès"}' \
  messages:='[{"phone_number":"+33612345678","message":"Test","url":"","priorite":1,"date_to_send":"2025-10-29 10:16:10"}]'
```

### Avec Postman

1. Méthode : `POST`
2. URL : `https://www.insim.app/api/v1/sendsms.php`
3. Headers : `Content-Type: application/json`
4. Body (raw JSON) :
```json
{
  "header": {
    "login": "votre-email@example.com",
    "accessKey": "votre-clé-d'accès"
  },
  "messages": [
    {
      "phone_number": "+33612345678",
      "message": "Test depuis Postman",
      "url": "",
      "priorite": 1,
      "date_to_send": "2025-10-29 10:16:10"
    }
  ]
}
```

### Avec Python (requests)

```python
import requests
import json

url = "https://www.insim.app/api/v1/sendsms.php"
headers = {"Content-Type": "application/json"}
data = {
    "header": {
        "login": "votre-email@example.com",
        "accessKey": "votre-clé-d'accès"
    },
    "messages": [
        {
            "phone_number": "+33612345678",
            "message": "Test depuis Python",
            "url": "",
            "priorite": 1,
            "date_to_send": "2025-10-29 10:16:10"
        }
    ]
}

response = requests.post(url, headers=headers, json=data)
print(response.json())
```

### Avec PHP

```php
<?php
$url = "https://www.insim.app/api/v1/sendsms.php";
$data = [
    "header" => [
        "login" => "votre-email@example.com",
        "accessKey" => "votre-clé-d'accès"
    ],
    "messages" => [
        [
            "phone_number" => "+33612345678",
            "message" => "Test depuis PHP",
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

## 📝 Notes importantes

- Tous les numéros de téléphone doivent être au format international avec le préfixe `+` (ex: `+33612345678` pour la France)
- Le format de date pour `date_to_send` est : `"YYYY-MM-DD HH:mm:ss"` (ex: `"2025-10-29 10:16:10"`)
- Le champ `[Lien_Tracking]` dans le message sera remplacé par l'URL de tracking générée automatiquement
- Assurez-vous que votre clé d'accès API est valide et active
- En cas d'erreur, vérifiez les codes d'erreur retournés dans la réponse

## 🔗 Liens

- [Documentation Node.js Module](https://github.com/ArdaryinSIM/insim-api)
- [InSim Website](https://ardary-insim.com/)

## 📄 Licence

MIT

## 👤 Auteur

ArdaryinSIM

