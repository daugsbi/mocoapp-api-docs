# Offers

German: "Angebote"

## Attributes

The offer representation contains among the standard fields:

- positions (items)
- position types ("title", "description", "item", "subtotal", "page-break" or "separator")

```json5
{
  "id": 273,
  "identifier": "A1704-042",
  "date": "2017-04-12",
  "due_date": "2017-04-26",
  "title": "Offer - User Management",
  "recipient_address": "Beispiel GmbH\nPeter Muster\nBeispielstrasse 123\n12345 Berlin",
  "currency": "EUR",
  "net_total": 12750,
  "tax": 19,
  "gross_total": 15172.5,
  "discount": 10,
  "created_on": "2017-03-24", // 🚧 DEPRECATED: use created_at
  "updated_on": "2016-04-12", // 🚧 DEPRECATED: use updated_at
  "salutation": "Hallo Peter<br><br>Folgende Aufwände schätzen wir für die Umsetzung der Komponenten:",
  "footer": "Für Rückfragen stehe ich dir jederzeit gerne zur Verfügung.<br><br>Viele Grüsse<br><br>Tobias",
  "company": {
    "id": 1234,
    "name": "Acme Corp."
  },
  "project": {
    "id": 1234,
    "name": "A Project"
  },
  "deal": {
    "id": 1234,
    "name": "A Lead"
  },
  "items": [
    {
      "id": 29,
      "type": "item",
      "title": "Project Setup",
      "description": null,
      "quantity": 1,
      "unit": "d",
      "unit_price": 1500,
      "net_total": 1500,
      "optional": false
    },
    {
      "id": 30,
      "type": "item",
      "title": "Master Data",
      "description": null,
      "quantity": 3,
      "unit": "d",
      "unit_price": 1500,
      "net_total": 4500,
      "optional": false
    },
    {
      "id": 31,
      "type": "description",
      "title": null,
      "description": "Master data can be added.",
      "quantity": 0,
      "unit": null,
      "unit_price": 0,
      "net_total": 0,
      "optional": false
    },
    {
      "id": 34,
      "type": "item",
      "title": "OAuth Provider (Single Sign On)",
      "description": null,
      "quantity": 4,
      "unit": "d",
      "unit_price": 1500,
      "net_total": 6000,
      "optional": false
    },
    {
      "id": 35,
      "type": "description",
      "title": null,
      "description": "This component runs centrally and provides an OAuth Provider.<br>Other applications can access this authorization service.",
      "quantity": 0,
      "unit": null,
      "unit_price": 0,
      "net_total": 0,
      "optional": false
    },
    {
      "id": 38,
      "type": "item",
      "title": "Project Management / Communication / Hand-Over",
      "description": null,
      "quantity": 0.5,
      "unit": "d",
      "unit_price": 1500,
      "net_total": 750,
      "optional": false
    }
  ],
  "created_at": "2018-10-17T09:33:46Z",
  "updated_at": "2018-10-17T09:33:46Z"
}
```

## GET /offers

Retrieve all offers:

```bash
curl -X GET \
  'https://{domain}.mocoapp.com/api/v1/offers' \
  -H 'Authorization: Token token=YOUR_API_KEY'
```

This returns an array with complete offer information (see Attributes), except: `salutation`, `footer` and `items`.

Offers can be sorted by: `date`, `created_at` and `title`:

```bash
curl -X GET \
  'https://{domain}.mocoapp.com/api/v1/offers?sort=date' \
  -H 'Authorization: Token token=YOUR_API_KEY'
```

Additionally, these parameters can be supplied:

- **status** – ("created", "sent", "accepted", "partially_billed", "billed", "archived")
- **from** – "2018-01-01"
- **to** – "2018-01-31"
- **identifier** "A1903-003"

## GET /offers/{id}

Retrieve a single offer:

```bash
curl -X GET \
  'https://{domain}.mocoapp.com/api/v1/offers/{id}' \
  -H 'Authorization: Token token=YOUR_API_KEY'
```

This returns a single offer representation, including positions.

## GET /offers/{id}.pdf

Retrieve a single offer document:

```bash
curl -X GET \
  'https://{domain}.mocoapp.com/api/v1/offers/{id}.pdf' \
  -H 'Authorization: Token token=YOUR_API_KEY'
```

Additionally, the following parameters can be supplied:

- **letter_paper_id** – (letter paper ID, default: White)

This returns this offers's PDF document.

## POST /offers

Create an offer:

```bash
curl -X POST \
  'https://{domain}.mocoapp.com/api/v1/offers' \
  -H 'Authorization: Token token=YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
        "deal_id": 123,
        "recipient_address": "Acme\r\nPark Avenue",
        "date": "2019-05-10",
        "due_date": "2019-06-08",
        "title": "Offer - Shop Locator",
        "salutation": "Hey",
        "tax": 8.0,
        "discount": 0.0,
        "footer": "Bye",
        "items":[
            {"type":"title","title":"Stunden"},
            {"type":"description","description":"Aufstellung über geleistete Arbeiten"},
            {"type":"item","title":"MailChimp Einrichtung","quantity":1,"unit":"Std","unit_price":100.0}
            ]}'
```

Mandatory fields are marked with a star (\*):

- **deal_id\*** – 123456 (ID of the associated deal), required if no project_id is provided
- **project_id\*** – 123456 (ID of the associated project), required if no deal_id is provided
- **recipient_address\*** – "My customer..."
- **date\*** – "2018-09-17"
- **due_date\*** – "2018-10-16"
- **title\*** – "Offer"
- **tax\*** – 8.0
- **currency\*** – "CHF" (a valid currency of the account)
- **items\*** – positions (see [attributes](#attributes))
- **change_address** – address propagation ("offer", "customer"), default is "offer"
- **salutation** (salutation text)
- **footer** (footer text)
- **discount** – 10 (discount in percent)
- **contact_id** – 123456 (ID of the associated contact)
