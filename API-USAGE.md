# Student ID Card Generator API

A Node.js API for generating customizable student ID cards with various templates and styles.

---

## Base URL

```
https://id-livid-phi.vercel.app
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
curl "https://id-livid-phi.vercel.app/generate?name=Budi%20Santoso&university_name=Universitas%20Gadjah%20Mada&address=Bulaksumur,%20Yogyakarta" --output kartu_ugm.png
```

### Example 2: Indonesian Universities

```bash
# Institut Teknologi Bandung
curl "https://id-livid-phi.vercel.app/generate?name=Siti%20Nurhaliza&university_name=Institut%20Teknologi%20Bandung&address=Jl.%20Ganesha%20No.10,%20Bandung&template=2&style=3" --output kartu_itb.png

# Universitas Indonesia
curl "https://id-livid-phi.vercel.app/generate?name=Ahmad%20Dahlan&university_name=Universitas%20Indonesia&address=Depok,%20Jawa%20Barat" --output kartu_ui.png
```

### Example 3: POST Request with Custom University

```bash
curl -X POST https://id-livid-phi.vercel.app/generate \
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
curl "https://id-livid-phi.vercel.app/generate?name=John%20Doe&country=129&template=1&style=2" --output card_usa.png
```

### Example 5: JavaScript/Node.js with Custom University

```javascript
const axios = require('axios');
const fs = require('fs');

async function generateIDCard() {
  const response = await axios({
    method: 'post',
    url: 'https://id-livid-phi.vercel.app/generate',
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

url = "https://id-livid-phi.vercel.app/generate"
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
response = requests.post('https://id-livid-phi.vercel.app/generate', json={
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

### Example 8: Direct Browser Access

Simply paste this URL in your browser:

```
https://id-livid-phi.vercel.app/generate?name=Test&university_name=Universitas%20Gadjah%20Mada&address=Yogyakarta
```

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
**Base URL:** https://id-livid-phi.vercel.app  
**New Features:** Custom university name and address support
