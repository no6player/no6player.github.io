---
title: "Como corrigir o erro 0x000006d9 ao compartilhar uma impressora no Windows"
date: 2026-05-12
description: "Aparece o erro 0x000006d9 ao compartilhar uma impressora no Windows 10 ou 11? A solução é simples: inicie o serviço Firewall do Windows. Este guia explica as causas e os passos necessários."
tags: ["windows", "erro 0x000006d9", "compartilhar impressora", "serviço firewall windows", "services.msc", "windows 10", "windows 11", "solução de problemas"]
categories: ["Solução de Problemas"]
cover:
  image: "/images/printer-error-0x000006d9/step1.png"
  alt: "Caixa de diálogo de erro do Windows exibindo o erro 0x000006d9 — Não é possível salvar as configurações da impressora"
  caption: "O erro 0x000006d9 aparece quando o serviço Firewall do Windows não está em execução, impedindo a ativação do compartilhamento de impressora"
  relative: false
---

# Como corrigir o erro 0x000006d9 ao compartilhar uma impressora no Windows

O erro 0x000006d9 é um erro comum do Windows que aparece especificamente ao tentar **compartilhar** uma impressora — não ao usá-la. A impressora se conecta e imprime normalmente, mas ao abrir as configurações de compartilhamento e clicar em Aplicar, o Windows exibe:

> **Não é possível salvar as configurações da impressora. A operação não pôde ser concluída (Erro 0x000006d9).**

Este guia explica exatamente por que o erro 0x000006d9 ocorre e fornece uma correção clara passo a passo para Windows 10 e Windows 11.

---

## O que é o erro 0x000006d9?

Quando você ativa o compartilhamento de impressora no Windows, o sistema depende do **serviço Firewall do Windows** para registrar e expor a impressora compartilhada na rede — mesmo que o próprio firewall esteja desativado. Se o *serviço* Firewall do Windows estiver parado ou desabilitado, o Windows não consegue concluir a configuração do compartilhamento e retorna o erro 0x000006d9.

![Erro 0x000006d9 no Windows — Não é possível salvar as configurações da impressora](/images/printer-error-0x000006d9/step1.png)

A mensagem de erro menciona "armazenamento insuficiente", o que é enganoso. No contexto do compartilhamento de impressoras, o erro 0x000006d9 não tem nada a ver com espaço em disco — ele sempre indica que o serviço Firewall do Windows não está em execução.

---

## Por que o erro 0x000006d9 ocorre?

Existem quatro causas comuns:

1. **O serviço Firewall do Windows está parado** — O compartilhamento de impressora requer que este serviço esteja *em execução*, independentemente de o firewall estar ativado ou não.
2. **O serviço Firewall do Windows está desabilitado** — Ferramentas de otimização do sistema, instalações enxutas do Windows ou ajustes manuais frequentemente desabilitam este serviço completamente.
3. **Serviços dependentes não estão em execução** — Serviços que dependem do Firewall do Windows também podem estar parados ou em estado anômalo.
4. **Arquivos do sistema corrompidos** — Em casos raros, arquivos do sistema ausentes ou danificados impedem a inicialização do serviço.

---

## Solução

### Etapa 1 — Abrir o Gerenciador de Serviços

Pressione **Win + R**, digite `services.msc` e pressione Enter para abrir a janela **Serviços**.

![Caixa de diálogo Executar com services.msc](/images/printer-error-0x000006d9/step2.png)

### Etapa 2 — Localizar o serviço Firewall do Windows

Role a lista para localizar **Firewall do Windows** (também listado como *MpsSvc* em algumas versões).

![Janela Serviços — localizando o serviço Firewall do Windows](/images/printer-error-0x000006d9/step3.png)

### Etapa 3 — Habilitar e iniciar o serviço

Clique duas vezes em **Firewall do Windows** para abrir suas propriedades. Defina o **Tipo de inicialização** como **Automático** e clique em **Iniciar**. Clique em **OK** para salvar.

![Propriedades do serviço Firewall do Windows — tipo de inicialização Automático e início do serviço](/images/printer-error-0x000006d9/step4.png)

> **Importante:** Iniciar o *serviço* Firewall do Windows não ativa o próprio firewall. Você ainda pode manter o firewall desativado nas configurações de Segurança do Windows — o serviço só precisa estar em execução para que o compartilhamento de impressora funcione.

### Etapa 4 — Se Firewall do Windows não aparecer na lista

Em algumas instalações personalizadas ou enxutas do Windows, o serviço Firewall do Windows pode estar ausente da lista. Nesse caso, inicie primeiro estes dois serviços relacionados:

**Windows Update:** Clique duas vezes em **Windows Update**, defina o tipo de inicialização como **Automático**, clique em **Iniciar** e depois em **OK**.

![Propriedades do serviço Windows Update](/images/printer-error-0x000006d9/step5.png)

**Windows Installer:** Localize **Windows Installer**, clique duas vezes, defina o tipo de inicialização como **Automático**, clique em **Iniciar** e depois em **OK**.

![Propriedades do serviço Windows Installer](/images/printer-error-0x000006d9/step6.png)

### Etapa 5 — Reiniciar e tentar novamente

Reinicie o computador. Após reiniciar, abra **Serviços** e verifique se **Firewall do Windows** agora aparece na lista. Se aparecer, defina-o como **Automático** e inicie-o (como na Etapa 3). Em seguida, volte às configurações da impressora e ative o compartilhamento — o erro 0x000006d9 deve ter desaparecido.

---

## Perguntas frequentes

**P: O erro 0x000006d9 significa que o disco está cheio?**
Não. Embora a mensagem mencione "armazenamento insuficiente", no contexto do compartilhamento de impressoras este código de erro indica exclusivamente que o serviço Firewall do Windows não está em execução. Não tem nada a ver com espaço em disco.

**P: Já desativei o firewall — por que o erro ainda aparece?**
Porque o compartilhamento de impressoras não verifica se o firewall está ativado ou não, apenas se o *serviço* Firewall do Windows está em execução. O serviço deve estar iniciado mesmo que o firewall esteja desativado.

**P: Iniciar o serviço Firewall do Windows afetará minhas configurações de segurança?**
Não. Você ainda pode manter o firewall desativado em **Segurança do Windows → Firewall e proteção de rede**. Iniciar o serviço não ativa automaticamente o firewall.

**P: O serviço Firewall do Windows precisa ficar em execução permanentemente?**
Sim, para que o compartilhamento de impressora continue funcionando. Ao definir o tipo de inicialização como **Automático**, garante-se que ele seja iniciado com o Windows sempre.

---

## Resumo

O erro 0x000006d9 ao compartilhar uma impressora no Windows tem uma única causa: **o serviço Firewall do Windows não está em execução**. A correção:

1. Abrir **Serviços** (`services.msc`)
2. Localizar **Firewall do Windows**, definir o tipo de inicialização como **Automático** e clicar em **Iniciar**
3. Reiniciar o computador e reativar o compartilhamento de impressora

---

## Artigos relacionados

- [Corrigir o erro de impressora compartilhada 0x00000bcb no Windows](/pt/posts/printer-sharing-error-0x00000bcb/)
- [Corrigir o erro de impressora compartilhada 0x00000709 no Windows](/pt/posts/printer-sharing-error-0x00000709/)
- [Corrigir o erro de impressora compartilhada 0x0000011b no Windows](/pt/posts/printer-sharing-error-0x0000011b/)
