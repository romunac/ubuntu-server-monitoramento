# Projeto Ubuntu Server - Infraestrutura e Segurança de Redes

## Descrição

Este projeto foi desenvolvido para a disciplina de Introdução à Computação e apresenta uma solução de infraestrutura utilizando Ubuntu Server como servidor principal para gerenciamento de serviços, armazenamento de dados e acesso remoto seguro.

O objetivo é demonstrar a implementação de um ambiente resiliente, conectado e protegido, seguindo os princípios de disponibilidade, integridade e confiabilidade.

## Objetivos

- Implementar um servidor utilizando Ubuntu Server.
- Disponibilizar serviços de rede de forma segura.
- Aplicar mecanismos de proteção contra falhas de hardware, software e segurança.
- Demonstrar boas práticas de administração de sistemas Linux.

## Arquitetura da Rede

Fluxo de dados:

Internet → Roteador → Firewall (UFW) → Switch → Ubuntu Server

Tecnologias utilizadas:

- Ubuntu Server 24.04 LTS
- Ethernet Gigabit
- Wi-Fi
- HTTPS
- SSH
- UFW Firewall
- Fail2Ban

## Matriz de Risco

### Falha de Hardware
- Falta de energia elétrica
- Impacto: Alto

### Falha de Software
- Configuração incorreta de permissões
- Impacto: Alto

### Falha de Rede/Segurança
- Ataque de força bruta via SSH
- Impacto: Alto

## Medidas de Mitigação

### Hardware
- Utilização de nobreak (UPS)
- Backups periódicos

### Software
- Atualizações regulares
- Controle de permissões

### Rede e Segurança
- Firewall UFW
- Fail2Ban
- Autenticação SSH por chave pública
- Segmentação por VLAN

## Comandos Utilizados
```bash
sudo apt update
sudo apt upgrade -y
### Atualização do sistema

```

```mermaid
graph LR
    %% Definição de Estilos
    classDef server fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef stack fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef user fill:#fff3e0,stroke:#f57c00,stroke-width:2px;

    %% Nó Ubuntu Server (Alvo)
    subgraph UBUNTU_SERVER ["Ubuntu Server (Host Monitorado)"]
        direction TB
        OS["Ubuntu OS / Kernel"]
        
        subgraph AGENTS ["Agentes de Coleta"]
            NE["Node Exporter <br> (Métricas de Hardware/SO)"]
            PT["Promtail / Logstash <br> (Coleta de Logs)"]
            CA["cAdvisor <br> (Métricas de Containers)"]
        end
        
        OS --> NE
        OS --> PT
    end
    class UBUNTU_SERVER server;

    %% Nó Stack de Monitoramento
    subgraph MONITORING_STACK ["Servidor de Monitoramento (Docker Stack)"]
        direction TB
        PROM[("Prometheus <br> (Banco de Métricas)")]
        LOKI[("Grafana Loki <br> (Banco de Logs)")]
        GRAFANA{"Grafana <br> (Visualização)"}
        
        PROM --> GRAFANA
        LOKI --> GRAFANA
    end
    class MONITORING_STACK stack;

    %% Nó de Acesso
    subgraph CLIENT ["Acesso do Administrador"]
        ADMIN["DevOps / SysAdmin <br> (Navegador Web)"]
        ALERT["Alertmanager <br> (Discord/Slack/E-mail)"]
    end
    class CLIENT user;

    %% Conexões de Rede e Fluxo de Dados
    NE -- "Porta 9100 (HTTP Pull)" --> PROM
    CA -- "Porta 8080 (HTTP Pull)" --> PROM
    PT -- "Porta 3100 (HTTP Push)" --> LOKI
    
    GRAFANA -- "Porta 3000 (HTTPS)" --> ADMIN
    PROM --> ALERT

```

```bash
sudo apt update
sudo apt upgrade -y

