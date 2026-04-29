---
title: "Como corrigir o erro 0x00000040 de impressora compartilhada no Windows"
date: 2026-04-29
description: "Clientes Windows 7 recebem o erro 0x00000040 ao conectar a uma impressora compartilhada pelo Windows 11? Este guia explica como habilitar o SMB 1.0 e editar o registro para resolver o problema."
tags: ["windows", "erro 0x00000040", "impressora compartilhada", "windows 7", "windows 11", "registro", "SMB", "solução de problemas"]
categories: ["Solução de Problemas"]
cover:
  image: "/images/printer-error-0x00000040/step1.png"
  alt: "Caixa de diálogo de erro do Windows exibindo o erro 0x00000040 — O nome de rede especificado não está mais disponível"
  caption: "O erro 0x00000040 aparece quando um cliente Windows 7 não consegue se conectar a uma impressora compartilhada pelo Windows 11"
  relative: false
---

## Descrição do problema

Quando um PC com Windows 11 é usado como host de impressora compartilhada, clientes Windows 7 que tentam se conectar recebem a seguinte mensagem de erro:

> **A operação não pôde ser concluída (Erro 0x00000040). O nome de rede especificado não está mais disponível.**

![Erro 0x00000040 no Windows](/images/printer-error-0x00000040/step1.png)

Este erro é geralmente causado por um serviço SMB desabilitado, um nome de compartilhamento expirado, uma conexão de rede interrompida ou uma incompatibilidade de protocolo RPC entre os dois sistemas operacionais. A correção envolve habilitar o SMB 1.0 e adicionar dois valores no registro do host Windows 11.

---

## Solução

**1.** Abra **Painel de Controle → Programas → Ativar ou desativar recursos do Windows** e habilite o **Suporte ao Compartilhamento de Arquivos SMB 1.0/CIFS**.

![Habilitar SMB 1.0/CIFS nos Recursos do Windows](/images/printer-error-0x00000040/step2.png)

**2.** Pressione **Win + R**, digite `regedit` e pressione Enter para abrir o **Editor do Registro**.

![Caixa de diálogo Executar com regedit](/images/printer-error-0x00000040/step3.png)

**3.** Navegue até a seguinte chave (crie-a se não existir):

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Clique com o botão direito no painel direito e crie um novo **Valor DWORD (32 bits)**.

![Editor do Registro — Criando novo valor DWORD sob a chave RPC](/images/printer-error-0x00000040/step4.png)

**4.** Renomeie o novo valor para `RpcProtocols` e defina seus **dados do valor** como `7`.

![Editor do Registro — RpcProtocols definido como 7](/images/printer-error-0x00000040/step5.png)

**5.** Clique com o botão direito novamente no painel direito e crie outro **Valor DWORD (32 bits)**. Renomeie-o para `ForceKerberosForRpc` e defina seus **dados do valor** como `1`.

![Editor do Registro — ForceKerberosForRpc definido como 1](/images/printer-error-0x00000040/step6.png)

**6.** Após concluir todas as etapas, **reinicie o computador** e tente conectar à impressora compartilhada novamente.

---

## Resumo

O erro 0x00000040 ao conectar a uma impressora compartilhada é um problema comum em ambientes mistos Windows 7/Windows 11. Geralmente é causado por um serviço SMB desabilitado ou incompatibilidade de protocolo RPC. A correção inclui dois passos:

- Habilitar o **Suporte ao Compartilhamento de Arquivos SMB 1.0/CIFS** nos Recursos do Windows
- Adicionar os valores de registro **RpcProtocols** (`7`) e **ForceKerberosForRpc** (`1`) em `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

Após reiniciar, a conexão com a impressora compartilhada deverá funcionar normalmente.
