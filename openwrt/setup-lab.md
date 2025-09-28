**`[2025-09-28 16:32:00]`**

### Roteiro Simplificado: Configuração do Gateway do Laboratório (Re-Build)

Este roteiro cobre todas as configurações necessárias para ter seu gateway do laboratório operacional e seguro.

**Pré-requisito:**
* Seu Mac está conectado via cabo na porta LAN (única porta) do NanoStation.
* Seu Mac está com IP estático na faixa `192.168.1.x` (ex: `192.168.1.10`), para que você possa acessar `http://192.168.1.1`.
* O NanoStation está conectado ao injetor PoE e este na tomada.

---

#### 1. Acesso Inicial e Definição da Senha de Root

1.  Acesse a interface web do OpenWrt no seu navegador: `http://192.168.1.1`.
2.  Faça login com `Username: root` e **senha em branco**.
3.  Você verá um aviso amarelo: **"No password set!"**. Clique em "Go to password configuration..." ou vá para `System -> Administration`.
4.  Defina uma **senha forte** para o usuário `root`.
5.  Clique em **"Save & Apply"**. (Você será desconectado e precisará logar novamente com a nova senha).

#### 2. Configuração da Interface WAN (Conexão à Internet)

Aqui definiremos a única porta física do NanoStation (`eth0`) como a WAN.

1.  Vá para **`Network -> Interfaces`**.
2.  Clique em **"Add new interface..."**.
3.  **Name:** `WAN` (maiúsculo).
4.  **Protocol:** `DHCP client`.
5.  **Device:** Selecione **`eth0`**.
6.  Clique em **"Create interface"**.
7.  Na tela de configuração da WAN:
    * Vá para a aba **"Firewall Settings"**.
    * Em "Create / Assign firewall-zone", selecione a zona **`wan`**.
    * Clique em **"Save"**. **NÃO CLIQUE em "Save & Apply" ainda.**

#### 3. Configuração da Interface LAN (Rede do Laboratório)

Agora, vamos configurar a interface Wi-Fi (`wlan0`) para ser a LAN, e ativar o servidor DHCP para seus clientes do laboratório.

1.  Na mesma página **`Network -> Interfaces`**, edite a interface **`LAN`**.
2.  Vá para a aba **"Device"**.
3.  No campo "Bridge ports", você verá `eth0` e `wlan0`. **Desmarque `eth0`**, deixando apenas a interface wireless (`wlan0`) selecionada. Se `wlan0` não estiver visível (porque o Wi-Fi está desativado), simplesmente **desmarque `eth0`** e continue. Iremos ativar o Wi-Fi no próximo passo.
4.  Vá para a aba **"General Settings"**.
    * **IPv4 address:** `10.10.10.1`
    * **IPv4 netmask:** `255.255.255.0`
5.  Vá para a aba **"DHCP Server"**.
    * **DESMARQUE** a opção **"Ignore interface"**. Isso ativará o servidor DHCP.
    * (Opcional, mas recomendado) Vá para a aba **"IPv6 Settings"** e defina `Router Advertisement-Service`, `DHCPv6-Service` e `NDP-Proxy` para **`disabled`**.
6.  Clique em **"Save"**. **NÃO CLIQUE em "Save & Apply" ainda.**

#### 4. Configuração do Firewall para Isolamento

Este é o passo crucial para garantir a segurança do seu laboratório.

1.  Vá para **`Network -> Firewall`**.
2.  Na seção **"Zone Settings"**, procure a linha que mostra o encaminhamento (forwarding) da `lan` para a `wan`.
3.  Clique em **"Edit"** nesta regra.
4.  Mude a opção **"Forward"** de `ACCEPT` para **`REJECT`**.
5.  Clique em **"Save"**.

#### 5. Ativar e Configurar o Wi-Fi (Sua `LAB-NET`)

1.  Vá para **`Network -> Wireless`**.
2.  Encontre a interface **`radio0`** (sua interface Wi-Fi) e clique em **"Enable"** (se estiver desativada).
3.  Clique em **"Edit"** na interface Wi-Fi que aparecer (geralmente `radio0` ou a interface padrão da LAN).
4.  Vá para a aba **"General Setup"**.
    * **Mode:** `Access Point`.
    * **Network:** Certifique-se de que está atribuído à interface **`LAN`**.
    * **ESSID:** Mude para o nome da sua rede (ex: `LAB-NET-HACKING` ou `TARS-LAB`).
5.  Vá para a aba **"Wireless Security"**.
    * **Encryption:** Selecione `WPA2-PSK`.
    * **Key:** Defina uma senha forte para sua rede Wi-Fi do laboratório.
6.  Clique em **"Save"**.

#### 6. Aplicação Final e Reconexão do seu Mac

1.  Agora que todas as configurações estão salvas, você verá um botão **"Save & Apply"** no topo da interface. **Clique nele.**
2.  **O NanoStation irá aplicar todas as configurações e reiniciar seus serviços de rede.**
3.  **Você perderá o acesso via cabo.** A porta `eth0` agora é a WAN e não aceita gerenciamento direto por padrão.
4.  **No seu Mac:** Volte para `Ajustes do Sistema -> Rede -> Ethernet -> TCP/IP`. Mude a configuração de volta para **"Usar DHCP"**. Isso é importante para que sua rede principal funcione normalmente depois.
5.  **Conecte-se à Nova Rede Wi-Fi:** No seu Mac, conecte-se à rede Wi-Fi que você acabou de criar (`LAB-NET-HACKING` ou `TARS-LAB`).
6.  **Ajuste o IP do seu Mac (Temporário):** Se o seu Mac não adquirir um IP automaticamente (o que pode acontecer inicialmente), configure-o **manualmente** para:
    * **Endereço IP:** `10.10.10.10`
    * **Máscara de Sub-rede:** `255.255.255.0`
    * **Roteador:** `10.10.10.1`
7.  Acesse a interface OpenWrt (agora TARS!) via Wi-Fi: **`http://10.10.10.1`**.

Agora, os clientes Wi-Fi (incluindo seu Mac e as VMs) devem receber IPs do OpenWrt, e o roteador estará corretamente configurado para isolar o laboratório. O NanoStation aparecerá na lista de clientes do seu roteador principal, mas os clientes do Wi-Fi dele estarão isolados.
