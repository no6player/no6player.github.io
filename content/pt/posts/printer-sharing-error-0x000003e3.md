---
title: "Como corrigir o erro 0x000003e3 de impressora compartilhada no Windows 10"
date: 2026-04-28
description: "Aparece o erro 0x000003e3 ao conectar uma impressora compartilhada no Windows 10? Este guia explica como ajustar a política de segurança local e a política de grupo para restaurar a conexão."
tags: ["windows 10", "erro 0x000003e3", "impressora compartilhada", "política de segurança", "política de grupo", "solução de problemas"]
categories: ["Solução de Problemas"]
cover:
  image: "/images/printer-error-0x000003e3/step1.png"
  alt: "Caixa de diálogo de erro do Windows exibindo o erro 0x000003e3 ao conectar uma impressora compartilhada"
  caption: "O erro 0x000003e3 aparece quando o Windows 10 não consegue se conectar a uma impressora compartilhada na rede local"
  relative: false
---

## Descrição do problema

No Windows 10, ao tentar conectar a uma impressora compartilhada na rede local, a conexão falha com o seguinte código de erro:

> **Erro 0x000003e3**

![Erro 0x000003e3 no Windows 10](/images/printer-error-0x000003e3/step1.png)

Este é um erro comum de permissão ou autenticação ao conectar impressoras compartilhadas. Geralmente é causado por acesso negado, uma anomalia de serviço ou um problema de protocolo de rede. Siga as etapas abaixo para resolver o problema.

---

## Solução

### Etapa 1 — Modificar a política de segurança local

**1.** Abra **Painel de Controle → Programas → Ativar ou desativar recursos do Windows** e habilite o **Suporte ao Compartilhamento de Arquivos SMB 1.0/CIFS**.

![Habilitar suporte SMB 1.0/CIFS nos Recursos do Windows](/images/printer-error-0x000003e3/step2.png)

**2.** Pressione **Win + R**, digite `secpol.msc` e pressione Enter para abrir o editor de **Política de Segurança Local**.

![Caixa de diálogo Executar com secpol.msc](/images/printer-error-0x000003e3/step3.png)

**3.** Navegue até **Políticas Locais → Atribuição de Direitos de Usuário → Acessar este computador a partir da rede**. Adicione a conta **Convidado** local.  
Em seguida, vá para **Negar acesso a este computador a partir da rede** e **remova** a conta Convidado dessa lista.

![Política de Segurança Local — Atribuição de Direitos de Usuário](/images/printer-error-0x000003e3/step4.png)

**4.** Navegue até **Políticas Locais → Opções de Segurança → Acesso à rede: modelo de compartilhamento e segurança para contas locais** e defina como **Somente Convidado – os usuários locais são autenticados como Convidado**.

![Política de Segurança Local — Modelo de compartilhamento definido como Somente Convidado](/images/printer-error-0x000003e3/step5.png)

---

### Etapa 2 — Modificar a política de grupo local

**1.** Pressione **Win + R**, digite `gpedit.msc` e pressione Enter para abrir o **Editor de Política de Grupo Local**.

![Caixa de diálogo Executar com gpedit.msc](/images/printer-error-0x000003e3/step6.png)

**2.** Navegue até **Configuração do Computador → Configurações do Windows → Configurações de Segurança → Políticas Locais → Opções de Segurança**. Localize a política **Contas: limitar o uso de contas locais com senhas em branco apenas para logon no console**, clique duas vezes, selecione **Desabilitado** e clique em **OK**.

![Política de Grupo — Desabilitar restrição de senha em branco](/images/printer-error-0x000003e3/step7.png)

**3.** Abra o **Gerenciamento do Computador**, vá para **Ferramentas do Sistema → Usuários e Grupos Locais → Usuários**. Clique duas vezes em **Convidado**, clique na guia **Membro de**, depois em **Adicionar**. Na caixa de diálogo, digite `users` e clique em **OK**.

![Gerenciamento do Computador — Adicionando Convidado ao grupo Usuários](/images/printer-error-0x000003e3/step8.png)

**4.** De volta à janela Propriedades do Convidado, em **Membro de**, selecione **Usuários**, clique em **Adicionar**, depois em **Aplicar** e **OK**.

![Propriedades do Convidado — Guia Membro de mostrando o grupo Usuários](/images/printer-error-0x000003e3/step9.png)

**5.** Após concluir todas as etapas, **reinicie o computador** e tente adicionar a impressora compartilhada novamente.

---

## Resumo

O erro 0x000003e3 ao conectar uma impressora compartilhada no Windows 10 geralmente é causado por configurações incorretas de política de segurança ou de grupo. A solução inclui:

- Habilitar o suporte ao compartilhamento de arquivos SMB 1.0/CIFS
- Conceder ao Convidado direitos de acesso à rede
- Definir o modelo de segurança de compartilhamento como "Somente Convidado"
- Desabilitar a restrição de logon no console para senhas em branco
- Adicionar a conta Convidado ao grupo local Usuários

Se o erro persistir após seguir todas as etapas, verifique também:

- Se a conexão LAN está funcionando corretamente (host e cliente devem estar na mesma sub-rede e conseguir se pingar mutuamente)
- Se o computador host da impressora compartilhada tem as configurações correspondentes
- Desinstalar e reinstalar o driver da impressora e tentar novamente
