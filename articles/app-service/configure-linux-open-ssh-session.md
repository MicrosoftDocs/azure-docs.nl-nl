---
title: SSH-toegang voor Linux-containers
description: U kunt een SSH-sessie openen voor een Linux-container in Azure App Service. Aangepaste Linux-containers worden ondersteund met enkele wijzigingen in uw aangepaste afbeelding.
keywords: azure app service, web-app, linux, oss
author: msangapu-msft
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.topic: article
ms.date: 02/23/2021
ms.author: msangapu
ms.custom: seodec18, devx-track-azurecli
ms.openlocfilehash: 3610c4a571d73631ed39d416c72d0d6004dd170d
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2021
ms.locfileid: "107871755"
---
# <a name="open-an-ssh-session-to-a-linux-container-in-azure-app-service"></a>Open een SSH-sessie naar een Linux-container in Azure App Service

[Secure Shell (SSH)](https://wikipedia.org/wiki/Secure_Shell) wordt vaak gebruikt om beheeropdrachten op afstand uit te voeren vanuit een opdrachtregelterminal. App Service op Linux biedt SSH-ondersteuning in de app-container. 

![Linux App Service SSH](./media/configure-linux-open-ssh-session/app-service-linux-ssh.png)

U kunt ook rechtstreeks vanaf uw lokale ontwikkelmachine verbinding maken met de container met behulp van SSH en SFTP.

## <a name="open-ssh-session-in-browser"></a>SSH-sessie in de browser openen

[!INCLUDE [Open SSH session in browser](../../includes/app-service-web-ssh-connect-no-h.md)]

## <a name="use-ssh-support-with-custom-docker-images"></a>SSH-ondersteuning gebruiken met aangepaste Docker-afbeeldingen

Zie [SSH configureren in een aangepaste container.](configure-custom-container.md#enable-ssh)

## <a name="open-ssh-session-from-remote-shell"></a>SSH-sessie openen vanuit externe shell

> [!NOTE]
> Deze functie is momenteel beschikbaar als preview-versie.
>

Met TCP-tunneling kunt u een netwerkverbinding maken tussen uw ontwikkelmachine en Web App for Containers via een geverifieerde WebSocket-verbinding. Hiermee kunt u een SSH-sessie openen met uw container die wordt uitgevoerd in App Service van de client van uw keuze.

Om aan de slag te gaan, moet u [Azure CLI installeren.](/cli/azure/install-azure-cli) Als u wilt zien hoe het werkt zonder Azure CLI te installeren, opent [u Azure Cloud Shell](../cloud-shell/overview.md). 

Open een externe verbinding met uw app met behulp van [de opdracht az webapp remote-connection create.](/cli/azure/ext/webapp/webapp/remote-connection#ext-webapp-az-webapp-remote-connection-create) Geef _\<subscription-id>_ , en _ op voor uw _\<group-name>_ \_ \<app-name> app.

```azurecli-interactive
az webapp create-remote-connection --subscription <subscription-id> --resource-group <resource-group-name> -n <app-name> &
```

> [!TIP]
> `&` aan het einde van de opdracht is alleen voor het gemak als u Cloud Shell. Het proces wordt op de achtergrond uitgevoerd, zodat u de volgende opdracht in dezelfde shell kunt uitvoeren.

> [!NOTE]
> Als deze opdracht mislukt, moet u ervoor zorgen dat [externe foutopsporing](https://medium.com/@auchenberg/introducing-remote-debugging-of-node-js-apps-on-azure-app-service-from-vs-code-in-public-preview-9b8d83a6e1f0) is *uitgeschakeld* met de volgende opdracht:
>
> ```azurecli-interactive
> az webapp config set --resource-group <resource-group-name> -n <app-name> --remote-debugging-enabled=false
> ```

De uitvoer van de opdracht geeft u de informatie die u nodig hebt om een SSH-sessie te openen.

```output
Port 21382 is open
SSH is available { username: root, password: Docker! }
Start your favorite client and connect to port 21382
```

Open een SSH-sessie met uw container met de client van uw keuze, met behulp van de lokale poort. In het volgende voorbeeld wordt de [standaard-ssh-opdracht](https://ss64.com/bash/ssh.html) gebruikt:

```bash
ssh root@127.0.0.1 -p <port>
```

Wanneer u hier om wordt gevraagd, typt `yes` u om door te gaan met het maken van verbinding. Vervolgens wordt u om het wachtwoord gevraagd. Gebruik `Docker!` , dat eerder aan u is weergegeven.

<pre>
Warning: Permanently added '[127.0.0.1]:21382' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
</pre>

Nadat u bent geverifieerd, ziet u het welkomstscherm van de sessie.

<pre>
  _____
  /  _  \ __________ _________   ____
 /  /_\  \___   /  |  \_  __ \_/ __ \
/    |    \/    /|  |  /|  | \/\  ___/
\____|__  /_____ \____/ |__|    \___  &gt;
        \/      \/                  \/
A P P   S E R V I C E   O N   L I N U X

0e690efa93e2:~#
</pre>

U bent nu verbonden met uw connector.  

Voer de bovenste [opdracht](https://ss64.com/bash/top.html) uit. U zou het proces van uw app in de proceslijst moeten kunnen zien. In de onderstaande voorbeelduitvoer is dit de uitvoer met `PID 263` .

<pre>
Mem: 1578756K used, 127032K free, 8744K shrd, 201592K buff, 341348K cached
CPU:   3% usr   3% sys   0% nic  92% idle   0% io   0% irq   0% sirq
Load average: 0.07 0.04 0.08 4/765 45738
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     S     1528   0%   0   0% /sbin/init
  235     1 root     S     632m  38%   0   0% PM2 v2.10.3: God Daemon (/root/.pm2)
  263   235 root     S     630m  38%   0   0% node /home/site/wwwroot/app.js
  482   291 root     S     7368   0%   0   0% sshd: root@pts/0
45513   291 root     S     7356   0%   0   0% sshd: root@pts/1
  291     1 root     S     7324   0%   0   0% /usr/sbin/sshd
  490   482 root     S     1540   0%   0   0% -ash
45539 45513 root     S     1540   0%   0   0% -ash
45678 45539 root     R     1536   0%   0   0% top
45733     1 root     Z        0   0%   0   0% [init]
45734     1 root     Z        0   0%   0   0% [init]
45735     1 root     Z        0   0%   0   0% [init]
45736     1 root     Z        0   0%   0   0% [init]
45737     1 root     Z        0   0%   0   0% [init]
45738     1 root     Z        0   0%   0   0% [init]
</pre>

## <a name="next-steps"></a>Volgende stappen

U kunt vragen en zorgen posten op het [Azure-forum.](/answers/topics/azure-webapps.html)

Zie voor meer informatie over Web App for Containers:

* [Introductie van externe debugging van Node.js-apps op Azure App Service van VS Code](https://medium.com/@auchenberg/introducing-remote-debugging-of-node-js-apps-on-azure-app-service-from-vs-code-in-public-preview-9b8d83a6e1f0)
* [Quickstart: Een aangepaste container uitvoeren op App Service](quickstart-custom-container.md?pivots=container-linux)
* [Ruby gebruiken in Azure App Service onder Linux](quickstart-ruby.md)
* [Web App for Containers van Azure App Service: veelgestelde vragen](faq-app-service-linux.md)
