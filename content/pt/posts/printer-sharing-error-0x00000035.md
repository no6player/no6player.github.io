---
title: "Como corrigir o erro 0x00000035 de impressora compartilhada no Windows 10 e 11"
date: 2026-05-28
description: "Aparece o erro 0x00000035 ao conectar uma impressora compartilhada no Windows 10 ou 11? Este guia aborda todas as causas e soluções: conectividade de rede, configurações de compartilhamento, firewall, protocolo SMB e limpeza de credenciais."
tags: ["windows", "erro 0x00000035", "erro 0x80070035", "impressora compartilhada", "caminho de rede não encontrado", "SMB", "firewall windows", "windows 10", "windows 11", "solução de problemas"]
categories: ["Solução de Problemas"]
cover:
  image: "/images/printer-error-0x00000035/step1.png"
  alt: "Caixa de diálogo de erro do Windows exibindo o erro 0x00000035 — O caminho de rede não foi encontrado"
  caption: "O erro 0x00000035 aparece quando o Windows não consegue localizar o caminho compartilhado da impressora na rede local"
  relative: false
---

# Como corrigir o erro 0x00000035 de impressora compartilhada no Windows 10 e 11

O erro 0x00000035 — «O caminho de rede não foi encontrado» — é um dos erros de impressora compartilhada mais comuns no Windows 10 e Windows 11. Ao contrário de outros erros de impressora causados por uma única configuração, o erro 0x00000035 pode ter múltiplas causas. Este guia percorre cada causa e sua solução em ordem lógica.

---

## O que é o erro 0x00000035?

Ao tentar conectar a uma impressora compartilhada na rede local, o Windows exibe:

> **A operação não pôde ser concluída (Erro 0x00000035). O caminho de rede não foi encontrado.**

![Erro 0x00000035 no Windows — O caminho de rede não foi encontrado](/images/printer-error-0x00000035/step1.png)

**Sintomas típicos:**
- Inserir o IP do host ou o nome do computador na caixa Executar retorna o erro imediatamente
- Alguns dispositivos conseguem fazer ping no IP do host, mas não conseguem acessar recursos compartilhados
- Reiniciar o PC ou reconfigurar o compartilhamento da impressora não resolve o problema
- Outros dispositivos na mesma rede se conectam normalmente — apenas PCs específicos acionam o erro

O erro 0x00000035 também é reportado como **0x80070035** em alguns sistemas; ambos os códigos significam «caminho de rede não encontrado» e são corrigidos da mesma forma.

---

## Por que o erro 0x00000035 ocorre?

Existem seis causas comuns:

1. **Problema de ambiente de rede** — O host e o cliente não estão na mesma rede local ou sub-rede.
2. **Descoberta de rede e compartilhamento de arquivos desabilitados** — O host ou o cliente não ativou a descoberta de rede ou o compartilhamento de arquivos e impressoras.
3. **Serviços principais de compartilhamento não estão em execução** — Os serviços Servidor, Estação de Trabalho ou Auxiliar NetBIOS sobre TCP/IP estão parados ou desabilitados.
4. **Firewall ou software de segurança está bloqueando o tráfego** — O Firewall do Windows ou um antivírus de terceiros está bloqueando a porta SMB 445.
5. **Falha de protocolo de rede ou resolução de nomes** — NetBIOS está desabilitado, SMB 1.0/CIFS não está instalado ou a resolução do nome do host falha.
6. **Credenciais de rede desatualizadas** — O cliente possui credenciais incorretas em cache para o PC host.

---

## Solução

### Etapa 1 — Verificar a conectividade de rede

No **PC host**, abra o Prompt de Comando e execute `ipconfig`. Anote o endereço IPv4.

No **PC cliente**, abra o Prompt de Comando e execute:
```
ping <endereço IP do host>
```

![Prompt de Comando — ipconfig e ping para verificar a conectividade](/images/printer-error-0x00000035/step2.png)

Se o ping falhar, verifique o roteador e os cabos de rede e certifique-se de que ambos os dispositivos estejam conectados à mesma rede e sub-rede.

---

### Etapa 2 — Ativar a descoberta de rede e o compartilhamento de arquivos

No host e no cliente, vá para **Painel de Controle → Central de Rede e Compartilhamento → Alterar as configurações de compartilhamento avançadas**. Em **Rede privada**, ative:
- **Ativar a descoberta de rede**
- **Ativar o compartilhamento de arquivos e impressoras**

