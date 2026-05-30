---
title: "Como Corrigir o Erro de Impressora Compartilhada 0x0000007c no Windows 10 e 11"
date: 2026-05-30
description: "Recebendo o erro 0x0000007c ao conectar a uma impressora compartilhada no Windows 10 ou 11? Este guia cobre todas as causas e soluções — limpeza de driver, redefinição do Spooler de Impressão, protocolo SMB 1.0 e adição de drivers bidirecionais no host."
tags: ["windows", "error 0x0000007c", "impressora compartilhada", "driver de impressora", "SMB", "spooler de impressão", "windows 10", "windows 11", "solução de problemas"]
categories: ["Solução de Problemas"]
cover:
  image: "/images/printer-error-0x0000007c/step1.png"
  alt: "Caixa de diálogo de erro do Windows mostrando o erro 0x0000007c — A operação não pôde ser concluída"
  caption: "O erro 0x0000007c aparece quando o cliente não consegue carregar o driver de impressora remoto devido a incompatibilidade de driver ou protocolo SMB"
  relative: false
---

# Como Corrigir o Erro de Impressora Compartilhada 0x0000007c no Windows 10 e 11

O erro 0x0000007c — "A operação não pôde ser concluída" — é um erro comum de impressora compartilhada no Windows 10 e Windows 11. Ele ocorre quando o PC cliente se conecta a uma impressora compartilhada na LAN e o Windows não consegue carregar o driver de impressora remoto. Este é um problema de **compatibilidade de driver e protocolo SMB**, não uma falha de hardware ou interrupção de rede. Este guia explica cada causa e percorre cada solução em ordem.

---

## O Que É o Erro 0x0000007c?

Ao conectar a uma impressora compartilhada na rede local, o Windows exibe:

> **A operação não pôde ser concluída (Erro 0x0000007c).**

![Caixa de diálogo de erro do Windows mostrando o erro 0x0000007c — A operação não pôde ser concluída](/images/printer-error-0x0000007c/step1.png)

**Os sintomas típicos incluem:**
- O erro aparece imediatamente ao tentar adicionar uma impressora compartilhada na LAN
- Os compartilhamentos de arquivos de rede no mesmo PC host estão acessíveis — somente a impressora falha
- PCs mais antigos conectam à impressora compartilhada sem problemas; apenas PCs com Windows 10/11 mais novos recebem o erro
- Excluir e readicionar a impressora não resolve o problema

---

## Por Que o Erro 0x0000007c Acontece?

Existem seis causas comuns:

1. **Incompatibilidade de driver remoto** — A versão do driver de impressora ou a arquitetura (32 bits vs. 64 bits) no PC host não corresponde ao que o PC cliente requer. O cliente rejeita o driver enviado como inválido.
2. **Incompatibilidade de versão do protocolo SMB** — O Windows 10/11 usa SMB 3.0 por padrão, enquanto PCs host mais antigos ou drivers legados dependem do SMB 1.0. A incompatibilidade de protocolo faz com que a validação da transferência de driver falhe.
3. **Bloqueio de verificação de assinatura de driver** — Versões mais recentes do Windows impõem estritamente os requisitos de assinatura digital do driver. Drivers de impressora compartilhada mais antigos sem uma assinatura válida são bloqueados de carregar.
4. **Conflito de driver residual no cliente** — Um driver instalado anteriormente para o mesmo modelo de impressora deixou arquivos residuais que impedem o carregamento do novo driver remoto.
5. **Corrupção do cache do Spooler de Impressão** — Dados de cache do Spooler corrompidos impedem que o serviço receba e instale o driver compartilhado remoto.
6. **PC host sem drivers bidirecionais** — O host tem apenas um driver instalado para sua própria versão do Windows e não adicionou um pacote de driver correspondente para a arquitetura do PC cliente.

---

## Solução

Siga os passos abaixo em ordem. A maioria dos casos é resolvida pelos Passos 1 e 2.

### Passo 1 — Limpar o Cache de Impressão e Reiniciar o Spooler de Impressão

**1.** Pressione **Win + R**, digite `services.msc` e pressione Enter. Encontre o **Spooler de Impressão** na lista, clique com o botão direito e selecione **Parar**.

![Janela de Serviços — parando o serviço Spooler de Impressão](/images/printer-error-0x0000007c/step2.png)

**2.** Pressione **Win + R**, digite `C:\Windows\System32\spool\PRINTERS` e pressione Enter. Exclua **todos os arquivos** dentro da pasta (não exclua a pasta em si). Em seguida, volte para Serviços e inicie o **Spooler de Impressão** novamente, definindo o **Tipo de inicialização** como **Automático**.

![Explorador de Arquivos — limpando todos os arquivos na pasta de spool PRINTERS](/images/printer-error-0x0000007c/step3.png)

---

### Passo 2 — Remover Completamente o Driver Antigo do PC Cliente

**1.** Abra o **Painel de Controle → Dispositivos e Impressoras**. Clique com o botão direito em cada impressora com falha e selecione **Remover dispositivo**.

**2.** Pressione **Win + R**, digite `printui /s /t2` e pressione Enter para abrir as **Propriedades do Servidor de Impressão**. Vá para a aba **Drivers**, selecione o driver de impressora correspondente, clique em **Remover** e escolha **Remover driver e pacote de driver** para excluir completamente todos os arquivos do driver.

