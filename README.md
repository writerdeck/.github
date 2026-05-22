# WriterDeck — O Kindle do Apocalipse

> Um dispositivo dedicado à escrita, livre de distrações, construído para sobreviver ao fim do mundo digital.

---

## Propósito

O **WriterDeck** é um hardware de escrita minimalista projetado para quem leva a sério o ato de escrever. Sem notificações, sem abas abertas, sem redes sociais — apenas você e as palavras.

A ideia central é combinar uma tela de tinta eletrônica (e-ink) com um teclado mecânico e um processador de baixo consumo para criar uma máquina de escrever do século XXI: portátil, com bateria de longa duração e zero distrações. Apelidamos o dispositivo de **"Kindle do Apocalipse"** — ele ainda vai estar funcionando quando tudo o mais tiver apagado.

### Objetivos do projeto

- Proporcionar um ambiente de escrita focado e sem distrações
- Atingir autonomia de bateria superior a vários dias de uso contínuo
- Manter o hardware simples, reparável e de código aberto
- Ser leve e portátil o suficiente para ir a qualquer lugar

---

## 1. Pesquisa do Processador

Esta seção registra a investigação dos candidatos a processador/microcontrolador para o WriterDeck.

### Requisitos

| Critério | Meta |
|---|---|
| Consumo em operação | < 100 mW |
| Consumo em standby | < 1 mW |
| Capacidade de processamento | Suficiente para editor de texto simples |
| Suporte a display e-ink | SPI / I²C nativo |
| Conectividade | Wi-Fi ou USB opcional |
| Custo | Acessível e disponível no mercado |

### Candidatos avaliados

- **Raspberry Pi Zero 2 W** — Linux completo, boa comunidade, consumo moderado (~100 mA @ 5 V)
- **ESP32-S3** — Wi-Fi/BT integrado, baixo custo, bom suporte a e-ink via SPI
- **STM32L4** — Ultra low-power, ideal para standby longo, sem OS
- **RP2040 (Raspberry Pi Pico)** — Simples, barato, grande ecossistema MicroPython/C

### Status

- [ ] Definir candidato principal
- [ ] Medir consumo real em bancada
- [ ] Validar compatibilidade com a tela escolhida

---

## 2. Pesquisa da Tela

Esta seção documenta a busca pelo display ideal — o coração da experiência de escrita do WriterDeck.

### Requisitos

| Critério | Meta |
|---|---|
| Tecnologia | E-ink / e-paper (sem luz de fundo) |
| Resolução | ≥ 800 × 480 px (ideal: 1024 × 758 px) |
| Tamanho | 6″ a 10.3″ |
| Interface | SPI |
| Consumo | Próximo a zero quando estático |
| Atualização parcial | Obrigatório para experiência de digitação fluida |

### Candidatos avaliados

- **Waveshare 7.5" e-Paper** — Boa resolução, SPI, suporte à atualização parcial
- **Good Display GDEY075T7** — 800 × 480, rápido, disponível com drivers abertos
- **Dasung Paperlike** — Monitor e-ink externo; referência de mercado
- **Módulos reaproveitados de Kindle** — Alta qualidade de imagem, mas interface proprietária e difícil de hackear

### Considerações

- Atualização parcial é essencial: a tela precisa atualizar apenas o cursor/linha atual sem piscar a tela inteira
- Contraste e legibilidade sob luz solar direta são diferenciais importantes

### Status

- [ ] Definir tamanho e modelo do painel
- [ ] Testar atualização parcial com o processador escolhido
- [ ] Avaliar carcaça / moldura para o painel

---

## 3. Desenvolvimento do Firmware

Esta seção acompanha o desenvolvimento do software embarcado que dará vida ao WriterDeck.

### Arquitetura planejada

```
┌─────────────────────────────────────┐
│              Aplicação              │
│  Editor de texto minimalista (UTF-8)│
├─────────────────────────────────────┤
│           Camada de HAL             │
│  Driver e-ink · Driver teclado USB  │
├─────────────────────────────────────┤
│          Sistema / RTOS             │
│   Bare-metal / FreeRTOS / Linux     │
└─────────────────────────────────────┘
```

### Funcionalidades planejadas

- **Editor de texto**: navegação por cursor, inserção, deleção e salvamento de arquivos `.txt` / `.md`
- **Gerenciamento de arquivos**: listagem e abertura de documentos no armazenamento interno (SD card ou flash)
- **Exportação**: USB mass storage ou transferência via Wi-Fi (opcional)
- **Configurações**: brilho (se frontlight disponível), velocidade de atualização da tela, fonte e tamanho
- **Modo de baixo consumo**: suspender CPU e tela quando inativo, acordar com qualquer tecla

### Stack técnica (em avaliação)

| Componente | Opção A | Opção B |
|---|---|---|
| Linguagem | C / C++ | MicroPython |
| Build system | CMake + SDK do fabricante | Makefile |
| Editor base | Implementação própria | Port do `kilo` / `micro` |
| Sistema de arquivos | FatFS | LittleFS |

### Roadmap

- [ ] Bootloader e inicialização básica
- [ ] Driver de display e-ink com atualização parcial
- [ ] Driver de teclado (USB HID)
- [ ] Editor de texto funcional (v0.1)
- [ ] Persistência de arquivos
- [ ] Gerenciamento de energia / sleep
- [ ] Interface de configuração

---

## Contribuindo

Este é um projeto aberto. Sinta-se à vontade para abrir issues, propor componentes alternativos ou enviar pull requests com melhorias na documentação e no firmware.

---

## Licença

A definir — a intenção é publicar sob uma licença de hardware e software aberto (ex.: CERN OHL + MIT).