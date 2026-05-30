---
title: "Como Corrigir o Erro de Impressora 0x00000032 no Windows 10 e 11"
date: 2026-05-30
description: "Recebendo o erro 0x00000032 (A solicitação não é suportada) ao instalar ou conectar uma impressora no Windows 10 ou 11? Este guia cobre todas as causas e soluções — limpeza de driver, redefinição do Spooler de Impressão, Política de Grupo e reparo de impressora compartilhada."
tags: ["windows", "error 0x00000032", "erro de impressora", "a solicitação não é suportada", "spooler de impressão", "driver de impressora", "política de grupo", "windows 10", "windows 11", "solução de problemas"]
categories: ["Solução de Problemas"]
cover:
  image: "/images/printer-error-0x00000032/step1.png"
  alt: "Caixa de diálogo de erro do Windows mostrando o erro 0x00000032 — A solicitação não é suportada"
  caption: "O erro 0x00000032 aparece quando o Windows se recusa a carregar ou instalar o driver da impressora devido a incompatibilidade ou um driver antigo bloqueado"
  relative: false
---

# Como Corrigir o Erro de Impressora 0x00000032 no Windows 10 e 11

O erro 0x00000032 — "A solicitação não é suportada" — é um dos erros de driver de impressora mais comuns no Windows 10 e Windows 11. Ele ocorre ao instalar uma impressora USB local, conectar a uma impressora de rede compartilhada ou atualizar um driver de impressora. Diferente dos erros de rede, o erro 0x00000032 é uma falha de classe de driver: o Windows está se recusando a carregar, substituir ou instalar o driver de impressora atual. Este guia explica cada causa e percorre cada solução em uma ordem lógica.

---

## O Que É o Erro 0x00000032?

Quando você instala ou conecta a uma impressora, o Windows exibe:

> **A operação não pôde ser concluída (Erro 0x00000032). A solicitação não é suportada.**

![Caixa de diálogo de erro do Windows mostrando o erro 0x00000032 — A solicitação não é suportada](/images/printer-error-0x00000032/step1.png)

**Os sintomas típicos incluem:**
- Adicionar uma impressora USB local falha imediatamente com este erro
- Conectar a uma impressora compartilhada na LAN falha em um PC cliente, mas funciona em outros
- Atualizar o driver da impressora aciona o erro mesmo após um download novo
- O driver genérico/universal instala corretamente, mas o driver oficial retorna 0x00000032

---

## Por Que o Erro 0x00000032 Acontece?

Existem seis causas comuns:

1. **Incompatibilidade de driver (mais comum)** — A versão do driver não corresponde ao driver do PC host, um driver de 32 bits é misturado com um sistema de 64 bits (ou vice-versa), ou o driver não suporta a versão do Windows. O Windows julga a solicitação de instalação inválida e a bloqueia.
2. **Arquivos de driver residuais bloqueando a instalação** — Arquivos e entradas de registro de um driver instalado anteriormente não foram completamente removidos. O driver está retido pelo sistema e não pode ser sobrescrito, acionando o 0x00000032.
3. **Falha no serviço Spooler de Impressão** — O processo do Spooler está congelado ou mantendo arquivos de driver abertos, impedindo que um novo driver seja gravado ou carregado.
4. **Bloqueio de política de instalação de driver do sistema** — A política de segurança do Windows está bloqueando a instalação de drivers não assinados ou de terceiros.
5. **Falha de sincronização de driver compartilhado** — Ao conectar a uma impressora compartilhada na LAN, o PC host não tem um driver correspondente para a versão do Windows do cliente (especialmente incompatibilidades entre 32 bits e 64 bits). O Windows recusa a solicitação de download do driver.
6. **Corrupção menor de arquivos do sistema** — Componentes do Windows relacionados à impressão ausentes ou danificados fazem com que a interface de instalação do driver pare de funcionar.

---

## Solução

