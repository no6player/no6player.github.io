---
title: "Como instalar um driver de impressora no Windows 10/11"
date: 2026-04-26
description: "Guia completo passo a passo para instalar drivers de impressora no Windows 10 e Windows 11 — USB, rede e instalação manual com dicas de solução de problemas."
tags: ["windows", "driver", "instalação", "USB", "impressora de rede"]
categories: ["Instalação"]
cover:
  image: "/images/printer-driver/connection-types.svg"
  alt: "Visão geral dos métodos de conexão de impressora"
  caption: "Duas principais formas de conectar uma impressora: USB e rede/Wi-Fi"
  relative: false
---

Instalar corretamente um driver de impressora é a base de uma configuração de impressão funcional. Este guia apresenta todos os métodos — desde o simples plug-and-play até a instalação totalmente manual — e mostra como resolver os problemas mais comuns.

---

## Antes de começar

Reúna as seguintes informações antes de começar:

| O que você precisa | Onde encontrar |
|---|---|
| Número do modelo da impressora | Etiqueta na parte frontal ou inferior da impressora |
| Versão do Windows (10 ou 11) | Configurações → Sistema → Sobre |
| Tipo de sistema (32 bits / 64 bits) | Configurações → Sistema → Sobre → Tipo de sistema |
| Tipo de conexão (USB ou Wi-Fi) | Decida antes de começar |

---

## Método 1 — USB Plug-and-Play (O mais simples)

Este método funciona para a maioria das impressoras modernas.

**Passos:**

1. Certifique-se de que a impressora está **desligada**.
2. Conecte o cabo USB da impressora a uma porta USB do computador.
3. Ligue a impressora.
4. O Windows detecta automaticamente a impressora e baixa o driver.
5. Aguarde a notificação: *«O dispositivo está pronto»* no canto inferior direito.

> **Dica:** Use um cabo USB com menos de 3 metros. Cabos mais longos podem causar erros de conexão.

---

## Método 2 — Configurações do Windows (USB ou rede)

Use este método se o plug-and-play não funcionou, ou para adicionar uma impressora de rede.

![Configurações do Windows — Impressoras e scanners](/images/printer-driver/windows-settings.svg)

**Passos:**

1. Abra **Iniciar** → **Configurações** (ícone ⚙).
2. Clique em **Bluetooth e dispositivos** → **Impressoras e scanners**.
3. Clique em **Adicionar uma impressora ou scanner** (botão azul no topo).
4. O Windows procura impressoras próximas. Aguarde 20–30 segundos.

![Assistente de adição de impressora — selecione sua impressora da lista](/images/printer-driver/add-printer-wizard.svg)

5. Quando sua impressora aparecer na lista, clique nela.
6. Clique em **Adicionar dispositivo**.
7. O Windows instala o driver automaticamente.

> **Impressora de rede não aparece?** Certifique-se de que a impressora e o computador estejam conectados à **mesma rede Wi-Fi**.

---

## Método 3 — Download manual do driver

Use este método quando o Windows não encontrar o driver automaticamente.

![Download manual do driver — quatro etapas](/images/printer-driver/manual-driver-download.svg)

**Passos:**

1. **Encontre o modelo da sua impressora** — verifique a etiqueta na impressora.
2. **Acesse o site do fabricante:**
   - HP: `support.hp.com`
   - Canon: `canon.com/support`
   - Epson: `epson.com/support`
   - Brother: `support.brother.com`
3. **Pesquise** pelo número exato do modelo.
4. Em **Software e drivers**, selecione seu sistema operacional.
5. Baixe o **driver completo** (recomendado) ou o driver básico.
6. Abra o arquivo `.exe` baixado e siga o assistente de instalação.

---

## Método 4 — Impressora de rede via endereço IP

Use este método quando a impressora de rede não for detectada automaticamente.

**Passos:**

1. Imprima uma **página de configuração** pelo painel de controle da impressora. Ela mostra o endereço IP.
2. Abra **Configurações** → **Bluetooth e dispositivos** → **Impressoras e scanners** → **Adicionar uma impressora ou scanner**.
3. Clique em **«A impressora que desejo não está listada»**.
4. Selecione **«Adicionar uma impressora usando um endereço TCP/IP ou nome do host»** → **Avançar**.
5. Insira o endereço IP da impressora (ex: `192.168.1.105`).
6. Clique em **Avançar** — o Windows se conecta e instala o driver.

---

## Solução de problemas

![Problemas comuns e soluções](/images/printer-driver/troubleshooting.svg)

### Impressora não encontrada durante a instalação

- Verifique se a impressora está **ligada** (não em modo de economia de energia).
- Para USB: tente uma porta USB diferente; evite hubs USB.
- Para rede: confirme que ambos os dispositivos estão na mesma rede Wi-Fi.
- Desative temporariamente o firewall e tente novamente.

### Impressora aparece como «Offline»

1. Pressione `Win + R`, digite `services.msc` e pressione **Enter**.
2. Role até **Spooler de Impressão**.
3. Clique com o botão direito → **Reiniciar**.
4. Abra **Dispositivos e Impressoras**, clique com o botão direito na impressora → **Ver o que está sendo impresso**.
5. No menu, clique em **Impressora** e desmarque **Usar impressora offline**.

### Falha na instalação do driver

- Clique com o botão direito no instalador → **Executar como administrador**.
- Desative temporariamente o antivírus e tente novamente.
- Verifique se baixou o driver correto (64 bits ou 32 bits).

---

## Resumo

| Método | Ideal para | Fonte do driver |
|---|---|---|
| USB Plug-and-Play | Impressoras novas, configuração rápida | Windows Update |
| Configurações do Windows | Impressora USB ou de rede | Windows Update |
| Download manual | Recursos completos, falha na instalação automática | Site do fabricante |
| Endereço IP | Impressoras de rede não detectadas automaticamente | Windows / Fabricante |

Após a instalação, sempre imprima uma **página de teste**: clique com o botão direito na impressora → **Propriedades da impressora** → **Imprimir página de teste**.
