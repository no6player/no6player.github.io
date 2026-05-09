---
title: "Como corrigir o erro 0x00000709 de impressora compartilhada no Windows"
date: 2026-04-29
description: "Aparece o erro 0x00000709 ao conectar uma impressora compartilhada no Windows? Este guia explica como resolver o problema adicionando credenciais do Windows e modificando um valor do registro."
tags: ["windows", "erro 0x00000709", "impressora compartilhada", "gerenciador de credenciais", "registro", "solução de problemas"]
categories: ["Solução de Problemas"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Caixa de diálogo de erro do Windows exibindo o erro 0x00000709 — A operação não pôde ser concluída"
  caption: "O erro 0x00000709 aparece quando o Windows não consegue se conectar a uma impressora compartilhada na rede"
  relative: false
---

## Descrição do problema

Ao tentar conectar a uma impressora compartilhada na rede, a conexão falha com a seguinte mensagem de erro:

> **A operação não pôde ser concluída (Erro 0x00000709). Verifique o nome da impressora e certifique-se de que ela está conectada à rede.**

![Erro 0x00000709 no Windows](/images/printer-error-0x00000709/step1.png)

O erro 0x00000709 é um erro comum de conexão a impressoras compartilhadas no Windows. Geralmente é causado por uma atualização do sistema, uma configuração incorreta do protocolo RPC ou um problema de autenticação de credenciais. A correção envolve adicionar credenciais do Windows e modificar um valor do registro.

---

## Solução

**1.** Abra o **Painel de Controle**, configure a visualização como **Ícones pequenos** e clique em **Gerenciador de Credenciais**.

![Painel de Controle — Gerenciador de Credenciais](/images/printer-error-0x00000709/step2.png)

**2.** Selecione **Credenciais do Windows** e clique em **Adicionar uma credencial do Windows**.

![Gerenciador de Credenciais — Adicionar uma credencial do Windows](/images/printer-error-0x00000709/step3.png)

**3.** No campo **Endereço de Internet ou de rede**, insira o endereço IP do PC host. Defina o **Nome de usuário** como `guest` e deixe o campo **Senha** em branco. Clique em **OK**.

![Adicionar credencial do Windows — inserir IP do host e nome de usuário convidado](/images/printer-error-0x00000709/step4.png)

**4.** Pressione **Win + R**, digite `regedit` e pressione Enter para abrir o **Editor do Registro**.

![Caixa de diálogo Executar com regedit](/images/printer-error-0x00000709/step5.png)

**5.** Navegue até a seguinte chave (crie-a se não existir):

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Clique com o botão direito no painel direito e crie um novo **Valor DWORD (32 bits)**.

![Editor do Registro — Criando novo valor DWORD sob a chave RPC](/images/printer-error-0x00000709/step6.png)

**6.** Renomeie o novo valor para `RpcUseNamedPipeProtocol` e defina seus **dados do valor** como `1`.

![Editor do Registro — RpcUseNamedPipeProtocol definido como 1](/images/printer-error-0x00000709/step7.png)

**7.** Após concluir todas as etapas, **reinicie o computador** e tente conectar à impressora compartilhada novamente.

---

## Resumo

O erro 0x00000709 ao conectar a uma impressora compartilhada é um problema comum no Windows, geralmente causado por uma atualização do sistema, uma configuração incorreta do protocolo RPC ou uma falha de autenticação. A correção inclui dois passos:

- Adicionar o endereço IP do PC host e a conta **guest** no **Gerenciador de Credenciais do Windows**
- Criar o valor de registro **RpcUseNamedPipeProtocol** (`1`) em `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

Após reiniciar, a conexão com a impressora compartilhada deverá funcionar normalmente.
