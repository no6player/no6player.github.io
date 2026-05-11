---
title: "Como corrigir o erro 0x00000bcb de impressora compartilhada no Windows 10 e 11"
date: 2026-05-11
description: "Aparece o erro 0x00000bcb ao conectar uma impressora compartilhada no Windows 10 ou 11? Este guia aborda todas as causas e soluções — ajuste do registro, limpeza de drivers e reinicialização do spooler."
tags: ["windows", "erro 0x00000bcb", "impressora compartilhada", "registro", "point and print", "driver impressora", "windows 10", "windows 11", "solução de problemas"]
categories: ["Solução de Problemas"]
cover:
  image: "/images/printer-error-0x00000bcb/step1.png"
  alt: "Caixa de diálogo de erro do Windows exibindo o erro 0x00000bcb — O driver de impressora especificado não foi encontrado neste sistema"
  caption: "O erro 0x00000bcb aparece quando o Windows não consegue instalar o driver da impressora compartilhada devido a restrições de política RPC"
  relative: false
---

# Como corrigir o erro 0x00000bcb de impressora compartilhada no Windows 10 e 11

O erro 0x00000bcb é uma falha de conexão comum em impressoras compartilhadas no Windows 10 e Windows 11. O Windows exibe uma mensagem informando que o driver de impressora especificado não foi encontrado e precisa ser baixado — mas o download falha. Este artigo explica exatamente por que o erro 0x00000bcb ocorre e como corrigi-lo passo a passo.

---

## O que é o erro 0x00000bcb?

Ao tentar conectar a uma impressora compartilhada na rede local, o Windows exibe:

> **A operação não pôde ser concluída (Erro 0x00000bcb). O driver de impressora especificado não foi encontrado neste sistema e precisa ser baixado.**

![Erro 0x00000bcb no Windows](/images/printer-error-0x00000bcb/step1.png)

O Windows tenta instalar automaticamente o driver de impressora a partir do PC host ao conectar a uma impressora compartilhada. O erro 0x00000bcb significa que essa instalação automática foi bloqueada — na maioria das vezes por uma política de segurança introduzida em atualizações recentes do Windows.

---

## Por que o erro 0x00000bcb ocorre?

Existem cinco causas comuns:

1. **A atualização do Windows reforçou a segurança RPC** — Os patches de segurança da Microsoft restringiram a instalação de drivers Point and Print apenas a administradores, bloqueando o download automático de impressoras compartilhadas.
2. **Driver de impressora ausente ou incompatível** — O PC cliente não possui o driver correto, ou a versão instalada não corresponde à do host.
3. **Erro no serviço Spooler de impressão ou cache de impressão corrompido** — Um trabalho de impressão travado pode impedir a instalação de novos drivers.
4. **Permissões LAN insuficientes** — O PC cliente não tem os direitos necessários para obter e instalar drivers do host.
5. **Firewall ou software de segurança bloqueando a comunicação compartilhada** — Regras de antivírus de terceiros ou do Firewall do Windows podem bloquear o tráfego de compartilhamento de impressoras.

---

## Solução

### Etapa 1 — Modificar o registro (restrição Point and Print)

**1.** Pressione **Win + R**, digite `regedit` e pressione Enter para abrir o **Editor do Registro**.

![Caixa de diálogo Executar com regedit](/images/printer-error-0x00000bcb/step2.png)

**2.** Navegue até o seguinte caminho. Se alguma parte do caminho não existir, crie as subchaves ausentes manualmente: clique com o botão direito na chave pai → **Novo → Chave**.

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint
```

![Editor do Registro — navegando até a chave PointAndPrint](/images/printer-error-0x00000bcb/step3.png)

**3.** No painel direito, clique com o botão direito e selecione **Novo → Valor DWORD (32 bits)**. Nomeie-o `RestrictDriverInstallationToAdministrators`.

![Editor do Registro — criar DWORD RestrictDriverInstallationToAdministrators](/images/printer-error-0x00000bcb/step4.png)

**4.** Clique duas vezes no novo valor e defina seus **dados do valor** como `0`. Clique em **OK**.

![Editor do Registro — RestrictDriverInstallationToAdministrators definido como 0](/images/printer-error-0x00000bcb/step5.png)

### Etapa 2 — Remover o driver de impressora antigo

**5.** Abra **Painel de Controle → Hardware e Sons → Dispositivos e Impressoras**. Clique em **Propriedades do servidor de impressão** na barra de menu superior, depois vá para a guia **Drivers**. Selecione o driver antigo da impressora compartilhada e clique em **Remover**.

![Propriedades do servidor de impressão — guia Drivers](/images/printer-error-0x00000bcb/step6.png)

### Etapa 3 — Reiniciar e reconectar

**6.** Reinicie o computador. Após reiniciar, tente adicionar a impressora compartilhada novamente. O Windows agora deve conseguir baixar e instalar o driver sem exibir o erro 0x00000bcb.

---

## Perguntas frequentes

**P: O erro 0x00000bcb afeta apenas o Windows 10 ou também o Windows 11?**
Ambos. O erro 0x00000bcb aparece no Windows 10 e no Windows 11, pois a causa subjacente — uma política de segurança Point and Print reforçada — foi aplicada a ambos os sistemas via Windows Update.

**P: É seguro definir RestrictDriverInstallationToAdministrators como 0?**
Sim, em um ambiente LAN doméstico ou de escritório. Este valor do registro apenas relaxa a permissão de instalação de drivers para impressoras compartilhadas na sua rede local. Não desabilita nenhuma outra função de segurança.

**P: Segui todos os passos, mas o erro 0x00000bcb persiste. O que mais posso tentar?**
Instale manualmente o driver de impressora no PC cliente (baixe-o do site do fabricante) e depois conecte a impressora compartilhada. Isso contorna completamente o download automático do driver.

**P: O erro 0x00000bcb é o mesmo que o erro 0x00000709?**
Não são o mesmo código, mas compartilham causas semelhantes — ambos envolvem restrições de política RPC e problemas de permissão de drivers. Se os passos acima não resolverem o 0x00000bcb, consulte o [guia de correção do 0x00000709](/pt/posts/printer-sharing-error-0x00000709/).

---

## Resumo

O erro 0x00000bcb ao conectar uma impressora compartilhada no Windows 10 ou Windows 11 é causado principalmente por uma política de segurança do Windows Update que restringe a instalação automática de drivers de impressora. A correção em três etapas:

1. Definir `RestrictDriverInstallationToAdministrators` = `0` no registro em `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint`
2. Remover o driver antigo em **Propriedades do servidor de impressão → Drivers**
3. Reiniciar o computador e reconectar a impressora compartilhada

---

## Artigos relacionados

- [Corrigir o erro de impressora compartilhada 0x00000709 no Windows](/pt/posts/printer-sharing-error-0x00000709/)
- [Corrigir o erro de impressora compartilhada 0x0000011b no Windows](/pt/posts/printer-sharing-error-0x0000011b/)
- [Corrigir o erro de impressora compartilhada 0x000003e3 no Windows 10](/pt/posts/printer-sharing-error-0x000003e3/)
