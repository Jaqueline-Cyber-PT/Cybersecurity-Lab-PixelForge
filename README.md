# Auditoria de Infraestrutura: Fase 1 (Reconhecimento)

## 1. Introdução e Âmbito
Este projeto teve como objetivo executar a fase inicial de reconhecimento e enumeração de um ativo crítico em ambiente controlado, aplicando de forma prática os conceitos de análise de superfície de ataque em infraestrutura Microsoft. O foco concentrou-se na identificação de portas abertas, serviços expostos, validação do papel do host na infraestrutura e avaliação preliminar do nível de *hardening* implementado.

## 2. Metodologia e Execução Técnica

A minha análise começou com o objetivo de mapear o servidor **PF-DC01**, o Controlador de Domínio da organização **PixelForge**. Para garantir que o ambiente estava operacional e verificar a latência da rede, o primeiro contacto foi um teste de conectividade simples via ICMP. Através do comando `ping`, confirmei que o host estava ativo e, pelo valor do TTL observado, obtive o primeiro indício de que se tratava de um sistema Windows. `ping 192.168.1.150`

<img width="948" height="248" alt="image" src="https://github.com/user-attachments/assets/01c9b2ad-f989-48e2-8160-2141b734bbcc" />

Com a conectividade confirmada, a etapa seguinte foi realizar uma varredura profunda para identificar quais portas e serviços estavam expostos. Utilizei o Nmap com uma abordagem TCP SYN, solicitando também a deteção de versões e do sistema operativo. Este passo foi crucial para confirmar que estava perante um Active Directory, expondo serviços críticos como Kerberos, LDAP e SMB. Comandos: `sudo nmap -sS -sV -O 192.168.1.150 -oA logs/scan_recon_dc01`

<img width="932" height="393" alt="image" src="https://github.com/user-attachments/assets/bca5a527-3222-4e59-8da8-10e67d17f977" />


Após mapear o perímetro, foquei-me em perceber se as configurações de segurança iniciais (Hardening) estavam a proteger informações sensíveis contra utilizadores não autenticados atravás do comando `enum4linux-ng -a 192.168.1.150*`. Para isso, recorri ao enum4linux-ng, tentando extrair dados via SMB e RPC. O resultado foi positivo para a segurança: o servidor bloqueou as tentativas de enumeração anónima com erros de acesso negado, provando que o primeiro nível de proteção estava ativo.

<img width="926" height="215" alt="image" src="https://github.com/user-attachments/assets/05ccc007-3d9c-4c18-8b8b-5980c00f737b" />
<img width="915" height="348" alt="image" src="https://github.com/user-attachments/assets/624212e1-e891-4c41-9420-a0b0c266f53a" />


Para encerrar esta fase de reconhecimento externo, realizei testes adicionais de força bruta em DNS e verificação de partilhas de rede. Embora as portas de gestão remota (RDP e WinRM) estivessem abertas (representando um vetor de risco a monitorizar) a postura global do servidor mostrou-se sólida contra ataques automáticos de descoberta de utilizadores. Comando: `nmap --script dns-brute --script-args dns-brute.domain=pixelforge.internal 192.168.1.150`

<img width="928" height="137" alt="image" src="https://github.com/user-attachments/assets/eda93de4-1e03-4e7d-a458-db8a24e3a713" />

## 3. Conclusão da Análise de Superfície

Esta fase de reconhecimento permitiu traçar um diagnóstico claro da postura defensiva perimetral do PF-DC01. Por um lado, identifiquei um hardening sólido no protocolo SMB, onde o bloqueio de sessões nulas impede eficazmente que um atacante obtenha uma lista de utilizadores ou partilhas sem credenciais. Por outro lado, a exposição das portas de gestão remota (RDP e WinRM) e os banners de serviços identificados revelam uma superfície de ataque previsível, típica de um ambiente Windows Server.

Concluo esta etapa com a validação de que o servidor está protegido contra ataques de enumeração oportunistas, mas a visibilidade externa é apenas metade da equação. Para determinar se o sistema está verdadeiramente resiliente, a Fase 2 será dedicada a uma inspeção interna detalhada. Utilizarei o Nessus Essentials para uma auditoria credenciada, procurando por vulnerabilidades críticas de software e falhas de configuração que não são visíveis a partir de uma varredura externa.
