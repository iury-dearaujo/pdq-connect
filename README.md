# PDQ Connect

`PDQ Connect` is a Windows Service API that provides a REST interface to interact with [PDQ Deploy](https://www.pdq.com/pdq-deploy-and-inventory/). It allows remote triggering of deployments, monitoring, and status retrieval for software installations across a network.

## 🔧 Features

- Trigger PDQ Deploy packages via REST
- List available packages
- Monitor deployment status
- Designed to run as a Windows Service

## 📦 Technologies

- PowerShell / Python / .NET (dependendo do que usou)
- PDQ Deploy CLI
- NSSM (for service setup)

## 🚀 Getting Started

1. Install PDQ Deploy on your machine
2. Clone this repository
3. Setup the service using `nssm` or another method
4. Configure credentials and endpoints
5. Start interacting via HTTP calls

# PDQ Connect API

### Documentação: Autenticação e Configuração da API PDQ Connect

---

## 📘 Visão Geral

PDQ Connect API é uma aplicação desenvolvida com FastAPI para gerenciar operações do PDQ Deploy via interface REST segura. A API oferece funcionalidades como autenticação com JWT, execução remota de scripts PowerShell, e gerenciamento de tokens. A configuração é feita via arquivo `.env` e utiliza criptografia com bcrypt e chaves RSA.

---

## 🚀 Funcionalidades

- **🔐 Autenticação**: Login seguro com bcrypt e tokens JWT.
- **🖥️ Execução Remota**: Scripts PowerShell em computadores remotos via API.
- **🔁 Gerenciamento de Tokens**: Tokens de acesso e atualização com controle de expiração.
- **⚙️ Configuração via `.env`**: Permite ajustes finos e seguros do ambiente.

---

## 🛠️ Instalação

### Pré-requisitos

- Python 3.12+
- Poetry

### Passos

```bash
git clone <url-do-repositorio>
cd pdq-connect
poetry config virtualenvs.in-project true
poetry install --with dev --no-root --no-interaction
python ./scripts/generate_pem.py --name jwt --bits 2048
python ./scripts/scy.py --username ... --password \!...

# Configure o .env conforme abaixo
```

### Exemplo de `.env`

```dotenv
APP_NAME=pdq-connect
VERSION=0.1.0
APP_ENV=development
LOG_LEVEL=debug
STORED_HASHED_COMPOSITE={{GERADO COM O ARQ.}}
JWT_SECRET_KEY={{UUID}}
JWT_ALGORITHM=RS256
JWT_EXPIRATION_MINUTES=15
PRIVATE_KEY_PATH="jwt_private_key.pem"
PUBLIC_KEY_PATH="jwt_public_key.pem"
```

---

## ▶️ Execução

### Modo Desenvolvimento

```bash
poetry run uvicorn app.main:app --reload --log-level debug --host 127.0.0.1 --port 8081
```

### Modo Produção

```bash
poetry run python app/run.py
```

---

## 🧪 Testes

### Rodar Testes

```bash
poetry run pytest --cov=pdq_conn --cov-report=term-missing
```

### Relatório de Cobertura

```bash
pytest --cov-report=html --cov-report=term --cov=pdq_conn tests
```

---

## 📡 Endpoints

### 🔑 Login

- `POST /login`
- Autentica e retorna um token JWT.

```json
{
  "username": "admin",
  "password": "secret"
}
```

### 🔐 Autenticar Máquina

- `POST /pdq/authenticate`
- Autentica remotamente via PowerShell.

```json
{
  "computerName": "remote-pc",
  "user": "admin",
  "password": "123"
}
```

### 🛠️ Executar Script

- `POST /pdq/execute`
- Executa script em máquina remota.

```json
{
  "computerName": "remote-pc",
  "user": "admin",
  "password": "123",
  "script": "base64_script",
  "arguments": {
    "arg1": "base64_value"
  }
}
```

---

## ⏳ Expiração de Token JWT

### Implementação

1. Defina `JWT_EXPIRATION_MINUTES` no `.env`.
2. Adicione a claim `exp` ao gerar o token.
3. Verifique `exp` ao decodificar.

#### Geração

```python
expires_at = datetime.now(timezone.utc) + timedelta(minutes=int(JWT_EXPIRATION_MINUTES))
jwt_data = {
    "sub": login.username,
    "exp": int(expires_at.timestamp()),
    "iat": datetime.now(timezone.utc).timestamp(),
    "type": token_type
}
```

#### Verificação

```python
if datetime.now(timezone.utc) > datetime.fromtimestamp(payload.get("exp", 0), timezone.utc):
    raise HTTPException(status_code=403, detail="Token expirado")
```

---

## 🔐 Segurança

- Nunca compartilhe o `.env`
- Use senhas fortes e chaves RSA
- Rotacione chaves periodicamente

---

## 📄 Licença

Licenciado sob [Apache-2.0](LICENSE).

---

## 👤 Autor

**Iury Araujo** – [iury.araujo@senior.com.br](mailto:iury.de.araujo9@gmail.com)

