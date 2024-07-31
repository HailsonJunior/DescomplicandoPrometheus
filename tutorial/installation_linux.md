# Instalação do Prometheus

## Pré-requisitos
Certifique-se de ter privilégios de superusuário para realizar as operações abaixo.

## Passo 1: Criar Usuário e Grupo

Crie um usuário e grupo dedicados para o Prometheus:

```bash
sudo addgroup --system prometheus
sudo adduser --system --shell /sbin/nologin --ingroup prometheus prometheus
```

## Passo 2: Criar Diretórios Necessários

Crie os diretórios para os arquivos de configuração e dados:

```bash
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```

## Passo 3: Baixar o Prometheus

Baixe a versão mais recente do Prometheus a partir do site oficial. Como exemplo:

```bash
curl -LO https://github.com/prometheus/prometheus/releases/download/v2.53.1/prometheus-2.53.1.linux-amd64.tar.gz
```

Extraia o arquivo:

```bash
tar -xvzf prometheus-2.53.1.linux-amd64.tar.gz
cd prometheus-2.53.1.linux-amd64/
```

## Passo 4: Mover Arquivos para o Diretório de Configuração

Mova os arquivos de configuração e diretórios necessários para /etc/prometheus:

```bash
sudo mv consoles/ console_libraries/ prometheus.yml /etc/prometheus/
```

Mova os binários do Prometheus para o diretório /usr/local/bin

```bash
sudo mv prometheus promtool /usr/local/bin/
```

## Passo 5: Ajustar Permissões

Defina as permissões corretas para os diretórios e arquivos:

```bash
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus
```

## Passo 6: Configurar o prometheus.yml

Edite o arquivo de configuração /etc/prometheus/prometheus.yml com o seguinte conteúdo básico:

```
global:
  scrape_interval: 15s 
  evaluation_interval: 15s 

rule_files:

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
```

## Passo 7: Configurar o Serviço do Systemd

Crie um arquivo de serviço para o Prometheus:

```bash
sudo vim /etc/systemd/system/prometheus.service
```

Adicione o seguinte conteúdo:

```
[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.external-url=

SyslogIdentifier=prometheus
Restart=always

[Install]
WantedBy=multi-user.target
```

## Passo 8: Iniciar e Habilitar o Serviço

Recarregue o daemon do systemd e inicie o serviço do Prometheus:

```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
```

## Passo 9: Acessar o Prometheus

Abra o navegador e acesse http://localhost:9090 para verificar se o Prometheus está funcionando corretamente.

Esta documentação cobre os passos básicos para a instalação e configuração do Prometheus no Linux. Para configurações adicionais e uso avançado, consulte a documentação oficial do Prometheus.