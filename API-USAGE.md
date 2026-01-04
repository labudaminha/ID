# Student ID Card Generator API

A Node.js API for generating customizable student ID cards with various templates and styles.

---

## Base URL

```
https://sicg.vercel.app
```

## Features

- 2 templates, 6 variants each
- Student Name
- **Custom University Name (NEW!)** - Input nama universitas bebas
- **Custom Address (NEW!)** - Input alamat universitas bebas
- College Name
- College Logo
- Student Photo
- Student ID
- Date of Birth
- Address
- Date of Issue
- Valid Until
- 130+ Country Supported
- Support for both GET and POST requests

## API Endpoints

### Health Check
```
GET /
```
Returns API information and available parameters.

### Generate ID Card
```
GET /generate
POST /generate
```

## Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `name` | string | **Yes** | - | Student name |
| `university_name` | string | No | From country data | **Custom university name (overrides country selection)** |
| `address` | string | No | From country data | **Custom university address (overrides country selection)** |
| `dob` | string | No | `2001-01-25` | Date of birth (format: YYYY-MM-DD) |
| `id` | string | No | `1` | ID format (`1` = numeric like 123-456-7890, `2` = alphanumeric like ABC123456789) |
| `id_value` | string | No | Auto-generated | Custom student ID (overrides auto-generation) |
| `academicyear` | string | No | `2025-2028` | Academic year |
| `country` | number | No | `0` | Country index (see Country ID Table below) |
| `template` | string | No | `1` | Template selection (`1` or `2`) |
| `style` | string | No | `2` | Style variant (`1` to `6`) |
| `opacity` | number | No | `0.1` | Center logo watermark opacity (0.0 to 1.0) |
| `principal` | string | No | `Osama Aziz` | Principal/Authority signature name |
| `student_photo` | string (URL) | No | Default photo | Student photo URL |
| `college_logo` | string (URL) | No | Default logo | College logo URL |
| `issue_date` | string | No | `15 AUG 2025` | Card issue date |
| `issue_txt` | string | No | `Date Of Issue` | Issue date label |
| `exp_date` | string | No | `31 DEC 2025` | Card expiration date |
| `exp_txt` | string | No | `Card Expires` | Expiration label (only for template 1) |

## Usage Examples

### Example 1: Custom University Name (NEW!)

Generate ID card with custom university name:

```bash
curl "https://sicg.vercel.app/generate?name=Budi%20Santoso&university_name=Universitas%20Gadjah%20Mada&address=Bulaksumur,%20Yogyakarta" --output kartu_ugm.png
```

### Example 2: Indonesian Universities

```bash
# Institut Teknologi Bandung
curl "https://sicg.vercel.app/generate?name=Siti%20Nurhaliza&university_name=Institut%20Teknologi%20Bandung&address=Jl.%20Ganesha%20No.10,%20Bandung&template=2&style=3" --output kartu_itb.png

# Universitas Indonesia
curl "https://sicg.vercel.app/generate?name=Ahmad%20Dahlan&university_name=Universitas%20Indonesia&address=Depok,%20Jawa%20Barat" --output kartu_ui.png
```

### Example 3: POST Request with Custom University

```bash
curl -X POST https://sicg.vercel.app/generate \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Muhammad Rizki",
    "university_name": "Institut Pertanian Bogor",
    "address": "Jl. Raya Dramaga, Bogor",
    "dob": "2003-05-15",
    "academicyear": "2024-2028",
    "template": "1",
    "style": "4",
    "issue_date": "01 SEP 2024",
    "exp_date": "31 AUG 2028"
  }' \
  --output kartu_ipb.png
```

### Example 4: Using Country Index (Original Method)

```bash
# Masih bisa pakai country index seperti biasa
curl "https://sicg.vercel.app/generate?name=John%20Doe&country=129&template=1&style=2" --output card_usa.png
```

### Example 5: JavaScript/Node.js with Custom University

```javascript
const axios = require('axios');
const fs = require('fs');

async function generateIDCard() {
  const response = await axios({
    method: 'post',
    url: 'https://sicg.vercel.app/generate',
    data: {
      name: 'Dewi Lestari',
      university_name: 'Universitas Airlangga',
      address: 'Jl. Airlangga No.4-6, Surabaya',
      dob: '2002-08-20',
      academicyear: '2024-2027',
      template: '2',
      style: '5'
    },
    responseType: 'arraybuffer'
  });

  fs.writeFileSync('student_card.png', response.data);
  console.log('ID card generated successfully!');
}

generateIDCard();
```

### Example 6: Python with Custom University

```python
import requests

url = "https://sicg.vercel.app/generate"
data = {
    "name": "Andi Wijaya",
    "university_name": "Universitas Diponegoro",
    "address": "Jl. Prof. Sudarto, Semarang",
    "dob": "2003-12-10",
    "academicyear": "2025-2028",
    "template": "1",
    "style": "3"
}

response = requests.post(url, json=data)

if response.status_code == 200:
    with open("student_card.png", "wb") as f:
        f.write(response.content)
    print("ID card generated successfully!")
else:
    print(f"Error: {response.status_code}")
```

### Example 7: Telegram Bot Integration