Salve as configurações e tente novamente conectar à impressora compartilhada.

![Configurações de compartilhamento avançadas — ativando a descoberta de rede e o compartilhamento](/images/printer-error-0x00000035/step3.png)

---

### Etapa 3 — Permitir o compartilhamento de impressoras no firewall

Abra **Firewall do Windows Defender → Permitir um aplicativo ou recurso pelo Firewall do Windows Defender**. Encontre **Compartilhamento de Arquivos e Impressoras** e marque as caixas **Privado** e **Público**.

Se você usa software antivírus de terceiros, desative-o temporariamente para verificar se está bloqueando a conexão.

![Firewall do Windows — permitindo o compartilhamento de arquivos e impressoras](/images/printer-error-0x00000035/step4.png)

---

### Etapa 4 — Corrigir protocolos de rede e configurações de acesso

Abra as **propriedades do adaptador de rede** e certifique-se de que **Cliente para Redes Microsoft** e **Compartilhamento de Arquivos e Impressoras para Redes Microsoft** estejam marcados.

Também abra **Ativar ou desativar recursos do Windows** e habilite **Suporte ao Compartilhamento de Arquivos SMB 1.0/CIFS**, depois reinicie.

![Propriedades do adaptador de rede — habilitando SMB 1.0/CIFS](/images/printer-error-0x00000035/step5.png)

---

### Etapa 5 — Limpar credenciais de rede desatualizadas

Pressione **Win + R**, digite `control keymgr.dll` e abra o **Gerenciador de Credenciais**. Em **Credenciais do Windows**, exclua todas as entradas relacionadas ao endereço IP ou nome do computador do PC host.

![Gerenciador de Credenciais — removendo credenciais desatualizadas](/images/printer-error-0x00000035/step6.png)

---

## Perguntas frequentes

**P: O ping funciona mas o erro 0x00000035 ainda aparece. Por quê?**
Um ping bem-sucedido confirma apenas a conectividade IP básica. O erro 0x00000035 é causado por serviços de compartilhamento, regras de firewall ou problemas de protocolo. Verifique o serviço Servidor, as regras de firewall e o protocolo SMB.

**P: O erro 0x00000035 é o mesmo que o erro 0x80070035?**
Sim. Ambos significam «O caminho de rede não foi encontrado». A única diferença é o formato do código de erro. As causas e correções são idênticas.

**P: Apenas um PC cliente tem o erro — os demais funcionam bem. O que devo fazer?**
O problema é específico daquele PC cliente. Limpe suas credenciais desatualizadas, reinicie seus serviços de compartilhamento locais e ative o protocolo SMB. Nenhuma alteração é necessária no PC host.

**P: Como evitar que o erro volte a aparecer?**
Configure os serviços **Servidor**, **Estação de Trabalho** e **Auxiliar NetBIOS sobre TCP/IP** para iniciar automaticamente. Evite também caracteres especiais no nome de compartilhamento da impressora.

---

## Resumo

O erro 0x00000035 ao conectar uma impressora compartilhada no Windows 10 ou Windows 11 significa que o Windows não consegue localizar o caminho de rede compartilhado. Não está relacionado ao hardware ou driver da impressora. Corrija-o nesta ordem:

1. **Verificar a conectividade de rede** — fazer ping no IP do host; garantir que ambos os dispositivos estejam na mesma sub-rede
2. **Ativar a descoberta de rede e o compartilhamento de arquivos e impressoras**
3. **Permitir o compartilhamento de arquivos e impressoras no Firewall do Windows**
4. **Habilitar SMB 1.0/CIFS** e verificar as configurações de protocolo do adaptador de rede
5. **Limpar credenciais desatualizadas** no Gerenciador de Credenciais

---

## Artigos relacionados

- [Corrigir o erro de impressora compartilhada 0x000006d9 no Windows](/pt/posts/printer-sharing-error-0x000006d9/)
- [Corrigir o erro de impressora compartilhada 0x00000bcb no Windows](/pt/posts/printer-sharing-error-0x00000bcb/)
- [Corrigir o erro de impressora compartilhada 0x00000709 no Windows](/pt/posts/printer-sharing-error-0x00000709/)
