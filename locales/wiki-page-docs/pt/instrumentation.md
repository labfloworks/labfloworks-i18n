---
title: Instrumentação VISA/SCPI
description: Guia de ligação, configuração e utilização de hardware real e simulado no FloWorks através do padrão VISA/SCPI.
---

# 🔌 Instrumentação VISA/SCPI

O FloWorks integra comunicação direta com instrumentos de laboratório reais através do protocolo **SCPI** (Standard Commands for Programmable Instruments) sobre a camada de abstração **VISA** (Virtual Instrument Software Architecture). Além disso, oferece simuladores puramente em Python para desenvolver, testar e partilhar fluxos sem necessidade de hardware físico.

---

## 🌐 O que é VISA/SCPI?

| Tecnologia | Descrição |
|------------|-------------|
| **VISA** | Camada padrão que abstrai a interface física (USB-TMC, Ethernet/LAN, GPIB, RS‑232). Permite mudar de um instrumento real para um simulado apenas modificando a cadeia de ligação. |
| **SCPI** | Linguagem de comandos ASCII estandardizada para controlar geradores, osciloscópios, multímetros, LCR meters, etc. Os fabricantes estendem o padrão, mas a base é universal. |
| **PyVISA** | Backend Python utilizado pelo FloWorks. Suporta `@py` (simulação pura) e backends nativos (`@ni`, `@ivi`, `@keysight`, etc.). |

---

## ⚙️ Configuração Típica

=== "📍 Cadeia de Ligação (Resource String)"
    Formato padrão VISA:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (Osciloscópio USB)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (Porta série RS-232)
    - `GPIB0::1::INSTR` (GPIB legado)

=== "⏱️ Tempo de Espera e Opções**"
    - **Timeout**: Configurável em ms. Aumenta se o instrumento requer medições longas ou varreduras de frequência.
    - **Inicialização**: Alguns nós permitem injetar comandos SCPI personalizados ao ligar (ex. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Nós de Hardware Disponíveis

<div class="grid cards" markdown>

- **🔭 Osciloscópio SCPI**  
  Captura formas de onda no domínio temporal. Suporta multicanal, escalagem automática, trigger hardware e menu "Mostrar canal" para alternar sinais em tempo real.

- **⚡ LCR Meter**  
  Mede impedância, indutância, capacitância, resistência e fator de perda. Retorna um `master_payload` com dados primários e secundários numa única aquisição.

- **🎛️ Gerador de Funções Arbitrárias**  
  Envia sinais para hardware SDG ou simula saídas. Configura modulação (AM/FM/PM), varredura linear/logarítmica, burst e fase.

- **📊 Multímetro Digital (DMM)** *(Em expansão)*  
  Interface SCPI para medições de tensão DC/AC, corrente, resistência e frequência. Compatível com Keithley, Agilent e Rigol.

- **🔋 Fonte de Alimentação Programável** *(Em expansão)*  
  Controlo de tensão/corrente de saída com proteção OVP/OCP. Útil para bancos de testes automatizados.

</div>

---

## 🔄 Fluxo de Trabalho Típico

1. **Adicionar o nó** ao canvas a partir da toolbar (`Fontes` ou `Instrumentos`).
2. **Configurar a ligação**: Seleciona backend, introduz a cadeia VISA e ajusta timeout/inicialização.
3. **Ligar ao fluxo**: Une a saída do instrumento a nós de processamento (FFT, filtros, aritmética) ou visualização.
4. **Executar (`F5`)**: O motor topológico solicita a aquisição, o driver faz parse da resposta SCPI e empacota os dados.
5. **Visualizar/Exportar**: Os dados fluem pelo grafo para serem processados pelos nós seguintes. 

---

## 🛠️ Solução de Problemas

!!! warning "1. VISA não encontra o instrumento (`VI_ERROR_RSRC_NFOUND`)"
    - **Causa:** Cadeia incorreta, cabo desconectado ou backend não deteta o dispositivo.
    - **Solução:** Executa `pyvisa-shell` ou a utilidade do fabricante (NI MAX, Keysight Connection Expert) para listar recursos válidos. Verifica permissões de utilizador.

!!! warning "2. Timeout durante a aquisição"
    - **Causa:** Varredura lenta, trigger não se cumpre ou instrumento ocupado noutra tarefa.
    - **Solução:** Aumenta o timeout no nó. Verifica que o trigger do osciloscópio está configurado corretamente (`AUTO` ou `NORMAL`). Usa `*CLS` no início.

!!! warning "3. Simulação não responde ou falha"
    - **Causa:** `PyVISA-py` não está instalado ou há conflito com outro backend.
    - **Solução:** `pip install pyvisa-py`. No nó, seleciona explicitamente `@py` como backend.

!!! warning "4. Erros SCPI (`Command Error`, `Execution Error`)"
    - **Causa:** Comando não suportado pelo firmware ou sintaxe incorreta.
    - **Solução:** Consulta o manual de programação SCPI do teu instrumento. Alguns fabricantes requerem prefixos `:` ou terminadores `
`. O FloWorks adiciona `
` automaticamente, mas podes ajustar o terminador no driver.

!!! info "5. Criar um nó para um instrumento não suportado"
    - Herda de `BaseNode` e usa o padrão `DeviceBase` em `instrument/`.
    - Implementa um driver `headless` que retorne tuplos `(x, y)` ou `master_payload`.
    - Segue o [📘 Guia: Adicionar um Novo Nó](adding-a-new-node.md) para registar portos, serialização e i18n.

---

## 📚 Recursos Relacionados

- [🧩 Referência Técnica de Nós](node-reference.md) → Detalhes de `oscilloscope_node`, `generator_node` e contratos de serialização.
- [📦 Guia de Executável Portable](guia-ejecutable-portable.md) → Gestão de firewall, `resource_path()` e empacotamento PyInstaller.
- [📘 Adicionar um Novo Nó](adding-a-new-node.md) → Como estender `instrument/` e registar drivers personalizados.
- [📄 Formato `.sflow`](sflow-format.md) → Como persistem as configurações de hardware e os arrays capturados.
