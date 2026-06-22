graph TD
    %% Estilos de Subgrafos
    classDef hardware fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef network fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef server fill:#fbe9e7,stroke:#d84315,stroke-width:2px;

    %% Estilos de Alertas e Mitigações
    classDef falha fill:#ffebee,stroke:#c62828,stroke-width:2px,stroke-dasharray: 5 5;
    classDef mitigacao fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;

    %% 1. Camada de Infraestrutura Física (Hardware)
    subgraph Infra [01. Infraestrutura Física e Energia]
        A[Rede Elétrica Concessionária] --> B[Filtro de Linha / QDC]
        F1{<b>FALHA DE HARDWARE</b><br/>Falta de Energia Elétrica} --- B
        M1[<b>MITIGAÇÃO</b><br/>Nobreak UPS &<br/>Backups Periódicos] -.-> B
    end
    style Infra fill:#f5f5f5,stroke:#9e9e9e

    %% 2. Camada de Rede e Perímetro (Internet ao Switch)
    subgraph Rede [02. Topologia de Rede e Conectividade]
        WAN[Internet] -->|Link WAN| Rot[Roteador]
        Rot -->|Segmentação por VLAN| SW[Switch Gigabit Ethernet / Wi-Fi]
        
        F3{<b>FALHA DE REDE/SEGURANÇA</b><br/>Ataque de Força Bruta via SSH} --- WAN
    end
    style Rede fill:#e3f2fd,stroke:#2196f3

    %% 3. Camada do Servidor (Ubuntu Server, Software e Proteções)
    subgraph Servidor [03. Ambiente Ubuntu Server 24.04 LTS]
        SW -->|Tráfego Filtrado| UFW[Firewall Ativo: UFW]
        UFW -->|Análise de Logs| F2B[Fail2Ban]
        F2B -->|Acesso Autenticado| OS[Ubuntu Server OS]
        
        OS -->|Gerenciamento| SSH[Serviço SSH]
        OS -->|Serviços Web| HTTPS[Protocolo HTTPS]
        
        F2{<b>FALHA DE SOFTWARE</b><br/>Configuração Incorreta<br/>de Permissões} --- OS
        M2[<b>MITIGAÇÃO</b><br/>Controle de Permissões estrito<br/>& Atualizações Regulares] -.-> OS
        
        M3[<b>MITIGAÇÃO DE REDE</b><br/>Autenticação por Chave Pública,<br/>Bloqueios Fail2Ban e UFW] -.-> SSH
    end
    style Servidor fill:#fff3e0,stroke:#e65100

    %% Aplicação dos Estilos
    class F1,F2,F3 falha;
    class M1,M2,M3 mitigacao;
