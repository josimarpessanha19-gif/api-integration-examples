# 🔗 API Integration Examples

> Exemplos práticos e prontos para uso de integração com diversas APIs usando Python e Node.js.

[![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![Node.js](https://img.shields.io/badge/Node.js-43853D?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org)
[![Author](https://img.shields.io/badge/Author-Josimar%20Pessanha-blueviolet?style=flat-square)](https://github.com/josimarpessanha19-gif)

---

## 📚 Exemplos Disponíveis

### 🔑 Autenticação OAuth 2.0
```python
import requests

class OAuth2Client:
    def __init__(self, client_id, client_secret, token_url):
        self.client_id = client_id
        self.client_secret = client_secret
        self.token_url = token_url
        self.access_token = None

    def authenticate(self) -> str:
        response = requests.post(self.token_url, data={
            "grant_type": "client_credentials",
            "client_id": self.client_id,
            "client_secret": self.client_secret
        })
        self.access_token = response.json()["access_token"]
        return self.access_token

    def get(self, url: str, **kwargs) -> dict:
        if not self.access_token:
            self.authenticate()
        headers = {"Authorization": f"Bearer {self.access_token}"}
        return requests.get(url, headers=headers, **kwargs).json()
```

### 📨 Webhook Handler (Node.js)
```javascript
const express = require('express');
const crypto = require('crypto');

const app = express();
app.use(express.json());

// Verificar assinatura do webhook
function verifySignature(payload, signature, secret) {
    const expected = crypto
        .createHmac('sha256', secret)
        .update(JSON.stringify(payload))
        .digest('hex');
    return `sha256=${expected}` === signature;
}

app.post('/webhook', (req, res) => {
    const signature = req.headers['x-hub-signature-256'];
    
    if (!verifySignature(req.body, signature, process.env.WEBHOOK_SECRET)) {
        return res.status(401).json({ error: 'Assinatura inválida' });
    }
    
    console.log('Evento recebido:', req.body.event);
    // Processar evento...
    res.json({ received: true });
});
```

### 🔄 Retry com Exponential Backoff (Python)
```python
import time
import requests
from functools import wraps

def retry(max_attempts=3, backoff_factor=2):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except requests.RequestException as e:
                    if attempt == max_attempts - 1:
                        raise e
                    wait = backoff_factor ** attempt
                    print(f"Tentativa {attempt + 1} falhou. Aguardando {wait}s...")
                    time.sleep(wait)
        return wrapper
    return decorator

@retry(max_attempts=3, backoff_factor=2)
def fetch_data(url: str) -> dict:
    response = requests.get(url, timeout=10)
    response.raise_for_status()
    return response.json()
```

---

## 🌐 APIs Integradas

| API | Tipo | Linguagem |
|-----|------|-----------|
| Stripe | Pagamentos | Python + Node.js |
| SendGrid | E-mail | Python |
| Twilio / WhatsApp | Mensagens | Python + Node.js |
| AWS S3 | Storage | Python (boto3) |
| OpenAI | IA | Python |
| Mercado Pago | Pagamentos BR | Python + Node.js |
| ViaCEP | CEP Brasil | Python + Node.js |
| IBGE | Dados BR | Python |

---

## 🚀 Como Usar

```bash
git clone https://github.com/josimarpessanha19-gif/api-integration-examples.git
cd api-integration-examples

# Python
pip install -r requirements.txt
cp .env.example .env
python examples/oauth2_client.py

# Node.js
npm install
cp .env.example .env
node examples/webhook_handler.js
```

---

## 👨‍💻 Autor

**Josimar Pessanha** — Full Stack Developer

📧 josimarpessanha19@gmail.com | [LinkedIn](https://www.linkedin.com/in/josimar-pessanha-de-aguiar-7a99a9356/) | [GitHub](https://github.com/josimarpessanha19-gif)
