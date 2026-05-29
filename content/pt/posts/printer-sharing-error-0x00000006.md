---
title: "Como Corrigir o Erro de Impressora Compartilhada 0x00000006 no Windows 10 e 11"
date: 2026-05-29
description: "Recebendo o erro 0x00000006 (Identificador Inválido) ao conectar a uma impressora compartilhada no Windows 10 ou 11? Este guia abrange todas as causas e soluções — serviço Print Spooler, reinstalação de driver e reparo de arquivos do sistema."
tags: ["windows", "erro 0x00000006", "impressora compartilhada", "identificador inválido", "print spooler", "driver de impressora", "sfc scannow", "windows 10", "windows 11", "solução de problemas"]
categories: ["Troubleshooting"]
cover:
  image: "/images/printer-error-0x00000006/step1.png"
  alt: "Caixa de diálogo de erro do Windows exibindo o erro 0x00000006 — O identificador é inválido"
  caption: "O erro 0x00000006 aparece quando o Windows não consegue gerar um identificador válido para a conexão com a impressora compartilhada"
  relative: false
---

# Como Corrigir o Erro de Impressora Compartilhada 0x00000006 no Windows 10 e 11

O erro 0x00000006 — "O identificador é inválido" — é um erro comum de impressora compartilhada no Windows 10 e Windows 11. Ele aparece quando o Windows não consegue gerar ou reconhecer um identificador de conexão válido para a impressora compartilhada. Ao contrário dos erros de caminho de rede, o erro 0x00000006 é causado por problemas com o serviço Print Spooler, drivers de impressora ou componentes do sistema — não pela rede em si. Este guia percorre todas as causas e suas correções em uma ordem lógica.

---

## O Que É o Erro 0x00000006?

Quando você tenta conectar a uma impressora compartilhada na sua rede local, o Windows exibe:

> **A operação não pôde ser concluída (Erro 0x00000006). O identificador é inválido.**

![Caixa de diálogo de erro do Windows exibindo o erro 0x00000006 — O identificador é inválido](/images/printer-error-0x00000006/step1.png)

**Os sintomas típicos incluem:**
- Clicar em "Conectar" ao adicionar uma impressora compartilhada retorna o erro imediatamente
- A impressora é adicionada com sucesso, mas o envio de um trabalho de impressão aciona o erro
- A conexão com a impressora cai e o erro 0x00000006 aparece ao reconectar
- Outros dispositivos na mesma rede imprimem normalmente — apenas PCs específicos acionam o erro

---

## Por Que o Erro 0x00000006 Acontece?

Existem cinco causas comuns:

1. **Falha no serviço Print Spooler** — A causa mais comum. Se o serviço Spooler travar, congelar ou tiver arquivos de cache corrompidos, o Windows não consegue gerar um identificador de conexão válido.
2. **Problemas com o driver de impressora** — O PC cliente não possui o driver correto, tem uma versão de driver incompatível ou possui arquivos residuais de um driver antigo que conflitam com uma nova instalação.
3. **Arquivos do sistema ou componentes de impressão corrompidos** — Atualizações do Windows com falha, ferramentas agressivas de limpeza do sistema ou software antivírus que exclui arquivos do sistema relacionados à impressão podem causar esse erro.
4. **Problemas de compartilhamento de rede ou de caminho** — O nome do compartilhamento da impressora contém caracteres especiais, espaços ou símbolos, ou o endereço IP do host não pode ser resolvido pelo cliente.
5. **Bloqueios de permissão ou política de segurança** — O cliente não tem permissão para acessar recursos compartilhados no PC host, ou o Firewall do Windows / software de segurança de terceiros está bloqueando a comunicação de compartilhamento de impressora.

---

## Solução

Siga os passos abaixo em ordem — a maioria dos casos é resolvida na Etapa 1 ou na Etapa 2.

### Etapa 1 — Reiniciar o Print Spooler e Limpar a Fila de Impressão

**1.** Pressione **Win + R**, digite `services.msc` e pressione Enter para abrir a janela Serviços.

![Janela Serviços — abrindo services.msc via caixa de diálogo Executar](/images/printer-error-0x00000006/step2.png)

**2.** Localize **Print Spooler** na lista, clique com o botão direito e selecione **Parar**. Aguarde 5 a 10 segundos até o serviço parar completamente.

![Janela Serviços — parando o serviço Print Spooler](/images/printer-error-0x00000006/step3.png)

**3.** Pressione **Win + R**, digite `C:\Windows\System32\spool\PRINTERS` e pressione Enter. Exclua **todos os arquivos** dentro desta pasta (não exclua a pasta em si). Isso limpa o cache de impressão corrompido.

![Explorador de Arquivos — limpando a pasta de spool PRINTERS](/images/printer-error-0x00000006/step4.png)

**4.** Retorne à janela Serviços, clique com o botão direito em **Print Spooler**, selecione **Iniciar** e defina o **Tipo de inicialização** como **Automático** para que o serviço inicie automaticamente após cada reinicialização.

