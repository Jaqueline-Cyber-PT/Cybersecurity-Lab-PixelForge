# 🛡️ Laboratório de Cibersegurança: Auditoria e Hardening (Active Directory)

Bem-vindo ao meu repositório de portfólio. Este projeto documenta a implementação, auditoria de segurança e hardening de uma infraestrutura baseada em **Windows Server 2022**.

---

## 🚩 Fase 1: Reconhecimento e Análise de Superfície de Ataque

### 📋 Visão Geral
Nesta fase inicial, o objetivo foi atuar como um auditor externo para identificar todos os serviços expostos no Controlador de Domínio (**PF-DC01**) e verificar se as configurações de segurança iniciais (Hardening) estavam a bloquear ataques de enumeração comuns.

### 🛠️ Ferramentas e Tecnologias
* **SO Alvo:** Windows Server 2022 (Domain Controller).
* **Plataforma de Ataque:** Kali Linux 2024.
* **Ferramentas:** `Nmap` (Port scanning & NSE Scripts), `Enum4linux-ng` (SMB/RPC enumeration).

### 🚀 Execução Técnica

#### 1. Varredura de Portas (Nmap)
Foi executada uma varredura completa para identificar serviços críticos.
* **Portas Identificadas:** 53 (DNS), 88 (Kerberos), 389/636 (LDAP/S), 445 (SMB), 3389 (RDP), 5985 (WinRM).
* **Descoberta:** O servidor foi identificado corretamente como um Controlador de Domínio do domínio `pixelforge.internal`.

#### 2. Testes de Enumeração SMB
Tentei extrair a lista de utilizadores e partilhas de rede sem credenciais (Null Session).
* **Resultado:** O sistema retornou `ACCESS_DENIED`.
* **Conclusão:** O Hardening de rede está funcional, impedindo que um atacante obtenha informações sensíveis de forma anónima.

#### 3. Brute-force de DNS
Utilizei o script `dns-brute` para tentar mapear outros hosts na rede.
* **Resultado:** Nenhuma entrada adicional descoberta.

### ⚠️ Análise de Risco (Findings)

| Risco | Gravidade | Descrição |
| :--- | :--- | :--- |
| **Exposição de RDP (3389)** | Média | A porta de Ambiente de Trabalho Remoto está aberta, o que permite tentativas de força bruta. |
| **Porta 5985 (WinRM)** | Baixa | Serviço ativo para gestão remota via PowerShell. Deve ser restrito a IPs de administração. |
| **Configuração de SMB** | Segura | Bloqueio de sessões nulas validado com sucesso. |

### 🏁 Conclusão da Fase 1
O servidor **PF-DC01** apresenta uma postura de segurança inicial sólida. A superfície de ataque está mapeada e os protocolos de enumeração anónima estão devidamente mitigados.

---
**🔜 Próximo Passo:** Instalação do Nessus Essentials e Auditoria de Vulnerabilidades Credenciada para análise de patches e configurações internas de registro.