Siga os passos abaixo em ordem. A maioria dos casos é resolvida pelos Passos 1 e 2.

### Passo 1 — Reiniciar o Spooler de Impressão e Limpar o Cache de Impressão

**1.** Pressione **Win + R**, digite `services.msc` e pressione Enter.

**2.** Encontre o **Spooler de Impressão** na lista, clique com o botão direito e selecione **Parar**.

![Janela de Serviços — parando o serviço Spooler de Impressão](/images/printer-error-0x00000032/step2.png)

**3.** Pressione **Win + R**, digite `C:\Windows\System32\spool\PRINTERS` e pressione Enter. Exclua **todos os arquivos** dentro da pasta (não exclua a pasta em si).

![Explorador de Arquivos — limpando todos os arquivos na pasta de spool PRINTERS](/images/printer-error-0x00000032/step3.png)

**4.** Volte para Serviços, clique com o botão direito em **Spooler de Impressão**, selecione **Iniciar** e defina o **Tipo de inicialização** como **Automático**.

---

### Passo 2 — Remover Completamente o Pacote de Driver Residual

O erro 0x00000032 é mais frequentemente causado por um driver antigo bloqueado. Você deve remover o pacote de driver completo — não apenas o dispositivo de impressora:

**1.** Abra o **Painel de Controle → Dispositivos e Impressoras**. Clique com o botão direito em cada impressora com falha ou duplicada e selecione **Remover dispositivo**.

**2.** Pressione **Win + R**, digite `printui /s /t2` e pressione Enter para abrir as **Propriedades do Servidor de Impressão**. Vá para a aba **Drivers**. Selecione o driver da impressora com problema, clique em **Remover** e escolha **Remover driver e pacote de driver** para excluir todos os arquivos do driver.

![Propriedades do Servidor de Impressão — removendo completamente o driver e o pacote de driver](/images/printer-error-0x00000032/step4.png)

**3.** Reinicie o computador. Em seguida, baixe o driver completo correto do site do fabricante e instale-o.

---

### Passo 3 — Ajustar a Política do Sistema para Permitir a Instalação do Driver

Nas edições Professional e Enterprise do Windows, a Política de Grupo pode estar bloqueando a instalação do driver:

**1.** Pressione **Win + R**, digite `gpedit.msc` e pressione Enter. Navegue até **Configuração do Computador → Modelos Administrativos → Impressoras**.

**2.** Desative **Não permitir a instalação de impressoras usando drivers no modo kernel** (se estiver habilitado).

**3.** Habilite **Permitir que o Spooler de Impressão aceite conexões de clientes**.

![Editor de Política de Grupo — ajustando as políticas de instalação de driver de impressora](/images/printer-error-0x00000032/step5.png)

---

### Passo 4 — Corrigir Problemas de Impressora Compartilhada (Conexão LAN)

Se o erro 0x00000032 aparecer apenas ao conectar a uma impressora compartilhada na rede local:

**1.** No **PC host**, adicione o pacote de driver adicional que corresponde à **versão do Windows do PC cliente** (32 bits ou 64 bits). Faça isso em **Propriedades do Servidor de Impressão → Drivers → Adicionar**.

**2.** No **PC cliente**, habilite **Habilitar logon de convidado inseguro**: abra **Política de Grupo Local → Configuração do Computador → Modelos Administrativos → Rede → Estação de Trabalho Lanman** e defina **Habilitar logons de convidado inseguros** como **Habilitado**.

**3.** Limpe as credenciais de rede obsoletas no PC cliente: pressione **Win + R**, digite `control keymgr.dll`, abra o **Gerenciador de Credenciais** e exclua todas as entradas relacionadas ao endereço IP ou nome do computador do PC host.

![Gerenciador de Credenciais — limpando credenciais de rede obsoletas no PC cliente](/images/printer-error-0x00000032/step6.png)

**4.** Reconecte usando o endereço IP do PC host diretamente: `\\192.168.1.x`.

