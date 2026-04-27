---
title: "Como corrigir o erro 0x0000011b ao conectar impressora compartilhada no Windows"
date: 2026-04-27
description: "Recebendo a mensagem 'O Windows não consegue se conectar à impressora, erro 0x0000011b'? Este guia mostra a solução comprovada pelo registro para restaurar a conexão imediatamente."
tags: ["windows", "erro 0x0000011b", "impressora compartilhada", "registro", "solução de problemas"]
categories: ["Solução de Problemas"]
cover:
  image: "/images/printer-error-0x0000011b/step1.png"
  alt: "Caixa de diálogo de erro do Windows 0x0000011b"
  caption: "O erro 0x0000011b aparece quando o Windows não consegue se conectar à impressora compartilhada"
  relative: false
---

As impressoras compartilhadas são essenciais em ambientes de escritório e redes domésticas. No entanto, um erro frustrante impede muitos usuários de se conectar:

> **«O Windows não consegue se conectar à impressora. Falha na operação com erro 0x0000011b.»**

![Caixa de diálogo de erro 0x0000011b no Windows](/images/printer-error-0x0000011b/step1.png)

Este erro surgiu em massa após uma atualização de segurança da Microsoft (KB5005565, final de 2021). A solução leva menos de dois minutos.

---

## Por que o erro 0x0000011b ocorre?

A causa é uma mudança na **privacidade de autenticação RPC**. Após a atualização, o Windows exige um nível de autenticação mais rigoroso que sistemas mais antigos nem sempre conseguem negociar, causando o erro `0x0000011b`.

---

## Solução — Editar o Registro do Windows

> ⚠️ **Antes de começar:** Faça backup do registro: **Arquivo → Exportar**.

### Passo 1 — Abrir o Editor do Registro

Pressione **Win + R**, digite `regedit` e pressione **Enter**.

![Pesquisa por regedit no menu Iniciar do Windows](/images/printer-error-0x0000011b/step2.png)

### Passo 2 — Navegar até a chave Print

Cole o seguinte caminho na barra de endereços e pressione **Enter**:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print
```

![Editor do Registro com o caminho Control\Print](/images/printer-error-0x0000011b/step3.png)

### Passo 3 — Criar um novo valor DWORD

1. Clique com o botão direito em área vazia do painel direito.
2. Selecione **Novo → Valor DWORD (32 bits)**.

![Menu de contexto para criar um novo valor DWORD](/images/printer-error-0x0000011b/step4.png)

3. Nomeie o valor exatamente: `RpcAuthnLevelPrivacyEnabled`

### Passo 4 — Definir o valor como 0

1. Clique duas vezes em `RpcAuthnLevelPrivacyEnabled`.
2. Insira `0` no campo **Dados do valor**.
3. Clique em **OK**.

![Configurando RpcAuthnLevelPrivacyEnabled como 0](/images/printer-error-0x0000011b/step5.png)

### Passo 5 — Reiniciar e verificar

Reinicie o PC e conecte-se novamente à impressora compartilhada.

![Fila de impressão mostrando conexão bem-sucedida](/images/printer-error-0x0000011b/step6.png)

---

## O erro persiste?

- **Aplique a solução no computador host** — mesma alteração no PC que compartilha a impressora.
- **Verifique o Firewall** — permitir compartilhamento de arquivos e impressoras nos dois lados.
- **Mesma rede** — ambos os computadores devem estar na mesma sub-rede.
- **Reinicie o Spooler de Impressão** — `services.msc` → **Spooler de Impressão** → **Reiniciar**.

---

## Resumo

| Passo | Ação |
|---|---|
| 1 | Abrir o Editor do Registro (`Win + R` → `regedit`) |
| 2 | Navegar até `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print` |
| 3 | Criar novo valor **DWORD (32 bits)** |
| 4 | Nomear `RpcAuthnLevelPrivacyEnabled` e definir como `0` |
| 5 | Reiniciar o computador |
