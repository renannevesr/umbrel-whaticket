# Umbrel WhaTicket Community App Store

Community App Store para o Umbrel que disponibiliza o
[WhaTicket Community](https://github.com/canove/whaticket-community) —
sistema de atendimento multiusuário baseado em mensagens do WhatsApp.

## Estrutura

```
umbrel-whaticket/
├── umbrel-app-store.yml          # define a loja (id: whaticket)
├── README.md
└── whaticket-app/                # o app (id: whaticket-app)
    ├── umbrel-app.yml            # metadados exibidos na UI
    ├── docker-compose.yml        # containers: db, backend, frontend
    └── data/                     # dados persistentes
```

## ANTES de instalar: edite o IP

Abra `whaticket-app/docker-compose.yml` e substitua TODAS as ocorrências de
`CHANGE_ME_UMBREL_IP` pelo IP local do seu Umbrel Home (ex.: `192.168.0.50`).
Recomenda-se também trocar `JWT_SECRET` e `JWT_REFRESH_SECRET` por valores
aleatórios.

## Como instalar

1. No Umbrel: **App Store → ⋯ → Community App Stores**.
2. Cole: `https://github.com/renannevesr/umbrel-whaticket`
3. Adicione e instale o **WhaTicket**.

A PRIMEIRA instalação é LENTA: o Umbrel constrói as imagens a partir do
código (Node 14 + download do Chrome). Em Umbrel Home (x86) funciona, mas
pode levar 15–40 minutos. É normal ficar "instalando" por muito tempo.

## Primeiro acesso

- Acesse pela porta **3000** do Umbrel.
- Login padrão: **admin@whaticket.com** / **admin**

O backend já roda migrations E seeds automaticamente, então o usuário admin
é criado sozinho. Se mesmo assim não logar, rode via SSH:

```bash
docker exec -it whaticket-app_backend_1 npx sequelize db:seed:all
```

## Diagnóstico (se travar)

Via SSH no Umbrel:

```bash
docker ps -a | grep whaticket
docker logs whaticket-app_backend_1 2>&1 | tail -60
```

## Avisos

- O WhatsApp não autoriza clientes não oficiais. Há risco de bloqueio do número.
  Use por sua conta e risco, de preferência em rede local.