---

### Passo 5 — Reparar Arquivos do Sistema

**1.** Pesquise por **cmd**, clique com o botão direito e selecione **Executar como administrador**.

**2.** Execute o seguinte comando e aguarde sua conclusão:

```
sfc /scannow
```

![Prompt de Comando — executando sfc /scannow para reparar arquivos do sistema](/images/printer-error-0x00000032/step7.png)

**3.** Reinicie o computador e tente instalar o driver da impressora novamente.

---

## Perguntas Frequentes

**P: O que realmente significa o erro 0x00000032 "A solicitação não é suportada"?**
Significa que o Windows está recusando a solicitação de instalação do driver. O sistema determinou que a solicitação é inválida — mais comumente porque o driver não corresponde à versão do sistema, um driver antigo conflitante está bloqueado no lugar, ou uma política de segurança está bloqueando a instalação.

**P: O driver genérico instala corretamente, mas o driver oficial gera o erro 0x00000032. Por quê?**
Isso é quase sempre causado por um pacote de driver residual bloqueado de uma instalação anterior. O driver genérico usa um caminho de instalação diferente, por isso funciona. Exclua o pacote de driver completo via `printui /s /t2`, reinicie e instale o driver oficial.

**P: Outros computadores podem conectar à impressora compartilhada — apenas o meu recebe o erro 0x00000032. O que devo fazer?**
O problema é específico ao ambiente de driver do seu PC cliente. Os outros PCs têm drivers compatíveis; o seu tem um conflito de driver antigo ou uma incompatibilidade de versão. Limpe o driver apenas no seu PC (Passos 1 e 2) — nenhuma alteração é necessária no PC host ou na impressora.

**P: Reinstalei o driver várias vezes, mas ainda recebo o erro. O que estou perdendo?**
Você provavelmente excluiu o dispositivo de impressora, mas não o **pacote de driver**. Simplesmente remover o dispositivo de Dispositivos e Impressoras deixa os arquivos do driver no lugar. Abra `printui /s /t2`, vá para a aba Drivers e exclua o pacote de driver completamente antes de reinstalar.

**P: Uma incompatibilidade de 32 bits/64 bits entre o host e o cliente pode causar o erro 0x00000032?**
Sim. Se o PC host for de 64 bits e o cliente for de 32 bits, e o host não tiver adicionado um pacote de driver de 32 bits, o cliente não poderá baixar um driver correspondente e o Windows retornará "A solicitação não é suportada." Adicione o driver da arquitetura ausente no PC host nas Propriedades do Servidor de Impressão.

---

## Resumo

O erro 0x00000032 ao instalar ou conectar uma impressora no Windows 10 ou Windows 11 significa que o Windows está recusando a solicitação de instalação do driver. Não está relacionado à rede, ao hardware da impressora ou a danos graves no sistema. Corrija-o nesta ordem:

1. **Reiniciar o Spooler de Impressão e limpar o cache de impressão**
2. **Remover completamente o pacote de driver antigo** via `printui /s /t2` — esta é a correção mais comum
3. **Ajustar a Política de Grupo** para permitir a instalação do driver (edições Professional/Enterprise)
4. **Para impressoras compartilhadas**: adicione drivers de arquitetura correspondentes no host, habilite o logon de convidado no cliente e limpe as credenciais obsoletas
5. **Execute `sfc /scannow`** para reparar quaisquer arquivos de sistema corrompidos

---

## Artigos Relacionados

- [Como Corrigir o Erro de Impressora Compartilhada 0x00000006 no Windows](/pt/posts/printer-sharing-error-0x00000006/)
- [Como Corrigir o Erro de Impressora Compartilhada 0x00000035 no Windows](/pt/posts/printer-sharing-error-0x00000035/)
- [Como Corrigir o Erro de Impressora Compartilhada 0x00000709 no Windows](/pt/posts/printer-sharing-error-0x00000709/)
