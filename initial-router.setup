# Jornada de Criação do Laboratório de Hacking: Do TP-Link ao Gateway Ubiquiti

* **Autor:** Savio
* **Data de Criação:** 2025-09-27
* **Objetivo:** Montar um laboratório de hacking seguro e isolado para fins de estudo e prática de pentesting, utilizando hardware reaproveitado.
* **Status:** Fase de Configuração Lógica do Gateway Concluída. Pronto para implementação da rede física.

---

## Resumo Executivo

Este documento detalha o processo de criação de um gateway de rede para um laboratório de hacking isolado. A tentativa inicial com um roteador SOHO (TP-Link TL-WR840N) foi abortada devido a severas limitações de hardware que o tornavam obsoleto e inseguro. Uma pivô estratégica foi realizada para um dispositivo de classe prosumer (Ubiquiti NanoStation Loco M2), que se provou uma plataforma robusta e ideal. O processo cobriu desde a recuperação de acesso e identificação de hardware até a instalação e configuração de base do OpenWrt, resultando em um gateway seguro e pronto para a próxima fase do projeto.

---

## Fase 1: A Tentativa Inicial com o TP-Link TL-WR840N v6

### 1.1. A Proposta Inicial

O plano original era reaproveitar um roteador doméstico sobressalente, um **TP-Link TL-WR840N**, para atuar como o coração do laboratório. Sua função seria criar uma rede fisicamente isolada (`LAB-NET`), servindo como firewall e gateway para as máquinas virtuais atacantes e alvos, impedindo qualquer comunicação com a rede principal.

### 1.2. O Primeiro Obstáculo: Acesso e Identificação

* **Problema:** A senha de acesso ao firmware original da TP-Link havia sido esquecida.
* **Solução:** Foi necessário realizar um **reset de fábrica** no dispositivo, pressionando o botão de reset por 10-15 segundos.
* **Resultado:** O acesso foi restaurado com as credenciais padrão (`admin`/`admin`), permitindo a identificação precisa do hardware via etiqueta: **Versão 6.0**.

### 1.3. O Obstáculo Crítico: Hardware "4/32" e Obsolescência

Com a versão do hardware (`v6`) confirmada, a pesquisa na documentação oficial do OpenWrt revelou um alerta crítico para dispositivos com **4MB de Flash e 32MB de RAM**, conhecidos como "4/32".

**As implicações deste hardware eram severas:**

1.  **Fim de Suporte:** O suporte oficial do OpenWrt para esta classe de dispositivo encerrou em 2022. A última versão compatível era a **19.07.10**, um sistema operacional datado e sem atualizações de segurança.
2.  **Inviabilidade de Segurança:** Utilizar um sistema operacional sem patches de segurança recentes para gerenciar um laboratório de segurança era uma falha conceitual grave.
3.  **Limitação de Espaço:** Os 4MB de Flash seriam quase totalmente consumidos pela instalação base, impossibilitando a adição de qualquer pacote ou ferramenta extra.
4.  **Instabilidade:** A baixa memória RAM (32MB) tornaria a interface web lenta e o dispositivo propenso a travamentos sob carga de rede, mesmo que mínima.

> **Conclusão da Fase 1:** O TP-Link TL-WR840N v6 foi considerado um **beco sem saída técnico**. Prosseguir com ele significaria construir o laboratório sobre uma fundação instável, insegura e obsoleta. A decisão foi abandonar este hardware e procurar uma alternativa mais robusta.

---

## Fase 2: A Pivô Estratégica para o Ubiquiti NanoStation Loco M2

### 2.1. A Nova Proposta de Hardware

Uma alternativa foi encontrada: um **Ubiquiti NanoStation Loco M2**, um dispositivo de classe Prosumer/WISP, conhecido por sua robustez e melhor qualidade de hardware.

### 2.2. Análise Comparativa e Vantagens

| Característica | TP-Link TL-WR840N v6 | Ubiquiti NanoStation Loco M2 |
| :--- | :--- | :--- |
| **Memória Flash** | **4 MB** (Obsoleto) | **8 MB** (Suporte a versões modernas) |
| **Suporte OpenWrt** | Apenas versões antigas | **Versões recentes e suportadas (23.05+)**|
| **Alimentação** | Fonte Comum | **PoE Passivo 24V** |

A vantagem principal era clara: os **8MB de Flash** permitiam a instalação de versões recentes e seguras do OpenWrt, resolvendo o principal problema do TP-Link.

### 2.3. O Processo de Recuperação e Identificação

Assim como o TP-Link, o dispositivo precisava ser diagnosticado antes do flash.

1.  **Desafio de Acesso:** A senha do firmware original (AirOS) era desconhecida.
2.  **Identificação de Hardware:** A documentação do OpenWrt exigia saber a família do hardware (`XM` ou `XW`), informação não disponível na carcaça do aparelho.
3.  **Plano de Ação:** Foi executado um plano metodológico:
    * **Reset de Fábrica:** O dispositivo foi resetado para as configurações de fábrica.
    * **Acesso via IP Estático:** Foi necessário configurar um IP estático na máquina local (`192.168.1.10`) para acessar o IP padrão do NanoStation (`192.168.1.20`), que não possui DHCP ativo de fábrica.
    * **Login e Identificação:** O acesso foi feito com as credenciais padrão (`ubnt`/`ubnt`). A tela principal do AirOS confirmou a informação vital: a versão do firmware era baseada na família de hardware **`XW`**.

---

## Fase 3: Instalação e Configuração do Gateway OpenWrt

### 3.1. Flash do Firmware

Com a certeza do hardware `XW`, o processo de instalação foi direto:
1.  Foi baixada a imagem `...factory.bin` correta para o **NanoStation Loco M2 (XW)** a partir do site oficial do OpenWrt.
2.  Através da interface web do AirOS, na aba **`System`**, o novo firmware foi carregado e instalado com sucesso.

### 3.2. Sucesso e a Surpresa Positiva

O dispositivo reiniciou com sucesso no OpenWrt, acessível no IP padrão `192.168.1.1`. A análise da página de status revelou uma surpresa extremamente positiva: o dispositivo possuía **64MB de RAM**, e não os 32MB esperados. Isso eliminou qualquer preocupação restante sobre performance e estabilidade.

### 3.3. Configuração de Base (Hardening e Rede)

Os seguintes passos foram executados para preparar o gateway:
1.  **Segurança:** Uma senha de `root` foi definida para proteger o acesso.
2.  **Configuração de Rede:** A interface `LAN` foi reconfigurada para usar o endereço IP final do laboratório: **`10.10.10.1`**.
3.  **Acesso Final:** O IP da máquina local foi ajustado para a nova sub-rede (`10.10.10.10`), e o acesso ao gateway foi restabelecido em seu novo endereço.

---

## 4. Estado Atual e Próximos Passos

### 4.1. Arquitetura do Gateway

O **Ubiquiti NanoStation Loco M2** está agora operando como um gateway OpenWrt dedicado, seguro e estável, no endereço `10.10.10.1`. Ele está logicamente pronto para gerenciar e isolar a rede do laboratório.

### 4.2. Próximos Passos

1.  **Implementar a Rede Física:** Conectar o gateway OpenWrt a um switch de rede.
2.  **Conectar as Máquinas:** Conectar as máquinas virtuais (atacante e alvos) ao mesmo switch, configurando-as para receber IP na faixa `10.10.10.0/24`.
3.  **Iniciar os Testes:** Começar os exercícios práticos de pentesting no ambiente seguro e isolado.
