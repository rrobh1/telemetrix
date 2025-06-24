---

# 🚀 Infra Stack: Observabilidade + DevTools com Docker

Este projeto configura uma stack completa de infraestrutura para desenvolvimento e observabilidade, utilizando Docker Compose. Inclui Traefik, Grafana, Loki, Prometheus, MinIO, Portainer, Zipkin, PostgreSQL, entre outros.

---

## 🧱 Componentes Incluídos

| Serviço           | Porta Padrão | URL (via Traefik)                                  | Finalidade                       |
| ----------------- | ------------ | -------------------------------------------------- | -------------------------------- |
| **Traefik**       | 80 / 8080    | [http://traefik.local](http://traefik.local)       | Proxy reverso + dashboard        |
| **Grafana**       | 3000         | [http://grafana.local](http://grafana.local)       | Visualização de métricas/logs    |
| **Loki**          | 3100         | [http://loki.local](http://loki.local)             | Logs centralizados               |
| **Prometheus**    | 9090         | [http://prometheus.local](http://prometheus.local) | Coleta de métricas               |
| **Promtail**      | 9080         | [http://promtail.local](http://promtail.local)     | Agente de logs                   |
| **Node Exporter** | 9100         | [http://node.local](http://node.local)             | Métricas do host                 |
| **MinIO**         | 9001         | [http://minio.local](http://minio.local)           | S3 local para armazenamento      |
| **Zipkin**        | 9411         | [http://zipkin.local](http://zipkin.local)         | Tracing de requisições           |
| **Portainer**     | 9000         | [http://portainer.local](http://portainer.local)   | Gerenciador de containers Docker |
| **PostgreSQL**    | 5432         | —                                                  | Banco de dados                   |
| **Pg Exporter**   | 9187         | [http://pgexporter.local](http://pgexporter.local) | Métricas do PostgreSQL           |
| **cAdvisor**      | —            | —                                                  | Métricas de containers           |
| **Blackbox**      | 9115         | [http://blackbox.local](http://blackbox.local)     | Monitoramento de alvos externos  |

---

## 📁 Estrutura de Arquivos

```bash
Projetos/infra/
├── .env                  # Variáveis de ambiente (senhas, usuários)
├── docker-compose.yml    # Compose principal
├── start-infra.sh        # Inicia a stack
├── stop-infra.sh         # Encerra a stack
├── loki.yaml             # Configuração do Loki
├── promtail-config.yml   # Configuração do Promtail
├── prometheus.yml        # Configuração do Prometheus
├── blackbox.yml          # Configuração do Blackbox Exporter
└── telemetrix.desktop   # Atalho de desktop com ação de Start e Stop
```

---

## ⚙️ Instalação

### 1. Clone o repositório

```bash
git clone https://github.com/seu-rrobh1/telemetrix.git ~/Projetos/infra
cd ~/Projetos/infra
```

### 2. Configure variáveis de ambiente

Edite o arquivo `.env`:

```env
DEV_USER=admin
DEV_PASS=admin123
```

---

## ▶️ Inicialização Rápida

### Via terminal:

```bash
chmod +x start-infra.sh stop-infra.sh
./start-infra.sh
```

### Via atalho de desktop (Linux)

1. Crie o arquivo `~/.local/share/applications/telemetrix.desktop`:

```ini
[Desktop Entry]
Version=1.0
Type=Application
Name=Telemetrix
Comment=Inicia a stack de infraestrutura (Traefik, Grafana etc)
Exec=/home/SEU_USUARIO/Projetos/infra/start-infra.sh
Icon=docker
Terminal=true
Categories=Development;
Actions=Stop;OpenGrafana;OpenTraefik;OpenPortainer;OpenMinio;OpenZipkin;

[Desktop Action Stop]
Name=Parar Stack
Exec=/home/robson/WWW/Infra/stop-infra.sh
Terminal=true

[Desktop Action OpenGrafana]
Name=Abrir Grafana
Exec=firefox http://grafana.local
Terminal=false

[Desktop Action OpenTraefik]
Name=Abrir Traefik
Exec=firefox http://traefik.local
Terminal=false

[Desktop Action OpenPortainer]
Name=Abrir Portainer
Exec=firefox http://portainer.local
Terminal=false

[Desktop Action OpenMinio]
Name=Abrir Minio
Exec=firefox http://minio.local
Terminal=false

[Desktop Action OpenZipkin]
Name=Abrir Zipkin
Exec=firefox http://zipkin.local
Terminal=false
```

2. Torne executável:

```bash
chmod +x ~/.local/share/applications/telemetrix.desktop
```

3. Pressione `Super` (tecla Windows), digite **Telemetrix** e fixe na barra com clique direito → **Adicionar aos Favoritos**.

> ❗ Lembre-se de substituir `SEU_USUARIO` pelo seu nome real de usuário (verifique com `whoami`).

---

## 🌐 Acesso aos Serviços

Adicione ao seu `/etc/hosts`:

```bash
127.0.0.1 traefik.local grafana.local prometheus.local loki.local promtail.local portainer.local minio.local zipkin.local pgexporter.local blackbox.local node.local
```

---

## 📌 Requisitos

* Docker
* Docker Compose
* Linux (testado no Ubuntu 22.04)
* Permissão para editar o arquivo `/etc/hosts`

---

## 📊 Métricas e Logs

| Origem     | Destino                      | Visualização       |
| ---------- | ---------------------------- | ------------------ |
| Prometheus | Node, PG, Blackbox, cAdvisor | Grafana dashboards |
| Loki       | Promtail                     | Grafana Explore    |
| Zipkin     | App (ex: Fastify)            | Web UI do Zipkin   |

---

## 🧪 Observações

* O Traefik está com dashboard exposto em `http://traefik.local:8080`.
* Para ambientes de produção, é recomendado adicionar HTTPS, autenticação e desativar o painel inseguro.
* Portas não expostas diretamente são roteadas via Traefik com nomes `.local`.

---

## 📄 Licença

MIT © [Robson Rodrigues](https://github.com/rrobh1)

---
