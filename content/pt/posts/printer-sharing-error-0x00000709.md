---
title: "Como corrigir o erro 0x00000709 de impressora compartilhada no Windows"
date: 2026-04-29
description: "Aparece o erro 0x00000709 ao conectar uma impressora compartilhada no Windows 10 ou Windows 11? Este guia passo a passo resolve o problema adicionando credenciais do Windows e modificando um valor do registro."
tags: ["windows", "erro 0x00000709", "impressora compartilhada", "gerenciador de credenciais", "registro", "windows 10", "windows 11", "solução de problemas"]
categories: ["Solução de Problemas"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Caixa de diálogo de erro do Windows exibindo o erro 0x00000709 — A operação não pôde ser concluída"
  caption: "O erro 0x00000709 aparece quando o Windows não consegue se conectar a uma impressora compartilhada na rede"
  relative: false
---

# Como corrigir o erro 0x00000709 de impressora compartilhada no Windows

O erro 0x00000709 é um dos erros de impressora compartilhada mais comuns no Windows 10 e Windows 11. Ele impede a conexão a uma impressora compartilhada na rede e pode surgir de repente — muitas vezes após uma atualização do Windows. A boa notícia é que o problema pode ser resolvido em poucos minutos, sem software de terceiros.

---

## O que é o erro 0x00000709?

Ao tentar conectar a uma impressora compartilhada na rede local, o Windows pode exibir:

> **A operação não pôde ser concluída (Erro 0x00000709). Verifique o nome da impressora e certifique-se de que ela está conectada à rede.**

![Erro 0x00000709 no Windows](/images/printer-error-0x00000709/step1.png)

O erro 0x00000709 é um erro de permissão ou autenticação. O Windows tenta se comunicar com o host da impressora pela rede, mas é bloqueado pela ausência de credenciais ou por uma restrição do protocolo RPC.

---

## Por que o erro 0x00000709 ocorre?

Existem três causas principais:

- **Credenciais de rede ausentes** — O Windows não tem um nome de usuário/senha salvo para o computador host, então o acesso é negado.
- **Restrição de protocolo RPC** — Após certas atualizações de segurança do Windows, o método de comunicação RPC padrão entre computadores mudou, quebrando conexões de impressoras compartilhadas.
- **Conta Convidado não reconhecida** — A conta Convidado do host não é considerada confiável pelo computador cliente, causando falha silenciosa na autenticação.

Este erro aparece frequentemente em clientes **Windows 10** e **Windows 11** que se conectam a uma impressora compartilhada de outro PC.

---

## Solução

A correção tem duas partes: primeiro adicionar as credenciais do host ao Gerenciador de Credenciais do Windows e depois ajustar um valor de registro RPC no computador cliente.

### Parte 1 — Adicionar credenciais do Windows

**1.** Abra o **Painel de Controle**, configure a visualização como **Ícones pequenos** e clique em **Gerenciador de Credenciais**.

![Painel de Controle — Gerenciador de Credenciais](/images/printer-error-0x00000709/step2.png)

**2.** Selecione **Credenciais do Windows** e clique em **Adicionar uma credencial do Windows**.

![Gerenciador de Credenciais — Adicionar uma credencial do Windows](/images/printer-error-0x00000709/step3.png)

**3.** No campo **Endereço de Internet ou de rede**, insira o endereço IP do PC host (ex. `192.168.1.100`). Defina o **Nome de usuário** como `guest` e deixe o campo **Senha** em branco. Clique em **OK**.

![Adicionar credencial do Windows — inserir IP do host e nome de usuário convidado](/images/printer-error-0x00000709/step4.png)

> **Dica:** Você pode encontrar o endereço IP do PC host executando `ipconfig` em um Prompt de Comando naquele computador.

### Parte 2 — Modificar o registro

**4.** Pressione **Win + R**, digite `regedit` e pressione Enter para abrir o **Editor do Registro**.

![Caixa de diálogo Executar com regedit](/images/printer-error-0x00000709/step5.png)

**5.** Navegue até a seguinte chave. Se a subchave `RPC` não existir, crie-a manualmente em `Printers`:

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Clique com o botão direito no painel direito e selecione **Novo → Valor DWORD (32 bits)**.

![Editor do Registro — Criando novo valor DWORD sob a chave RPC](/images/printer-error-0x00000709/step6.png)

**6.** Renomeie o novo valor para `RpcUseNamedPipeProtocol` e defina seus **dados do valor** como `1`. Clique em **OK**.

![Editor do Registro — RpcUseNamedPipeProtocol definido como 1](/images/printer-error-0x00000709/step7.png)

**7.** Após concluir todas as etapas, **reinicie o computador** e tente conectar à impressora compartilhada novamente. O erro 0x00000709 não deve mais aparecer.

---

## Perguntas frequentes

**P: Esta solução funciona no Windows 10 e no Windows 11?**
Sim. O erro 0x00000709 ocorre no Windows 10 e no Windows 11. Os passos descritos se aplicam a ambas as versões.

**P: Preciso fazer alterações no PC host da impressora?**
Geralmente não. A correção é aplicada no PC **cliente**. Certifique-se, porém, de que a impressora está corretamente compartilhada no host e que a conta Convidado está ativada lá.

**P: O erro persiste após seguir todos os passos. O que mais posso tentar?**
- Verifique se o endereço IP do PC host no Gerenciador de Credenciais está correto.
- Certifique-se de que o firewall do PC host não está bloqueando o compartilhamento de arquivos e impressoras.
- Tente conectar usando o nome do PC host em vez do endereço IP.
- Desinstale e reinstale o driver da impressora no computador cliente.

---

## Resumo

O erro 0x00000709 ao conectar uma impressora compartilhada no Windows 10 ou Windows 11 é causado geralmente por credenciais de rede ausentes ou uma restrição do protocolo RPC. A correção em dois passos:

1. Adicionar o IP do host e a conta **guest** ao **Gerenciador de Credenciais do Windows**
2. Criar o valor de registro **RpcUseNamedPipeProtocol** = `1` em `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

---

## Artigos relacionados

- [Corrigir o erro de impressora compartilhada 0x0000011b no Windows](/pt/posts/printer-sharing-error-0x0000011b/)
- [Corrigir o erro de impressora compartilhada 0x000003e3 no Windows 10](/pt/posts/printer-sharing-error-0x000003e3/)
- [Corrigir o erro de impressora compartilhada 0x00000040 no Windows](/pt/posts/printer-sharing-error-0x00000040/)
