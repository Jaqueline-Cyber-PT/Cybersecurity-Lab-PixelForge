# Relatório Técnico de Auditoria de Infraestrutura - Fase 1

### Introdução e Âmbito
Este documento descreve a fase inicial de reconhecimento e mapeamento de superfície de ataque realizada no servidor **PF-DC01**, configurado como Controlador de Domínio para a organização **PixelForge**. O objetivo desta etapa consistiu na identificação de serviços expostos e na validação das configurações de segurança perimetral do host num ambiente de rede interna controlado.

### Metodologia de Reconhecimento
A auditoria foi conduzida através de uma abordagem *Black Box*, utilizando ferramentas de varredura de rede e enumeração de protocolos. Foram empregues o **Nmap** para a descoberta de serviços e execução de scripts de diagnóstico (NSE), e o **Enum4linux** para a análise profunda dos protocolos SMB e RPC.

### Análise de Resultados e Vetores Identificados
A varredura revelou uma superfície de ataque típica de um ambiente Active Directory, com serviços críticos ativos em portas padrão, nomeadamente Kerberos (88), LDAP/S (389/636) e SMB (445). A identificação do domínio `pixelforge.internal` foi confirmada através da resposta dos serviços de diretório.

Durante a análise do protocolo SMB, foram realizados testes de sessão nula (*Null Sessions*) para verificar a possibilidade de extração de contas de utilizadores sem autenticação. O servidor respondeu corretamente com `NT_STATUS_ACCESS_DENIED`, o que demonstra a eficácia das políticas de *hardening* aplicadas para impedir a enumeração anónima da base de dados SAM.

Relativamente à gestão remota, as portas 3389 (RDP) e 5985 (WinRM) encontram-se abertas. Embora necessárias para a administração do sistema, a sua exposição direta na rede interna representa um vetor que exige monitorização de tentativas de autenticação e, preferencialmente, a implementação de restrições por endereços IP autorizados.

### Conclusão e Próximas Etapas
O servidor apresenta uma postura de segurança inicial robusta, com as vulnerabilidades óbvias de enumeração devidamente mitigadas. O reconhecimento externo está concluído, fornecendo os dados necessários para a **Fase 2**, que compreenderá uma gestão de vulnerabilidades credenciada. O foco passará agora para a análise interna de *patches* e configurações de registo através do **Nessus Essentials**.
