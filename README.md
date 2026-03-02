# Relatório Técnico de Auditoria: Reconhecimento de Infraestrutura

Este projeto documenta a análise de superfície de ataque realizada no servidor **PF-DC01**, o Controlador de Domínio do ambiente **PixelForge**.

### Metodologia
A auditoria utilizou uma abordagem *Black Box* para identificar serviços expostos e validar configurações de segurança perimetral. Foram utilizadas as ferramentas **Nmap** (mapeamento de portas e diagnóstico NSE) e **Enum4linux** (análise de protocolos SMB/RPC).

### Análise de Resultados
A varredura identificou os serviços críticos Kerberos (88), LDAP/S (389/636) e SMB (445), confirmando a função do host como Active Directory Domain Controller (`pixelforge.internal`). 

Os testes de enumeração via *Null Sessions* resultaram em `ACCESS_DENIED`, validando que as políticas de *hardening* contra a extração anónima de dados da base SAM estão devidamente aplicadas. No entanto, a exposição das portas 3389 (RDP) e 5985 (WinRM) foi registada como um vetor que exige monitorização de tentativas de autenticação e restrição de acesso por IP.

### Conclusão e Próximos Passos
A postura de segurança inicial do servidor é robusta, com mitigações eficazes contra a enumeração básica de rede. O reconhecimento externo está concluído, fornecendo a base necessária para a **Fase 2**, que consistirá numa auditoria de vulnerabilidades credenciada (análise de patches e registos internos) com o **Nessus Essentials**.