![Propriedades do Servidor de Impressão — removendo completamente o driver e o pacote de driver](/images/printer-error-0x0000007c/step4.png)

**3.** Reinicie o computador. Baixe o driver completo correto do site do fabricante e instale-o manualmente antes de reconectar à impressora compartilhada.

---

### Passo 3 — Habilitar o Protocolo SMB 1.0/CIFS

Se o PC host estiver executando uma versão mais antiga do Windows ou usar pacotes de driver mais antigos que dependem do SMB 1.0:

**1.** Abra o **Painel de Controle → Programas → Ativar ou desativar recursos do Windows**.

**2.** Role para baixo e marque **Suporte a Compartilhamento de Arquivos SMB 1.0/CIFS**. Clique em **OK** e reinicie o computador.

![Recursos do Windows — habilitando o Suporte a Compartilhamento de Arquivos SMB 1.0/CIFS](/images/printer-error-0x0000007c/step5.png)

> **Nota de segurança:** O SMB 1.0 tem vulnerabilidades conhecidas. Habilite-o apenas como medida temporária e desative-o assim que o problema da impressora for resolvido ou atualize o driver do PC host para uma versão compatível com SMB 2.0/3.0.

---

### Passo 4 — Adicionar Drivers Bidirecionais no PC Host

Se o PC cliente tiver uma arquitetura Windows diferente (32 bits vs. 64 bits) do host:

**1.** No **PC host**, abra **Dispositivos e Impressoras**, clique com o botão direito na impressora compartilhada e selecione **Propriedades da impressora → aba Compartilhamento → Drivers Adicionais**.

**2.** Marque a arquitetura que corresponde ao **PC cliente** (por exemplo, marque **x86** se o cliente for de 32 bits e o host for de 64 bits), clique em **OK** e instale o pacote de driver correspondente.

Após o host ter o driver correspondente, reconecte à impressora compartilhada a partir do PC cliente.

---

## Perguntas Frequentes

**P: O que significa o erro 0x0000007c?**
O significado oficial é "o driver de impressora é inválido ou o driver de impressora remoto não pode ser carregado." Em termos simples, o sistema Windows do PC cliente não aceita o driver enviado pelo host. Isso é causado por incompatibilidade de protocolo ou incompatibilidade de versão do driver, não por falha de hardware.

**P: Consigo fazer ping no host e acessar pastas compartilhadas, mas a impressora apresenta o erro 0x0000007c. Por quê?**
Acessar pastas compartilhadas requer apenas conectividade de rede SMB. Conexões de impressora compartilhada requerem tanto **compatibilidade de protocolo quanto compatibilidade de driver**. Uma rede funcionando não garante compatibilidade de driver. Habilite o SMB 1.0 e instale manualmente o driver de impressora local para resolver isso.

**P: PCs antigos conectam à impressora compartilhada sem problemas. Por que meu novo PC com Windows 10/11 recebe o erro 0x0000007c?**
O Windows 10/11 desabilita o SMB 1.0 por padrão e impõe uma validação de assinatura de driver mais rigorosa do que as versões mais antigas do Windows. Os PCs mais antigos funcionam porque usam configurações de compatibilidade mais permissivas. O novo sistema bloqueia o driver compartilhado legado.

**P: Continuo excluindo e readicionando a impressora, mas o erro persiste. O que estou perdendo?**
Excluir o dispositivo de impressora sozinho não remove os arquivos do driver. Você deve excluir o **pacote de driver** usando `printui /s /t2`. Reinicie o computador e pré-instale o driver correto manualmente antes de reconectar à impressora compartilhada.

**P: Preciso editar o registro para corrigir o erro 0x0000007c?**
Não, na grande maioria dos casos. O erro 0x0000007c é resolvido habilitando o SMB 1.0, limpando o pacote de driver antigo e instalando manualmente o driver correto — sem necessidade de editar o registro.

---

## Resumo

O erro 0x0000007c ao conectar a uma impressora compartilhada no Windows 10 ou Windows 11 significa que o cliente não consegue carregar o driver de impressora remoto devido a incompatibilidade de driver ou protocolo SMB. Não está relacionado ao hardware da impressora ou danos ao sistema. Corrija-o nesta ordem:

1. **Limpar o cache de impressão e reiniciar o Spooler de Impressão**
2. **Remover completamente o pacote de driver antigo** via `printui /s /t2` e reinstalar manualmente
3. **Habilitar SMB 1.0/CIFS** se o host usar pacotes de driver legados
4. **Adicionar drivers bidirecionais no host** se o cliente e o host tiverem arquiteturas Windows diferentes

---

## Artigos Relacionados

- [Como Corrigir o Erro de Impressora Compartilhada 0x00000035 no Windows](/pt/posts/printer-sharing-error-0x00000035/)
- [Como Corrigir o Erro de Impressora Compartilhada 0x00000032 no Windows](/pt/posts/printer-sharing-error-0x00000032/)
- [Como Corrigir o Erro de Impressora Compartilhada 0x00000bcb no Windows](/pt/posts/printer-sharing-error-0x00000bcb/)
