# Relatório de Incidente de Cibersegurança: Análise de Tráfego de Rede

### Contexto do Projeto
Este projeto é um case de estudo prático desenvolvido como parte do <a href="https://www.coursera.org/google-certificates/cybersecurity-certificate">Certificado Profissional de Cibersegurança do Google</a>. Ele documenta a análise de um incidente simulado onde usuários não conseguiam acessar o site `www.yummyrecipesforme.com`, encontrando o erro "destination port unreachable".
O incidente começou quando múltiplos clientes relataram que não conseguiam acessar o site `www.yummyrecipesforme.com`, recebendo uma mensagem de erro de "porta de destino inacessível". Como analista de segurança cibernética, minha tarefa foi investigar a causa raiz do problema através da análise de tráfego de rede.

### Ferramentas e Tecnologias
* **Análise de Pacotes:** `tcpdump` 
* **Protocolos Analisados:** `UDP`, `ICMP`, `DNS`
* **Metodologia:**
  1. Confirmação do erro de acesso ao site.
  2. Captura de pacotes de rede durante uma nova tenativa de conexão para análise detalhada.
  3. Inspeção dos protocolos `UDP`, `DNS` e `ICMP` para identificar a anomalia.

## Análise e Descobertas
A análise do tráfego capturado revelou um padrão claro que apontava para uma falha no processo de resolução de nomes de domínio.

1. **A requisição:** "Meu computador" (`IP 192.51.100.15`) enviou uma consulta `DNS` padrão, via `UDP`, para o servidor em `203.0.113.2`. A consulta pedia o endereço IP correspondente a `yummyrecipesforme.com`.
2. **A evidência:** Em vez de uma resposta `DNS`, o servidor retornou um pacote de erro `ICMP`, como pode ser visto no log abaixo.
   ![Log de tráfego do tcpdump mostrando o erro ICMP](https://github.com/cleyandson/case-study-dns-incident/blob/bdb5e26f9fde90e32a65b9268c485640f13da4e9/Documents/log-trafego-tcpdump.png)
3. **A Anomalia:** A mensagem de erro dentro do pacote `ICMP` era explícita: **`udp port 53 unreachable`** (porta UDP 53 inacessível).

## Conclusão

A causa raiz do incidente não foi uma falha no servidor web so site, mas sim no **serviço DNS** do servidor `203.0.113.2`.

A mensagem "unreachable" indica que, embora o servidor estivesse online para responder com um erro `ICMP`, não havia nenhum serviço ou processo "escutando" na porta 53 para de fato resolver a consulta DNS. Isso impedia que qualquer usuário conseguisse traduzir o nome do site para um endereço IP, tornando o acesso impossível.

O problema foi devidamente documentado e reportado aos engenheiros de segurança para a investigação e correção no servidor afetado.

### Relatório Completo em PDF
* [**Relatório de incidente de segurança cibernética**](https://github.com/cleyandson/case-study-dns-incident/blob/bdb5e26f9fde90e32a65b9268c485640f13da4e9/Documents/PT-BR%20Relat%C3%B3rio%20de%20incidente%20de%20seguran%C3%A7a%20cibern%C3%A9tica%20an%C3%A1lise%20de%20tr%C3%A1fego%20de%20rede.pdf)

---
*Este projeto demonstra habilidades práticas em análise de rede e documentação de incidentes, conforme o currículo do Certificado Profissional de Cibersegurança do Google.*