```python
import requests
from telegram import Bot

bot = Bot(token='YOUR_BOT_TOKEN')

# Generate card dengan universitas custom
response = requests.post('https://sicg.vercel.app/generate', json={
    "name": "Rina Kusuma",
    "university_name": "Universitas Negeri Jakarta",
    "address": "Jl. Rawamangun Muka, Jakarta",
    "template": "2",
    "style": "1"
})

# Kirim ke Telegram
if response.status_code == 200:
    bot.send_photo(chat_id=CHAT_ID, photo=response.content)
```

## Country ID Table

Use the `country` parameter with the corresponding ID to select a university from a specific country (Optional if using `university_name`):

| ID | Country | ID | Country | ID | Country |
|----|---------|----|---------|----|------------|
| 0 | Afghanistan | 46 | Guatemala | 92 | Peru |
| 1 | Albania | 47 | Guinea | 93 | Philippines |
| 2 | Algeria | 48 | Guyana | 94 | Poland |
| 3 | Argentina | 49 | Haiti | 95 | Portugal |
| 4 | Armenia | 50 | Honduras | 96 | Qatar |
| 5 | Australia | 51 | Hungary | 97 | Romania |
| 6 | Austria | 52 | Iceland | 98 | Russia |
| 7 | Azerbaijan | 53 | India | 99 | Saint Lucia |
| 8 | Bahamas | 54 | Indonesia | 100 | Samoa |
| 9 | Bahrain | 55 | Iran | 101 | San Marino |
| 10 | Bangladesh | 56 | Iraq | 102 | Saudi Arabia |
| 11 | Barbados | 57 | Ireland | 103 | Senegal |
| 12 | Belarus | 58 | Italy | 104 | Serbia |
| 13 | Belgium | 59 | Jamaica | 105 | Singapore |
| 14 | Bhutan | 60 | Japan | 106 | Somalia |
| 15 | Bolivia | 61 | Jordan | 107 | South Africa |
| 16 | Brazil | 62 | Kazakhstan | 108 | South Korea |
| 17 | Bulgaria | 63 | Kenya | 109 | South Sudan |
| 18 | Cambodia | 64 | Kosovo | 110 | Spain |
| 19 | Cameroon | 65 | Kuwait | 111 | Sri Lanka |
| 20 | Canada | 66 | Kyrgyzstan | 112 | Sudan |
| 21 | Central African Republic | 67 | Laos | 113 | Sweden |
| 22 | Chile | 68 | Latvia | 114 | Switzerland |
| 23 | China | 69 | Lebanon | 115 | Syria |
| 24 | Colombia | 70 | Liberia | 116 | Taiwan |
| 25 | Comoros | 71 | Libya | 117 | Tajikistan |
| 26 | Costa Rica | 72 | Lithuania | 118 | Tanzania |
| 27 | Côte d'Ivoire | 73 | Luxembourg | 119 | Thailand |
| 28 | Croatia | 74 | Madagascar | 120 | Togo |
| 29 | Denmark | 75 | Malaysia | 121 | Trinidad and Tobago |
| 30 | Dominica | 76 | Maldives | 122 | Tunisia |
| 31 | Dominican Republic | 77 | Mexico | 123 | Turkey |
| 32 | Ecuador | 78 | Monaco | 124 | Turkmenistan |
| 33 | Egypt | 79 | Mongolia | 125 | Uganda |
| 34 | El Salvador | 80 | Morocco | 126 | Ukraine |
| 35 | Eritrea | 81 | Myanmar | 127 | United Arab Emirates |
| 36 | Estonia | 82 | Namibia | 128 | United Kingdom |
| 37 | Ethiopia | 83 | Nepal | 129 | United States |
| 38 | Finland | 84 | Netherlands | 130 | Uruguay |
| 39 | France | 85 | New Zealand | 131 | Uzbekistan |
| 40 | Gambia | 86 | Nigeria | 132 | Vatican City |
| 41 | Georgia | 87 | North Korea | 133 | Venezuela |
| 42 | Germany | 88 | Norway | 134 | Vietnam |
| 43 | Ghana | 89 | Oman | 135 | Yemen |
| 44 | Greece | 90 | Pakistan | 136 | Zambia |
| 45 | Grenada | 91 | Panama | 137 | Zimbabwe |

## Response

The API returns a PNG image with:
- **Content-Type:** `image/png`
- **Dimensions:** 1280x804 pixels
- The image is returned directly and can be saved or displayed

## Error Response

If required parameters are missing or an error occurs:

```json
{
  "error": "Name parameter is required"
}
```

or

```json
{
  "error": "Failed to generate ID card",
  "message": "Detailed error message"
}
```

## What's New in v1.1.0

✅ **Custom University Name** - Sekarang bisa input nama universitas sendiri  
✅ **Custom Address** - Bisa input alamat universitas custom  
✅ **Backward Compatible** - Country index masih tetap berfungsi  
✅ **Flexible** - Tidak terbatas pada 138 negara yang ada di database  

## Template & Style Preview

- **Template 1:** Classic design with red accents
- **Template 2:** Modern design with shadow effects
- **Styles 1-6:** Different color schemes and layouts for each template

## Technical Details

- **Built with:** Node.js, Express.js, Canvas
- **Image Processing:** node-canvas library
- **Fonts:** Times, AlexBrush
- **Deployment:** Vercel

---

**API Version:** 1.1.0  
**Last Updated:** January 2026  
**New Features:** Custom university name and address support
