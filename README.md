# 🔗 API Integration Examples
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

📧 josimarpessanha19@gmail.com | [LinkedIn](https://www.linkedin.com/in/josimar-pessanha-de-aguiar-545398404/) | [GitHub](https://github.com/josimarpessanha19-gif)