![Janela Serviços — iniciando o Print Spooler e configurando-o como Automático](/images/printer-error-0x00000006/step5.png)

**5.** Reinicie o PC cliente e tente conectar à impressora compartilhada novamente.

---

### Etapa 2 — Remover o Driver Antigo e Reinstalar um Driver Limpo

**1.** Abra **Painel de Controle → Dispositivos e Impressoras**. Clique com o botão direito em cada entrada de impressora com falha ou duplicada e selecione **Remover dispositivo** para limpar todas as conexões inválidas.

![Painel de Controle — removendo entradas de impressora com falha de Dispositivos e Impressoras](/images/printer-error-0x00000006/step6.png)

**2.** Pressione **Win + R**, digite `printui /s /t2` e pressione Enter para abrir **Propriedades do Servidor de Impressão**. Vá para a guia **Drivers**.

**3.** Localize o driver da impressora compartilhada de destino, selecione-o, clique em **Remover** e na caixa de diálogo marque **Remover driver e pacote de driver** para excluir todos os arquivos do driver completamente.

![Propriedades do Servidor de Impressão — removendo o pacote de driver de impressora](/images/printer-error-0x00000006/step7.png)

**4.** Reinicie o computador. Em seguida, baixe o driver completo correto para o modelo da sua impressora no site do fabricante e instale-o manualmente.

**5.** Após a instalação do driver, adicione a impressora compartilhada novamente — use o endereço IP do PC host para uma conexão mais confiável (por exemplo `\\192.168.1.100`).

---

### Etapa 3 — Reparar Arquivos do Sistema

**1.** Pesquise por **cmd**, clique com o botão direito e selecione **Executar como administrador**. Clique em **Sim** se aparecer um prompt do Controle de Conta de Usuário.

![Prompt de Comando — abrindo como administrador](/images/printer-error-0x00000006/step8.png)

**2.** Digite o seguinte comando e pressione Enter. Não feche a janela enquanto ele estiver em execução:

```
sfc /scannow
```

![Prompt de Comando — executando sfc /scannow para reparar arquivos do sistema](/images/printer-error-0x00000006/step9.png)

**3.** Após a conclusão da verificação, reinicie o computador e tente conectar à impressora compartilhada.

---

## Perguntas Frequentes

**P: O que "O identificador é inválido" significa no erro 0x00000006?**
No Windows, um "identificador" é um código único que o sistema usa para rastrear um recurso — neste caso, a conexão com a impressora. "Identificador inválido" significa que o Windows tentou criar ou referenciar um identificador de conexão, mas falhou, geralmente porque o serviço Print Spooler ou o driver de impressora está em um estado corrompido.

**P: Outros PCs conectam normalmente, mas apenas o meu exibe o erro 0x00000006. O que devo fazer?**
O problema está totalmente no seu PC cliente — o PC host e a impressora estão funcionando normalmente. Execute a Etapa 1 (reiniciar o Print Spooler) e a Etapa 2 (reinstalar o driver) apenas no seu PC cliente. Não são necessárias alterações no host.

**P: O ping funciona e a rede está boa, mas ainda recebo o erro 0x00000006. Por quê?**
Um ping bem-sucedido confirma apenas a conectividade IP básica. O erro 0x00000006 é causado pelo serviço Print Spooler ou pelo driver — não pelo caminho de rede. Concentre-se na Etapa 1 e na Etapa 2 em vez das configurações de rede.

**P: Excluir drivers ou modificar serviços afetará o uso normal do computador?**
Não. Todas as etapas deste guia são operações padrão de solução de problemas de impressora do Windows. Elas afetam apenas serviços, drivers e configurações relacionados à impressão — não a configuração principal do sistema, acesso à internet ou outro software. São seguras tanto para ambientes domésticos quanto corporativos.

**P: Depois de corrigir o erro, como evito que ele volte?**
Certifique-se de que o serviço **Print Spooler** esteja configurado para inicialização **Automática**. Além disso, mantenha o driver da impressora atualizado e evite caracteres especiais ou espaços no nome do compartilhamento da impressora no PC host.

---

## Resumo

O erro 0x00000006 ao conectar a uma impressora compartilhada no Windows 10 ou Windows 11 significa que o Windows não consegue gerar um identificador válido para a conexão com a impressora. Não é causado pelo hardware da impressora e não requer a reinstalação do Windows. Corrija-o nesta ordem:

1. **Reinicie o Print Spooler e limpe o cache de impressão** — a correção mais comum
2. **Remova completamente o driver antigo e reinstale um driver limpo**
3. **Execute `sfc /scannow`** para reparar arquivos do sistema corrompidos

---

## Artigos Relacionados

- [Como Corrigir o Erro de Impressora Compartilhada 0x00000035 no Windows](/pt/posts/printer-sharing-error-0x00000035/)
- [Como Corrigir o Erro de Impressora Compartilhada 0x000006d9 no Windows](/pt/posts/printer-sharing-error-0x000006d9/)
- [Como Corrigir o Erro de Impressora Compartilhada 0x00000709 no Windows](/pt/posts/printer-sharing-error-0x00000709/)
