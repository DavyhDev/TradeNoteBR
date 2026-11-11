<p style='font-size:2.5em;' align="center">TradeNote</p>

<p align="center"><a href="https://tradenote.co">🌐 Website do projeto original</a> | <a href="https://tradenote.co/project-overview.html">📚 Documentation</a> |  <a href="https://discord.gg/ZbHekKYb85"><img src="https://f003.backblazeb2.com/file/7ak-public/tradenote/discord-icon.svg" width="12" height="12"/> Discord</a><p>
<br />
    
**Trading journal open source para o mercado brasileiro (B3).** Registre e analise suas operações de
Day Trade em mini índice (WIN), mini dólar (WDO) e ações, com valores em reais, custos operacionais
brasileiros e apuração estimada de imposto de renda.

O TradeNoteBR é um **projeto derivado do [TradeNote](https://github.com/Eleven-Trading/TradeNote)**,
criado por Eleven Trading (eleven.trading). Veja a seção [Origem e licença](#origem-e-licença).

## Funcionalidades

- **Importação do Profit (Nelogica)** pelos CSVs da *Lista de Ordens* ou do *Relatório de
  Performance*, com prévia antes de gravar e bloqueio de importações duplicadas.
- **Registro manual de operações**, com resultado calculado enquanto você digita.
- **Reconhecimento automático** de WIN, WDO, IND, DOL (com vencimentos, ex.: `WINZ26`) e ações (`PETR4`):
  multiplicador, tick, pontos e ticks.
- **Cálculos em decimal exato**, com preço médio ponderado, encerramentos parciais e viradas de mão.
- **Custos por corretora** (corretagem, emolumentos, registro/liquidação, ISS), editáveis.
  Gross P&L, custos e Net P&L aparecem separados.
- **Impostos (estimativa)**: day trade × swing, compensação de prejuízos, isenção de R$ 20 mil, IRRF
  e DARF. É uma estimativa, não uma declaração fiscal.
- Tudo o que o TradeNote já oferecia: Dashboard, Daily, Calendário, Diário, Screenshots com anotações,
  Playbook, tags, MFE, filtros e importadores de corretoras americanas.
- Interface em português (pt-BR), R$ e horário de Brasília.
- **Uso local de usuário único**, sem tela de login, e sem telemetria.

## Início rápido

Requisitos: Ubuntu ou Debian, com `sudo`.

```bash
git clone <url-do-seu-repositório> tradenotebr
cd tradenotebr
./setup.sh
```

O `setup.sh` instala o MongoDB, o Node 18 e as dependências, gera o `.env` com senhas aleatórias e
deixa a aplicação rodando como serviço: em segundo plano, iniciando junto com o computador. Depois,
acesse **http://tradenotebr.localhost** (via Apache, porta 80) ou http://127.0.0.1:8080.

| Comando | O que faz |
|---|---|
| `./setup.sh start` / `stop` / `restart` | Controla o serviço |
| `./setup.sh status` / `logs` | Situação e logs |
| `./setup.sh backup` | Backup do banco em `backups/` |
| `./setup.sh restore ARQUIVO` | Restaura um backup |
| `./setup.sh help` | Todas as opções |

A documentação completa está em **[docs/BR.md](docs/BR.md)**: configuração, importação do Profit,
regras financeiras e tributárias, fontes, alterações no banco, testes e limitações conhecidas.

## Desenvolvimento

```bash
./setup.sh start --dev    # roda no terminal, com recarga automática ao editar o código
npm test                  # testes automatizados (vitest)
```

A aplicação usa Node 18, que o `setup.sh` instala via nvm.

## Origem e licença

O TradeNoteBR é uma obra derivada do **TradeNote**, Copyright © Eleven Trading, distribuído sob a
**GNU General Public License v3.0 ou posterior**. Este projeto mantém a mesma licença
(GPL-3.0-or-later); veja [LICENSE](LICENSE).

**Modificações em relação ao original** (a partir do commit `56eb096`, versão 19.0.3, em novembro de 2026):

- adaptação ao mercado brasileiro (instrumentos B3, BRL, custos, impostos);
- importador do Profit e registro manual;
- tradução da interface para pt-BR;
- modo local de usuário único;
- endurecimento de segurança (acesso restrito à própria máquina, master key aceita só a partir do
  próprio servidor);
- execução local como serviço do sistema (`setup.sh`), no lugar do Docker;
- remoção da telemetria, da assinatura em nuvem (Stripe) e das consultas de versão ao projeto original.

O detalhamento está em [docs/BR.md](docs/BR.md) e no histórico do git. O README original do TradeNote
está preservado em [docs/README-TradeNote-original.md](docs/README-TradeNote-original.md).

"TradeNote" é o nome do projeto original. O TradeNoteBR não é afiliado à Eleven Trading nem endossado
por ela. Profit e Nelogica são marcas de seus respectivos titulares.

Este software é fornecido "como está", sem garantias (veja a GPL). As estimativas tributárias não
constituem consultoria fiscal.






There are numerous great and very powerful trading journals out there. However, I wanted to build a journal for traders who care about data security and privacy but also for individuals that need simplicity and flexibility.

By creating and sharing TradeNote as an open source project, I hope to help other days traders like myself store, discover and recollect trade patterns so they can become and remain consistent and profitable traders.

![dashboard](https://tradenote.co/screenshots/dashboard2.png "Dashboard")


# Installation
For detailed installation and user guide, please visit the [documentation](https://tradenote.co/project-overview.html "documentation") page.
## Docker Compose
### Requirements
- Docker
- Docker Compose

### Installation
1. Download the docker compose.yml file
2. Run `docker compose up -d`

This will automatically setup the database (mongodDB) and the TradeNote app.

You can then access the website on http://localhost:8080.

If you cannot access the website, please refer to the importante notice above (and try changing the mongo version) or get support via [Discord](https://discord.gg/ZbHekKYb85 "Discord")

## Docker
### Requirements
- Docker
- Node 18.X
- MongoDB

### Installation
You need to have a running MongoDB database. Please see their [Docker Hub](https://hub.docker.com/_/mongo "Docker Hub") for instructions.

Then, run the TradeNote image with its environment variables.

```
docker run \
    -e MONGO_URI=<mongo_uri> \
    -e TRADENOTE_DATABASE=<tradenote_database>
    -e APP_ID=<app_id> \
    -e MASTER_KEY=<master_key> \
    -e TRADENOTE_PORT=<tradenote_port> \
    -p <tradenote_port>:<tradenote_port> \
    --name tradenote_app \
    -d eleventrading/tradenote
```
- **MONGO_URI**: The MongoDB connection string, with tradenote database name and enforced access control ([explanation](https://www.mongodb.com/docs/manual/reference/connection-string/ "explanation")). You can use any MongoDB instance you like. If you are using Docker, you can use the default MongoDB instance. (example: mongodb://tradenote:tradenote@mongo:27017/tradenote?authSource=admin)
- **TRADENOTE_DATABASE**: The TradeNote database name in the MongoDB (example: tradenote)
- **APP_ID**: Set a random string as application ID, which will be used to connect to the backend (no spaces) (example: 12345).
- **MASTER_KEY**: Set a random string as master key, which will be used to make root connections to the backend (no spaces) (example: 12345)
- **TRADENOTE_PORT**: TradeNote port number, from which you wish to serve the website. (example: 8080)


# Quick Start
## Registering a User
Start by registering a user. Visit `http://localhost:8080/register` to register a TradeNote user.
- Use any (random) email and set a password.
- Choose your broker and/or account timezone.

## Importing Trades
Please make sure to follow the instructions in the <a href="https://tradenote.co/brokers.html">brokers folder</a> for exporting and importing trades. 

# Contribute
I'm a trader and recreational developer. My days are very packed but I will do my best to answer your questions and update the code when needed. As such, do not hesitate to contact me if you would like to contribute and help improve this project. Things to work on and improve:
- Add support to other trading platforms
- Improve front end layout and develop new ideas
- And more...

# License
This project is open sourced under the GNU GPL v3 licence.
