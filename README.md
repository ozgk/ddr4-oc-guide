# 🧠 Guia Definitivo de Overclock de Memória DDR4

> Este guia foi escrito com o objetivo de ser o mais completo possível, cobrindo desde conceitos básicos até técnicas avançadas de overclock de RAM DDR4.

---

## 📑 Índice

1. [Introdução — Por que fazer OC de RAM?](#1--introdução--por-que-fazer-oc-de-ram)
2. [Conceitos Fundamentais](#2--conceitos-fundamentais)
   - [Frequência vs. Timings](#21-frequência-vs-timings)
   - [MHz vs. MT/s — A Diferença Real](#22-mhz-vs-mts--a-diferença-real)
   - [Como Calcular Latência Real (ns)](#23-como-calcular-latência-real-ns)
   - [Timings Primários, Secundários e Terciários](#24-timings-primários-secundários-e-terciários)
3. [Os Chips (ICs) de Memória DDR4](#3--os-chips-ics-de-memória-ddr4)
   - [Notação Abreviada dos ICs](#31-notação-abreviada-dos-ics)
   - [Samsung B-Die (S8B) — O Rei do OC](#32-samsung-b-die-s8b--o-rei-do-oc)
   - [Samsung D-Die (S8D)](#33-samsung-d-die-s8d)
   - [Hynix DJR / D-Die (H8D)](#34-hynix-djr--d-die-h8d)
   - [Hynix CJR / C-Die (H8C)](#35-hynix-cjr--c-die-h8c)
   - [Hynix AFR / A-Die (H8A)](#36-hynix-afr--a-die-h8a)
   - [Micron Rev. E (M8E)](#37-micron-rev-e-m8e)
   - [Micron Rev. B (M8B / M16B)](#38-micron-rev-b-m8b--m16b)
   - [Nanya B-Die (N8B)](#39-nanya-b-die-n8b)
   - [Samsung E-Die (S4E)](#310-samsung-e-die-s4e)
   - [Tabela Comparativa Completa dos ICs](#311-tabela-comparativa-completa-dos-ics)
   - [Ranking dos ICs — Do Melhor ao Pior](#312-ranking-dos-ics--do-melhor-ao-pior)
4. [Como Identificar o IC da Sua RAM](#4--como-identificar-o-ic-da-sua-ram)
   - [Via Software (Thaiphoon Burner)](#41-via-software-thaiphoon-burner)
   - [Via Label Física — Corsair](#42-via-label-física--corsair)
   - [Via Label Física — G.Skill](#43-via-label-física--gskill)
   - [Via Label Física — Kingston](#44-via-label-física--kingston)
5. [Voltage Scaling — Como Cada IC Responde a Voltagem](#5--voltage-scaling--como-cada-ic-responde-a-voltagem)
6. [Voltagem Máxima Recomendada para Uso Diário](#6--voltagem-máxima-recomendada-para-uso-diário)
7. [Binning — O que é e Por que Importa](#7--binning--o-que-é-e-por-que-importa)
8. [Ranks e Densidade — Single Rank vs. Dual Rank](#8--ranks-e-densidade--single-rank-vs-dual-rank)
9. [Influência da Placa-Mãe (Daisy Chain vs. T-Topology)](#9--influência-da-placa-mãe-daisy-chain-vs-t-topology)
10. [O Controlador de Memória Integrado (IMC)](#10--o-controlador-de-memória-integrado-imc)
    - [IMC Intel](#101-imc-intel)
    - [IMC AMD (Infinity Fabric / FCLK)](#102-imc-amd-infinity-fabric--fclk)
11. [Temperaturas e Seu Efeito na Estabilidade](#11--temperaturas-e-seu-efeito-na-estabilidade)
12. [Passo a Passo: Como Fazer OC de RAM DDR4](#12--passo-a-passo-como-fazer-oc-de-ram-ddr4)
    - [Preparação](#121-preparação)
    - [Encontrando um Baseline](#122-encontrando-um-baseline)
    - [Ajustando Timings Primários](#123-ajustando-timings-primários)
    - [Ajustando Timings Secundários](#124-ajustando-timings-secundários)
    - [Ajustando Timings Terciários](#125-ajustando-timings-terciários)
    - [Dicas Específicas para Intel](#126-dicas-específicas-para-intel)
    - [Dicas Específicas para AMD](#127-dicas-específicas-para-amd)
13. [Guia de Cada Timing Explicado](#13--guia-de-cada-timing-explicado)
14. [Softwares de Teste de Estabilidade](#14--softwares-de-teste-de-estabilidade)
15. [Softwares de Benchmark](#15--softwares-de-benchmark)
16. [Softwares para Visualizar Timings](#16--softwares-para-visualizar-timings)
17. [Links Úteis e Referências](#17--links-úteis-e-referências)

---


> [!IMPORTANT]
> Em plataformas AMD Ryzen, o overclock de RAM é **ainda mais impactante** porque a frequência da memória está diretamente ligada ao Infinity Fabric Clock (FCLK), que conecta os CCX/CCD do processador.

---

## 2. 📐 Conceitos Fundamentais

### 2.1 Frequência vs. Timings

A performance da memória é determinada por **dois fatores principais**:

| Fator | O que é | Melhor quando... |
|-------|---------|-----------------|
| **Frequência** | Número de ciclos por segundo (MHz/MT/s) | **Maior** |
| **Timings** | Número de ciclos de clock necessários para uma operação | **Menor** (exceto tREFI) |

A relação entre eles é crucial: **não adianta ter frequência altíssima se os timings estão frouxos demais**, e vice-versa. O objetivo do OC de RAM é encontrar o equilíbrio ideal entre ambos.

> [!TIP]
> **Regra geral:** DDR4-3000 CL15 e DDR4-3200 CL16 têm **a mesma latência real**, pois a frequência mais alta do 3200 compensa o CL mais alto.

### 2.2 MHz vs. MT/s — A Diferença Real

Quando alguém diz "minha RAM roda a 3200 MHz", tecnicamente está **errado**. O correto seria 3200 **MT/s** (MegaTransfers por segundo).

Como o DDR (Double Data Rate) transfere dados em **ambas as bordas do clock** (subida e descida), a frequência real é **metade** do número de transferências:

```
DDR4-3200 = 3200 MT/s = 1600 MHz de frequência real
DDR4-3600 = 3600 MT/s = 1800 MHz de frequência real
DDR4-4000 = 4000 MT/s = 2000 MHz de frequência real
```

> Na prática, todo mundo fala "3200 MHz" e é amplamente aceito. Mas é bom saber a diferença técnica.

### 2.3 Como Calcular Latência Real (ns)

Para converter um timing de clock cycles para **nanossegundos reais**, use esta fórmula:

```
Latência (ns) = 2000 × timing / frequência_DDR
```

**Exemplos práticos:**

| Configuração | Cálculo | Latência Real |
|-------------|---------|---------------|
| DDR4-3000 CL15 | 2000 × 15 / 3000 | **10.00 ns** |
| DDR4-3200 CL16 | 2000 × 16 / 3200 | **10.00 ns** |
| DDR4-3600 CL16 | 2000 × 16 / 3600 | **8.89 ns** |
| DDR4-3600 CL14 | 2000 × 14 / 3600 | **7.78 ns** |
| DDR4-4000 CL16 | 2000 × 16 / 4000 | **8.00 ns** |
| DDR4-3200 CL14 | 2000 × 14 / 3200 | **8.75 ns** |

> [!TIP]
> Use essa fórmula para comparar diferentes kits. O que tiver **menor latência em ns** é o melhor para performance.

### 2.4 Timings Primários, Secundários e Terciários

Os timings da RAM são divididos em **3 categorias**:

#### Timings Primários (P)
Os mais impactantes na performance. São os famosos números que você vê na especificação do kit (ex: 16-18-18-36).

| Timing | Nome Completo | O que faz |
|--------|---------------|-----------|
| **tCL** | CAS Latency | Tempo entre enviar comando de leitura e receber dados |
| **tRCD** | RAS to CAS Delay | Tempo para acessar uma linha na DRAM |
| **tRP** | Row Precharge | Tempo para fechar uma linha antes de abrir outra |
| **tRAS** | Row Active Time | Tempo mínimo que uma linha fica ativa |

#### Timings Secundários (S)
Afetam latência e bandwidth, mas com menos impacto individual que os primários.

| Timing | O que faz |
|--------|-----------|
| **tRRD_S** | Row-to-Row Delay (grupos de banco diferentes) |
| **tRRD_L** | Row-to-Row Delay (mesmo grupo de banco) |
| **tFAW** | Four Activate Window |
| **tWR** | Write Recovery Time |
| **tWTR_S/L** | Write to Read Delay |
| **tRFC** | Refresh Cycle Time — **MUITO importante** |
| **tCWL** | CAS Write Latency |

#### Timings Terciários (T)
Afetam principalmente bandwidth.

| Timing | O que faz |
|--------|-----------|
| **tREFI** | Refresh Interval — **Exceção:** único timing que você quer **ALTO** |
| **tRDRD** | Read to Read delays |
| **tWRWR** | Write to Write delays |
| **tRDWR** | Read to Write delays |
| **tWRRD** | Write to Read delays |

> [!NOTE]
> **tREFI** é a exceção à regra de "menor = melhor". O tREFI define o intervalo entre refreshes — enquanto a RAM faz refresh, ela **não pode fazer nada**. Logo, quanto **maior** o tREFI, menos tempo a RAM gasta fazendo refresh e mais tempo fica disponível para operações úteis.

---

## 3. 🔬 Os Chips (ICs) de Memória DDR4

O **IC (Integrated Circuit)**, também chamado de **die**, é o chip de memória real que está soldado no PCB do seu módulo de RAM. Ele é **o fator mais determinante** do potencial de overclock da sua memória.

> [!IMPORTANT]
> Dois kits de RAM com a mesma especificação na caixa (mesma frequência, mesmos timings, mesma marca) podem ter ICs completamente diferentes e, portanto, potenciais de OC completamente diferentes.

### 3.1 Notação Abreviada dos ICs

Para facilitar a comunicação na comunidade, é usada uma notação abreviada:

**Formato: XYZ**
- **X** = Primeira letra do fabricante (**S**amsung, **H**ynix, **M**icron, **N**anya)
- **Y** = Densidade em Gb (**8** para 8 Gb, **16** para 16 Gb, **4** para 4 Gb)
- **Z** = Revisão do die (letra)

**Exemplos:**
| Abreviação | IC Completo |
|-----------|-------------|
| **S8B** | Samsung 8 Gb B-die |
| **H8D** | Hynix 8 Gb D-die (DJR) |
| **M8E** | Micron 8 Gb Rev. E |
| **M16B** | Micron 16 Gb Rev. B |
| **H8C** | Hynix 8 Gb C-die (CJR) |

---

### 3.2 Samsung B-Die (S8B) — O Rei do OC

> **O IC mais famoso e desejado para overclock de DDR4 de todos os tempos.**

O Samsung B-Die é lendário na comunidade de overclock por um motivo simples: **todos os seus timings principais escalam com voltagem**. Isso é extremamente raro.

**Características:**
- ✅ tCL escala com voltagem — **SIM**
- ✅ tRCD escala com voltagem — **SIM** (único IC mainstream que faz isso!)
- ✅ tRP escala com voltagem — **SIM** (único IC mainstream que faz isso!)
- ✅ tRFC escala com voltagem — **SIM**
- 🏆 Frequência máxima esperada: **5000+ MT/s**
- 🌡️ **Sensível a temperatura** — Precisa de boa refrigeração

**Por que é tão bom?**

Em todos os outros ICs, tRCD e tRP simplesmente **não respondem** a mais voltagem. Você pode jogar 1.5V, 1.6V — não vai baixar. No B-Die, mais voltagem = timings mais apertados em **tudo**. Isso permite configurações extremamente apertadas como:

```
DDR4-3200 CL14-14-14-28 @ 1.35V (bin padrão bom)
DDR4-3600 CL14-14-14-28 @ 1.45V (OC excelente)
DDR4-4000 CL16-16-16-32 @ 1.45V (OC frequência alta)
DDR4-3600 CL12-12-12-28 @ 1.50V (extreme daily)
```

**Kits conhecidos com B-Die:**
- G.Skill Trident Z / Royal DDR4-3200 CL14
- G.Skill Trident Z DDR4-3600 CL16-16-16 @ 1.35V
- G.Skill Trident Z DDR4-4000+ CL17-17-17
- Corsair Vengeance LPX versão 4.31/4.32
- Patriot Viper Steel DDR4-4400

> [!WARNING]
> B-Die de **bin baixo** (ex: DDR4-2400 CL15) é **significativamente pior** que B-Die de bin alto (DDR4-3200 CL14). Não espere os mesmos resultados. O binning importa muito — veja a seção de [Binning](#7--binning--o-que-é-e-por-que-importa).

---

### 3.3 Samsung D-Die (S8D)

Uma revisão mais recente da Samsung, produzida em nó litográfico mais avançado.

**Características:**
- ✅ tCL escala com voltagem — **SIM**
- ❌ tRCD escala com voltagem — **NÃO**
- ❌ tRP escala com voltagem — **NÃO**
- ❌ tRFC escala com voltagem — **NÃO**
- 🏆 Frequência máxima esperada: **5000+ MT/s**

Apesar de não ter o voltage scaling universal do B-Die, o S8D compensa com excelente capacidade de frequência alta. É um IC moderno e eficiente.

---

### 3.4 Hynix DJR / D-Die (H8D)

O **melhor IC da Hynix** para DDR4 e uma excelente alternativa ao B-Die.

**Características:**
- ✅ tCL escala com voltagem — **SIM**
- ❌ tRCD escala com voltagem — **NÃO**
- ❌ tRP escala com voltagem — **NÃO**
- ✅ tRFC escala com voltagem — **SIM**
- 🏆 Frequência máxima esperada: **5000+ MT/s**

**Por que é bom?**

O DJR atinge frequências altíssimas com timings razoáveis. É frequentemente encontrado em kits de alta frequência (4000+ MHz) a preços mais acessíveis que B-Die.

**Kits conhecidos com DJR:**
- G.Skill Trident Z Neo DDR4-3600 CL16-19-19
- Patriot Viper Steel DDR4-4000+
- TeamGroup T-Force Xtreem ARGB DDR4-4000+

**Pontos fortes vs. B-Die:**
- 👍 Preço mais acessível
- 👍 Frequências muito altas (4400+ facilmente)
- 👍 tRFC escala com voltagem
- 👎 tRCD e tRP não escalam — ficam "travados" em valores mais altos

---

### 3.5 Hynix CJR / C-Die (H8C)

O IC "intermediário" da Hynix. Muito popular por ser barato e decente.

**Características:**
- ✅ tCL escala com voltagem — **SIM**
- ❌ tRCD escala com voltagem — **NÃO**
- ❌ tRP escala com voltagem — **NÃO**
- ✅ tRFC escala com voltagem — **SIM**
- 🏆 Frequência máxima esperada: **~4133 MT/s** (inconsistente)

**Observação importante:** O CJR é **inconsistente** entre amostras. Em testes com 3 sticks RipJaws V 3600 CL19 8GB, um ficou travado em DDR4-3600, outro chegou a DDR4-3800, e o último fez DDR4-4000, todos com CL16 a 1.45V. É uma loteria.

**Kits conhecidos com CJR:**
- G.Skill Ripjaws V DDR4-3600 CL18/19
- Crucial Ballistix Sport (modelos mais antigos)
- HyperX Fury DDR4-3200 CL16

**Pontos fortes:**
- 👍 Preço muito bom
- 👍 Funciona bem para DDR4-3600 CL16 na maioria dos casos
- 👎 Teto de frequência limitado comparado aos ICs top

---

### 3.6 Hynix AFR / A-Die (H8A)

O IC mais básico da Hynix. Encontrado em kits baratos.

**Características:**
- ✅ tCL escala com voltagem — **SIM**
- ❌ tRCD escala com voltagem — **NÃO**
- ❌ tRP escala com voltagem — **NÃO**
- ❓ tRFC — **Não confirmado**
- 🏆 Frequência máxima esperada: **~3600 MT/s**

> [!CAUTION]
> Este é um IC de baixo desempenho para OC. Se o seu kit tem Hynix AFR, não espere milagres. Foque em apertar timings dentro da frequência stock ao invés de tentar subir frequência.

---

### 3.7 Micron Rev. E (M8E)

Um IC **excelente** da Micron, frequentemente encontrado nos populares kits **Crucial Ballistix**.

**Características:**
- ✅ tCL escala com voltagem — **SIM**
- ❌ tRCD escala com voltagem — **NÃO**
- ❌ tRP escala com voltagem — **NÃO**
- ❌ tRFC escala com voltagem — **NÃO**
- 🏆 Frequência máxima esperada: **5000+ MT/s**
- ✅ tCL tem scaling linear **perfeito** com voltagem

**Por que é popular?**

Os kits Crucial Ballistix com Micron Rev. E oferecem uma relação custo-benefício **insana**. É possível comprar um kit DDR4-3000 CL15 ou DDR4-3200 CL16 barato e overclockar para DDR4-3600 CL16 ou até DDR4-3800 CL16 com facilidade.

**Kits conhecidos com Micron Rev. E:**
- Crucial Ballistix DDR4-3000 CL15 (BL2K8G30C15U4B)
- Crucial Ballistix DDR4-3200 CL16 (BL2K8G32C16U4B)
- Crucial Ballistix DDR4-3600 CL16 (BL2K8G36C16U4B)
- Crucial Ballistix MAX DDR4-4000+

> [!WARNING]
> ICs Micron **mais antigos** (antes do Rev. E) são conhecidos por **escalar negativamente** com voltagem. Ou seja, ficam **instáveis** quando você **aumenta** a voltagem acima de ~1.35V, mesmo mantendo a mesma frequência e timings. O Rev. E **não** tem esse problema.

---

### 3.8 Micron Rev. B (M8B / M16B)

Existem **duas versões** do Micron Rev. B e elas são **muito diferentes**:

| Versão | Densidade | Freq. Máx Esperada | Qualidade |
|--------|-----------|--------------------|-----------| 
| **M8B** (8 Gb) | 8 Gb | ~3600 MT/s | ❌ Fraco |
| **M16B** (16 Gb) | 16 Gb | 5000+ MT/s | ✅ Excelente |

> [!IMPORTANT]
> **Não confunda M8B com M16B!** O Micron Rev. B de 16 Gb (M16B) é um IC **moderno e excelente**, enquanto o de 8 Gb (M8B) é um IC antigo e limitado. O M16B é vendido em sticks de 16 GB e até 8 GB (com SPD modificado, encontrado em kits Crucial de alta performance como BLM2K8G51C19U4B).

---

### 3.9 Nanya B-Die (N8B)

Um IC menos comum, fabricado pela Nanya.

**Características:**
- ✅ tCL escala com voltagem — **SIM**
- ❌ tRCD escala com voltagem — **NÃO**
- ❌ tRP escala com voltagem — **NÃO**
- ❌ tRFC escala com voltagem — **NÃO**
- 🏆 Frequência máxima esperada: **4000+ MT/s**

Não é tão popular e raramente é encontrado em kits de alto desempenho, mas é um IC decente para OC moderado.

---

### 3.10 Samsung E-Die (S4E)

Um IC Samsung de 4 Gb, mais antigo.

**Características:**
- ✅ tCL escala com voltagem — **SIM**
- ❌ tRCD escala com voltagem — **NÃO**
- ❌ tRP escala com voltagem — **NÃO**
- ❌ tRFC escala com voltagem — **NÃO**
- 🏆 Frequência máxima esperada: **4000+ MT/s**

> [!NOTE]
> **Cuidado com a nomenclatura!** Quando alguém diz "E-die" sem especificar o fabricante, geralmente se refere a **Samsung 4 Gb E-die**. Mas muitas pessoas também chamam **Micron Rev. E** de "E-die", o que causa confusão. Sempre especifique o fabricante: "Samsung E-die" ou "Micron Rev. E".

---

### 3.11 Tabela Comparativa Completa dos ICs

| IC | Fabricante | Densidade | tCL Scaling | tRCD Scaling | tRP Scaling | tRFC Scaling | Freq. Máx (MT/s) |
|:---:|:---------:|:---------:|:----------:|:-----------:|:----------:|:-----------:|:----------------:|
| **S8B** | Samsung | 8 Gb | ✅ | ✅ | ✅ | ✅ | 5000+ |
| **S8D** | Samsung | 8 Gb | ✅ | ❌ | ❌ | ❌ | 5000+ |
| **H8D** | Hynix | 8 Gb | ✅ | ❌ | ❌ | ✅ | 5000+ |
| **M8E** | Micron | 8 Gb | ✅ | ❌ | ❌ | ❌ | 5000+ |
| **M16B** | Micron | 16 Gb | ✅ | ❌ | ❌ | ❌ | 5000+ |
| **N8B** | Nanya | 8 Gb | ✅ | ❌ | ❌ | ❌ | 4000+ |
| **S4E** | Samsung | 4 Gb | ✅ | ❌ | ❌ | ❌ | 4000+ |
| **H8C** | Hynix | 8 Gb | ✅ | ❌ | ❌ | ✅ | ~4133 |
| **H8A** | Hynix | 8 Gb | ✅ | ❌ | ❌ | ❓ | ~3600 |
| **M8B** | Micron | 8 Gb | ✅ | ❌ | ❌ | ❌ | ~3600 |

---

### 3.12 Ranking dos ICs — Do Melhor ao Pior

```
🥇 Tier S — Elite (Melhor para OC extremo)
   ├── Samsung B-Die (S8B) — O melhor IC DDR4 já feito. Scaling universal.
   
🥈 Tier A — Excelentes
   ├── Hynix DJR (H8D) — Frequências altíssimas, ótimo custo-benefício
   ├── Samsung D-Die (S8D) — Moderno e eficiente, frequências altas
   ├── Micron Rev. E (M8E) — Melhor custo-benefício, Crucial Ballistix
   └── Micron 16 Gb Rev. B (M16B) — IC moderno de 16 Gb, excelente

🥉 Tier B — Bons
   ├── Nanya B-Die (N8B) — Decente, 4000+ MHz
   └── Samsung E-Die (S4E) — 4 Gb, 4000+ MHz

🏅 Tier C — Medianos
   └── Hynix CJR (H8C) — Inconsistente, loteria de silicon

❌ Tier D — Limitados
   ├── Hynix AFR (H8A) — Teto de ~3600 MT/s
   └── Micron 8 Gb Rev. B (M8B) — Teto de ~3600 MT/s, antigo
```

---

## 4. 🔍 Como Identificar o IC da Sua RAM

### 4.1 Via Software (Thaiphoon Burner)

A maneira mais fácil de identificar o IC sem abrir o computador:

1. Baixe o [Thaiphoon Burner](http://www.softnology.biz/files.html)
2. Execute como Administrador
3. Clique em **"Read"** → Selecione o módulo DIMM
4. Procure por **"DRAM Components"** — ali estará o nome do IC

> [!NOTE]
> O Thaiphoon Burner lê o SPD (Serial Presence Detect) do módulo. Nem sempre é 100% preciso, especialmente com kits mais baratos que podem ter SPD genérico. A label física é mais confiável.

### 4.2 Via Label Física — Corsair

A Corsair usa um **número de versão de 3 dígitos** na etiqueta do módulo:

**Formato: `Ver X.YZ`**

| Posição | Significado | Valores |
|---------|-------------|---------|
| **X** (1º dígito) | Fabricante | 3 = Micron, 4 = Samsung, 5 = Hynix, 8 = Nanya |
| **Y** (2º dígito) | Densidade | 1 = 2Gb, 2 = 4Gb, 3 = 8Gb, 4 = 16Gb |
| **Z** (3º dígito) | Revisão | Letra da revisão do die |

**Exemplos:**
| Versão | IC |
|--------|----|
| **Ver 4.31** | Samsung 8 Gb **A**-die |
| **Ver 4.32** | Samsung 8 Gb **B-die** ← o cobiçado |
| **Ver 5.32** | Hynix 8 Gb **B**-die |
| **Ver 3.31** | Micron 8 Gb **A**-die |
| **Ver 5.39** | Hynix 8 Gb revisão 9 |

> [!TIP]
> Se você está comprando Corsair e quer B-Die, procure por **Ver 4.32** (Samsung 8 Gb B-die). Mas atenção: a Corsair pode mudar o IC entre lotes do mesmo produto sem aviso!

### 4.3 Via Label Física — G.Skill

A G.Skill usa um **código 042** na etiqueta:

**Formato: `04213X_Y_Z_`** (simplificado)

Exemplo completo: `04213X`**8**`8`**1**`0B`

| Posição | Significado | Valores |
|---------|-------------|---------|
| Caractere em negrito 1 | Densidade | 4 = 4Gb, **8** = 8Gb, S = 16Gb |
| Caractere em negrito 2 | Fabricante | **1** = Samsung, 2 = Hynix, 3 = Micron, 4 = PSC, 5 = Nanya, 9 = JHICC |
| Último caractere | Revisão | Letra da revisão |

**Exemplo decodificado:**
- `04213X8810B` → Densidade **8** Gb, Fabricante **1** (Samsung), Revisão **B** → **Samsung 8 Gb B-die** ✅

### 4.4 Via Label Física — Kingston

**Formato: `DPM_X_YZ_AMMYY`** (simplificado)

Exemplo: `DPM`**M**`16`**A**`1823`

| Posição | Significado | Valores |
|---------|-------------|---------|
| Letra em negrito | Fabricante | **H** = Hynix, **M** = Micron, **S** = Samsung |
| Próximos 2 dígitos | Ranks | 08 = Single Rank, 16 = Dual Rank |
| Letra seguinte | Mês de produção | 1-9 = Jan-Set, A = Out, B = Nov, C = Dez |
| Últimos 2 dígitos | Ano de produção | Ex: 23 = 2023 |

**Exemplo decodificado:**
- `DPMM16A1823` → Fabricante **M**icron, **16** = Dual Rank, **A** = Outubro, **18**23 = fabricado em 2018

---

## 5. ⚡ Voltage Scaling — Como Cada IC Responde a Voltagem

**Voltage scaling** é como o IC responde quando você aumenta a voltagem VDIMM. Entender isso é **fundamental** para saber o que esperar do seu OC.

### O Conceito

- Se um timing **escala com voltagem**, mais voltagem permite **apertar** (diminuir) esse timing, ou rodar o mesmo timing em **frequência mais alta**.
- Se um timing **não escala com voltagem**, ele simplesmente **não vai baixar** não importa quanta voltagem você coloque. Para rodar frequência mais alta, você terá que **afrouxar** esse timing.

### Scaling do tCL

O tCL é o timing que **escala com voltagem em praticamente todos os ICs**. A relação é quase linear em muitos casos:

**Exemplo com B-Die:**
```
DDR4-3200 CL14 @ 1.35V
   ↓ (+0.05V)
DDR4-3333 CL14 @ 1.40V
   ↓ (+0.05V)
DDR4-3533 CL14 @ 1.45V
   ↓ (+0.05V)
DDR4-3733 CL14 @ 1.50V
```

### Scaling do tRFC (Refresh Cycle Time)

O tRFC escala com voltagem em **B-Die** e **Hynix CJR/DJR**, mas **NÃO** escala na maioria dos ICs Micron, Nanya e Samsung D-Die. No B-Die, tRFC pode cair de ~300 para ~250 ou menos com mais voltagem.

### Tabela Resumida de Voltage Scaling

| IC | tCL | tRCD | tRP | tRFC |
|:--:|:---:|:----:|:---:|:----:|
| **S8B** (Samsung B-Die) | ✅ Linear | ✅ | ✅ | ✅ |
| **H8C, H8D** (Hynix CJR/DJR) | ✅ | ❌ | ❌ | ✅ |
| **H8A** (Hynix AFR) | ✅ | ❌ | ❌ | ❓ |
| **M8B, M8E, M16B** (Micron) | ✅ Linear | ❌ | ❌ | ❌ |
| **N8B** (Nanya) | ✅ | ❌ | ❌ | ❌ |
| **S4E, S8D** (Samsung) | ✅ | ❌ | ❌ | ❌ |

> [!IMPORTANT]
> **Implicação prática:** Se o tRCD do seu IC **não escala** com voltagem e você quer subir a frequência, **obrigatoriamente** terá que afrouxar o tRCD. Isso significa que, diferente do B-Die, a maioria dos ICs terá timings do tipo `CL16-20-20` em frequências altas, com tCL apertado mas tRCD/tRP soltos.

---

## 6. 🔋 Voltagem Máxima Recomendada para Uso Diário

> [!CAUTION]
> **Voltagem excessiva pode danificar permanentemente seus módulos de RAM!** Siga as recomendações abaixo.

A especificação JEDEC (JESD79-4B, p.174) define que o **máximo absoluto** para DDR4 é **1.50V**:

> *"Stresses greater than those listed under 'Absolute Maximum Ratings' may cause permanent damage to the device."*

Porém, esse é o máximo do **padrão geral**. Na prática, cada IC tem um limite diferente:

### Voltagens Recomendadas por IC

| IC | Voltagem Máxima Diária Recomendada | Notas |
|:--:|:----------------------------------:|:-----:|
| **Samsung B-Die** | **1.45-1.50V** | Aguenta bem voltagem alta, mas esquenta |
| **Hynix DJR** | **1.45-1.50V** | Tolerante a voltagem |
| **Samsung D-Die** | **1.40-1.45V** | |
| **Micron Rev. E** | **1.40-1.45V** | Não escala negativamente como ICs Micron antigos |
| **Micron Rev. B (16Gb)** | **1.40-1.45V** | |
| **Hynix CJR** | **1.40-1.45V** | |
| **Hynix AFR** | **1.35-1.40V** | Pouco benefício acima de 1.40V |
| **Micron Rev. B (8Gb)** | **1.35V** | ⚠️ Pode escalar negativamente acima disso |

> [!WARNING]
> ICs Micron **antigos** (pré-Rev.E, como M8B) podem ficar **instáveis** quando você **aumenta** a voltagem acima de 1.35V, mesmo com mesma frequência e timings. Esse fenômeno é chamado de **negative voltage scaling**.

---

## 7. 🏷️ Binning — O que é e Por que Importa

**Binning** é o processo de classificar chips com base na sua performance. O fabricante testa cada wafer e separa os ICs em diferentes "bins" (caixas/categorias) de acordo com a frequência e timings que conseguem atingir.

### Como Funciona na Prática

Um mesmo IC (ex: Samsung B-Die) pode ser vendido em vários bins diferentes:

```
        ┌─── DDR4-4000 CL17 @ 1.40V ───► Bin altíssimo (caro)
        │
Samsung ├─── DDR4-3600 CL16 @ 1.35V ───► Bin alto
B-Die   │
        ├─── DDR4-3200 CL14 @ 1.35V ───► Bin alto (equivalente ao 3600 CL16)
        │
        ├─── DDR4-3000 CL14 @ 1.35V ───► Bin médio
        │
        └─── DDR4-2400 CL15 @ 1.20V ───► Bin baixo (ruim para OC!)
```

> [!IMPORTANT]
> **B-Die de bin baixo é SIGNIFICATIVAMENTE pior que B-Die de bin alto!** Não espere que um kit DDR4-2400 CL15 vá atingir os mesmos resultados de um kit DDR4-3200 CL14, mesmo sendo ambos B-Die. A G.Skill é conhecida por fazer binning extensivo e é uma das melhores opções para garantir B-Die de qualidade.

### Como Comparar Bins do Mesmo IC

Para saber qual kit é um bin mais apertado, divida a frequência pelo timing que **NÃO escala com voltagem**:

**Exemplo com Micron Rev. E (tRCD não escala):**
- Kit A: DDR4-3000 CL15-**16**-16 → `3000 / 16 = 187.5`
- Kit B: DDR4-3200 CL16-**18**-18 → `3200 / 18 = 177.78`

**Resultado:** Kit A (187.5) é um bin **mais apertado** que Kit B (177.78).

Isso significa que o Kit A provavelmente consegue fazer DDR4-3200 CL16-18-18, mas o Kit B pode **não** conseguir fazer DDR4-3000 CL15-16-16.

> [!TIP]
> **Não use tCL para comparar bins!** O tCL escala com voltagem em todos os ICs, então não é um bom indicador de qualidade do bin. Use tRCD para a maioria dos ICs (exceto B-Die, onde todos escalam).

---

## 8. 📊 Ranks e Densidade — Single Rank vs. Dual Rank

### Single Rank (SR) vs. Dual Rank (DR)

| Aspecto | Single Rank | Dual Rank |
|---------|:-----------:|:---------:|
| **Frequência máxima** | ✅ Geralmente mais alta | ❌ Geralmente mais baixa |
| **Performance real** | ❌ Menor em muitos cenários | ✅ Maior (rank interleaving) |
| **Carga no IMC** | ✅ Menor | ❌ Maior (precisa mais VCCSA/SOC) |

### Por que Dual Rank pode ser MELHOR?

O ganho de performance vem do **rank interleaving**: o controlador de memória pode fazer **operações em paralelo** nos dois ranks. Enquanto um rank está fazendo refresh, o outro está disponível para leitura/escrita.

**Benefícios técnicos do Dual Rank:**
- **2x mais bank groups** disponíveis → Mais chances de usar timings curtos (tRRD_S ao invés de tRRD_L)
- **2x mais banks** → Mais rows podem estar abertas simultaneamente
- **Menos conflitos de row** → Menos operações de fechar/abrir rows
- **Aumento significativo em copy bandwidth** no AIDA64

> [!TIP]
> Em plataformas modernas (Comet Lake, Zen3+), o suporte para Dual Rank melhorou muito. Em muitas Z490, Dual Rank B-Die (2x16GB) atinge a mesma frequência que Single Rank B-Die, dando **todos os benefícios** do rank interleaving sem perda de frequência.

### Configuração x8 vs. x16

Sticks com configuração **x16** (8 chips de maior capacidade) têm **metade dos banks e bank groups** comparado a **x8** (16 chips), resultando em **menor performance**. Evite configurações x16 quando possível.

### Impacto Total de Ranks no Sistema

| Configuração | Ranks Totais | Carga no IMC |
|-------------|:------------:|:------------:|
| 2x SR (1 por canal) | 2 | ✅ Baixa |
| 2x DR (1 por canal) | 4 | ⚠️ Média |
| 4x SR (2 por canal) | 4 | ⚠️ Média |
| 4x DR (2 por canal) | 8 | ❌ Alta |

Mais ranks = mais carga no IMC = potencialmente precisa de mais VCCSA (Intel) ou SOC Voltage (AMD).

---

## 9. 🖥️ Influência da Placa-Mãe (Daisy Chain vs. T-Topology)

A placa-mãe influencia **significativamente** o OC de RAM através do **layout dos traces de memória**.

### Daisy Chain

```
IMC ──► DIMM Slot 1 ──► DIMM Slot 2
```

- ✅ **Melhor com 2 sticks** (1 por canal)
- ❌ Usar 4 sticks pode reduzir **significativamente** a frequência máxima
- 📝 Layout mais comum em placas mainstream

### T-Topology

```
                ┌──► DIMM Slot 1
IMC ──► Split ──┤
                └──► DIMM Slot 2
```

- ✅ **Melhor com 4 sticks** (2 por canal)
- ⚠️ Com 2 sticks, a perda não é tão grande quanto 4 sticks em Daisy Chain
- 📝 Encontrado em placas high-end (ex: Z390 Aorus Master)

### Como Saber qual Layout sua Placa Usa?

A maioria dos fabricantes **não divulga** essa informação. Mas você pode deduzir pela **QVL (Qualified Vendor List)**:
- Se a frequência mais alta validada é com **2 DIMMs** → Provavelmente **Daisy Chain**
- Se a frequência mais alta validada é com **4 DIMMs** → Provavelmente **T-Topology**

> [!NOTE]
> Segundo Buildzoid, a diferença entre Daisy Chain e T-Topology **só importa acima de DDR4-4000**. Se você está rodando DDR4-3600/3800 (típico em AMD Ryzen com FCLK 1:1), o layout não faz diferença significativa.

### Outras Considerações

- **Placas com 2 DIMM slots** (ITX) → Geralmente atingem as **maiores frequências**
- **Qualidade do PCB** → Placas mais caras geralmente têm PCBs com mais camadas e melhor integridade de sinal
- **BIOS** → A qualidade do BIOS do fabricante faz diferença enorme. ASUS e MSI costumam ter os melhores BIOSes para OC de RAM

---

## 10. 🧮 O Controlador de Memória Integrado (IMC)

O **IMC (Integrated Memory Controller)** é o componente dentro da CPU responsável por comunicar com a RAM. Ele é o **segundo maior limitador** do OC de RAM (depois do IC).

### 10.1 IMC Intel

**Voltagens relevantes:**

| Voltagem | Nome | Função | Range Típico |
|----------|------|--------|-------------|
| **VCCSA** | System Agent Voltage | Alimenta o IMC e controlador de memória | 0.95V - 1.35V |
| **VCCIO** | I/O Voltage | Alimenta o I/O do IMC | 0.95V - 1.25V |

**Dicas para Intel:**
- Comece com VCCSA e VCCIO no **padrão** e aumente conforme necessário
- Geralmente VCCSA precisa ser **igual ou ligeiramente maior** que VCCIO
- Em plataformas modernas (10ª gen+), o IMC é bastante capaz
- **Gear 1 vs. Gear 2** (11ª/12ª gen):
  - **Gear 1** = MCLK:IMC 1:1 → Melhor latência, mais difícil de estabilizar em frequências altas
  - **Gear 2** = MCLK:IMC 1:2 → Permite frequências mais altas, mas dobra a latência base

**IMC por Geração Intel (resumo):**

| Geração | Platform | IMC Quality |
|---------|----------|-------------|
| 6ª/7ª | Z170/Z270 | Bom |
| 8ª/9ª | Z370/Z390 | Muito bom |
| 10ª | Z490 | Excelente |
| 11ª | Z590 | Bom (Gear 1 limitado a ~3600-3866) |
| 12ª | Z690 | DDR4 e DDR5 (DDR4 excelente) |

### 10.2 IMC AMD (Infinity Fabric / FCLK)

O IMC AMD em plataformas Ryzen é **diferente e especial** porque está diretamente ligado ao **Infinity Fabric Clock (FCLK)**.

**Relação MCLK:FCLK:**

```
┌─────────────────────────────────────────────┐
│     MCLK : FCLK = 1:1 (modo ideal)         │
│                                              │
│  DDR4-3600 → MCLK 1800 MHz → FCLK 1800 MHz │
│  DDR4-3800 → MCLK 1900 MHz → FCLK 1900 MHz │
│                                              │
│     Qualquer dessincronia (2:1) causa        │
│     GRANDE penalidade de latência!           │
└─────────────────────────────────────────────┘
```

> [!IMPORTANT]
> Em AMD Ryzen, **SEMPRE** mantenha MCLK:FCLK em **1:1**. Rodar DDR4-4000 com FCLK 1800 (ratio 2:1) é geralmente **PIOR** que rodar DDR4-3600 com FCLK 1800 (1:1) por causa da latência adicionada pelo dessincronia.

**Voltagens relevantes AMD:**

| Voltagem | Função | Range Típico |
|----------|--------|-------------|
| **SOC Voltage (VSOC)** | Alimenta o IMC, IF, e I/O | 1.00V - 1.20V |
| **VDDG CCD** | Voltagem para a parte CCD do Infinity Fabric | 0.90V - 1.10V |
| **VDDG IOD** | Voltagem para a parte IOD do Infinity Fabric | 0.95V - 1.15V |
| **VDDP** | Voltagem auxiliar do IMC | 0.90V - 1.05V |

**FCLK Máximo por Geração:**

| Geração | FCLK Típico Máximo | DDR4 Equivalente (1:1) |
|---------|:------------------:|:---------------------:|
| Ryzen 1000 (Zen) | ~1466 MHz | DDR4-2933 |
| Ryzen 2000 (Zen+) | ~1600 MHz | DDR4-3200 |
| Ryzen 3000 (Zen2) | ~1800-1900 MHz | DDR4-3600-3800 |
| Ryzen 5000 (Zen3) | ~1800-2000 MHz | DDR4-3600-4000 |

> [!TIP]
> O "sweet spot" para Ryzen 3000/5000 é geralmente **DDR4-3600 CL14-16 com FCLK 1800 MHz** ou **DDR4-3800 CL16 com FCLK 1900 MHz** (se o seu chip aguentar). Teste FCLK stability separadamente com ferramentas como OCCT VRAM test + Prime95 Large FFTs.

**Regras importantes para AMD:**
1. **VSOC** deve ser **maior** que VDDG IOD, que deve ser **maior ou igual** a VDDG CCD
2. Hierarquia: `VSOC > VDDG IOD ≥ VDDG CCD`
3. VSOC excessivo pode causar **degradação** do IMC a longo prazo — não passe de 1.20V para uso diário
4. Teste FCLK stability **separadamente** de DRAM stability

---

## 11. 🌡️ Temperaturas e Seu Efeito na Estabilidade

> [!WARNING]
> A temperatura é um fator **frequentemente ignorado** mas **extremamente importante** na estabilidade do OC de RAM.

### Efeitos da Temperatura Alta

| Temperatura | Efeito |
|:-----------:|--------|
| **< 40°C** | ✅ Ideal — Máxima estabilidade |
| **40-50°C** | ⚠️ OK — Pode precisar afrouxar timings levemente |
| **50-60°C** | ❌ Problemático — tREFI pode precisar ser reduzido significativamente |
| **> 60°C** | 🔥 Perigoso — Instabilidade frequente, risco de corrupção de dados |

### Por que Temperatura Importa

A RAM usa capacitores para armazenar dados. Em temperaturas mais altas, os capacitores **perdem carga mais rápido**, exigindo refreshes mais frequentes. Isso afeta diretamente:

- **tREFI** — Pode precisar ser reduzido (menor = mais refreshes)
- **tRFC** — Pode precisar ser aumentado (mais tempo para cada refresh)
- **Estabilidade geral** — Timings que passam testes a 30°C podem falhar a 50°C

### Samsung B-Die e Temperatura

O B-Die é **particularmente sensível** a temperatura. Muitos overclockers reportam que:
- Testes passam de madrugada (ambiente frio) mas falham de tarde (ambiente quente)
- A diferença de 5-10°C pode ser o suficiente para causar erros

### Soluções

1. **Heatspreaders de qualidade** — Os incluídos no kit geralmente são suficientes
2. **Ventilação direta** — Um fan de 120mm apontado para os DIMMs faz grande diferença
3. **Não instale RAM entre GPU e CPU** — Se possível, use os slots mais afastados da GPU
4. **Monitore temperaturas** — Use HWiNFO64 para monitorar sensor DIMM (nem todas as placas reportam)

---

## 12. 🔧 Passo a Passo: Como Fazer OC de RAM DDR4

### 12.1 Preparação

**Antes de começar, você precisa:**

1. **Identificar seu IC** (veja [seção 4](#4--como-identificar-o-ic-da-sua-ram))
2. **Saber os limites do seu IMC** (veja [seção 10](#10--o-controlador-de-memória-integrado-imc))
3. **Ter software de teste instalado** (veja [seção 14](#14--softwares-de-teste-de-estabilidade))
4. **Ter software de benchmark** (veja [seção 15](#15--softwares-de-benchmark))
5. **Anotar** os valores stock de tudo (screenshot do BIOS ou Thaiphoon Burner)
6. **Saber resetar o BIOS** (clear CMOS) caso trave

> [!CAUTION]
> **SEMPRE** faça um teste de estabilidade rápido com as configurações **stock** primeiro! Se sua RAM já está instável no stock, OC só vai piorar.

### 12.2 Encontrando um Baseline

O primeiro passo é encontrar uma **frequência máxima estável** com timings soltos. Isso identifica o limite da sua RAM e IMC.

**Procedimento:**

```
1. Entre no BIOS
2. Desative XMP/DOCP (volte tudo para JEDEC/auto)
3. Defina timings primários bem soltos:
   - tCL = 16 (ou 18 para ICs fracos)
   - tRCD = 20
   - tRP = 20
   - tRAS = 40
   - tCWL = 16
4. Defina VDIMM = 1.40V (exceto para ICs com scaling negativo)
5. Comece na frequência do XMP e aumente 200 MT/s por vez
6. Para cada frequência:
   a. Se NÃO da boot → Diminua frequência 100 MT/s
   b. Se da boot mas é instável → Tente aumentar VDIMM ou afrouxar timings
   c. Se da boot e é estável → Anote e tente subir mais 200 MT/s
7. Quando encontrar o máximo, anote → Este é seu TETO de frequência
```

> [!TIP]
> **Para AMD Ryzen:** Lembre-se de manter FCLK = MCLK/2 (1:1 ratio). Então se está testando DDR4-3600, coloque FCLK = 1800 MHz. Se FCLK não estabiliza, o limite é do FCLK e não da RAM.

### 12.3 Ajustando Timings Primários

Agora que você tem uma frequência máxima, é hora de apertar os timings. **Trabalhe um timing por vez** e teste estabilidade entre cada alteração.

#### tCL (CAS Latency)
- **Comece em:** O valor que você usou no baseline
- **Reduza:** 1 por vez
- **Se instável:** Aumente VDIMM em +0.025V e teste novamente
- **Lembre:** tCL escala com voltagem em todos os ICs

**Valores esperados por IC a DDR4-3600:**
| IC | tCL Típico | tCL Apertado |
|:--:|:----------:|:------------:|
| S8B | 14 | 12-13 |
| H8D | 16 | 14-15 |
| M8E | 16 | 14-15 |
| H8C | 16 | 15-16 |

#### tRCD (RAS to CAS Delay)
- **B-Die:** Pode apertar com mais voltagem (escala!)
- **Todos os outros ICs:** Não escala com voltagem. Use o mínimo estável e pronto.
- **Se instável:** Não adianta aumentar VDIMM (exceto B-Die). Aumente o timing.

**Valores esperados por IC a DDR4-3600:**
| IC | tRCD Típico | tRCD Apertado |
|:--:|:-----------:|:-------------:|
| S8B | 14 | 12-13 |
| H8D | 19 | 17-18 |
| M8E | 18 | 16-17 |
| H8C | 19 | 18-19 |

#### tRP (Row Precharge)
- Mesma lógica do tRCD
- Em muitos ICs, tRP = tRCD (mesmo valor)
- **B-Die:** Pode apertar com voltagem

#### tRAS (Row Active Time)
- **Regra geral:** `tRAS = tCL + tRCD + 2` (ou `tRAS = tCL + tRCD + tRRD_L`, dependendo)
- Valores muito baixos podem causar instabilidade
- Em muitas placas Intel, tRAS pode ser setado muito baixo (ex: 28) sem problemas

### 12.4 Ajustando Timings Secundários

Após estabilizar os primários, passe para os secundários. Eles têm menos impacto individual, mas **somados** fazem diferença significativa.

#### tRFC (Refresh Cycle Time)
**Um dos timings MAIS impactantes em performance fora dos primários.**

tRFC é o tempo que a RAM leva para fazer um ciclo de refresh. Quanto menor, melhor.

**Valores esperados por IC a DDR4-3600:**
| IC | tRFC Típico | tRFC Apertado |
|:--:|:-----------:|:-------------:|
| S8B | 280-320 | 220-260 |
| H8D | 400-500 | 350-420 |
| M8E | 500-600 | 400-500 |
| H8C | 450-550 | 380-450 |

> [!TIP]
> B-Die é o rei do tRFC baixo porque tRFC **escala com voltagem** nele. Com 1.45V, muitos kits B-Die fazem tRFC ~250 ou menos a DDR4-3600.

#### tRRD_S e tRRD_L
- **tRRD_S**: Row-to-row delay para banco diferente → Geralmente 4
- **tRRD_L**: Row-to-row delay para mesmo banco → Geralmente 4-6
- Mínimo absoluto é geralmente 4 para ambos

#### tFAW (Four Activate Window)
- Dependente de tRRD: `tFAW ≥ 4 × tRRD_S` (para x8) ou `tFAW ≥ 4 × tRRD_S` (para x16, use `2 × tRRD`)
- Para x8: `tFAW = 16` é um bom ponto de partida
- Pode ser apertado até 16 na maioria dos ICs

#### tWR (Write Recovery)
- Geralmente 10-16
- Tente 10 ou 12 e veja se estabiliza

#### tCWL (CAS Write Latency)
- Geralmente `tCWL = tCL` ou `tCL - 1` ou `tCL - 2`
- Em frequências pares (3200, 3600, 4000), tCWL deve ser par
- Em frequências ímpares (3000, 3400, 3800), tCWL pode ser ímpar

#### tWTR_S e tWTR_L
- **tWTR_S**: Write to read (diferente bank group) → Mínimo 2-4
- **tWTR_L**: Write to read (mesmo bank group) → Mínimo 6-8

### 12.5 Ajustando Timings Terciários

Os terciários afetam principalmente bandwidth e são mais arriscados de apertar.

#### tREFI (Refresh Interval)
**O ÚNICO timing que você quer o MAIS ALTO possível.**

- **Default:** Geralmente ~7800 (depende da frequência)
- **Objetivo:** Aumente o máximo possível (32767 ou 65534 em algumas placas)
- **Cuidado:** Muito sensível à temperatura! O que estabiliza a 30°C pode falhar a 45°C

> [!WARNING]
> tREFI alto significa refreshes menos frequentes. Se a RAM esquentar muito, os capacitores perdem carga antes do próximo refresh, causando erros. Se você rodar tREFI alto, **monitore temperaturas**.

#### tRDRD, tWRWR, tRDWR, tWRRD
Esses timings controlam os delays entre operações consecutivas:

- **tRDRD_sg/dg**: Read-to-Read same/different group
- **tWRWR_sg/dg**: Write-to-Write same/different group
- **tRDWR_sg/dg**: Read-to-Write
- **tWRRD_sg/dg**: Write-to-Read

Valores mínimos variam por plataforma. Consulte guias específicos para sua placa.

### 12.6 Dicas Específicas para Intel

1. **Command Rate (CR):** Tente CR1 (1T) ao invés de CR2 (2T). CR1 tem ~1-2ns menos latência, mas é mais difícil de estabilizar
2. **VCCSA:** Aumente em incrementos de 0.025V se estiver instável em frequências altas
3. **VCCIO:** Geralmente segue VCCSA, mas pode ser ligeiramente menor
4. **Gear 1 vs Gear 2:** Sempre tente Gear 1 primeiro (11ª/12ª gen). Gear 1 DDR4-3600 é geralmente melhor que Gear 2 DDR4-4400
5. **Adaptive voltage para VDIMM:** Algumas placas ASUS têm essa opção — pode ajudar com estabilidade

### 12.7 Dicas Específicas para AMD

1. **FCLK primeiro:** Encontre seu FCLK máximo estável antes de otimizar timings
2. **Hierarquia de voltagem:** `VSOC > VDDG IOD ≥ VDDG CCD > VDDP`
3. **ProcODT:** Timing crítico para AMD. Valores comuns: 36.9Ω, 40Ω, 43.6Ω, 48Ω. Experimente
4. **CLDO_VDDP:** Geralmente 900-1000mV funciona bem
5. **GDM (Gear Down Mode):** Ativado por padrão, força timings pares. Desativar permite timings ímpares (ex: CL15 ao invés de CL16), mas exige CR1
6. **PowerDown Mode:** Desative para OC
7. **RttNom, RttWr, RttPark (On-Die Termination):** Valores críticos para estabilidade. Valores comuns para 2 sticks SR: RttNom = Disabled, RttWr = 80Ω, RttPark = 60Ω (ou 48Ω)
8. **CAD_BUS:** Tente 24-20-20-24 ou 30-20-20-24 como ponto de partida

---

## 13. 📖 Guia de Cada Timing Explicado

Aqui está uma referência rápida de **todos os timings importantes** e o que eles fazem:

| Timing | Nome | Direção | Impacto | Descrição |
|--------|------|:-------:|:-------:|-----------|
| **tCL** | CAS Latency | ⬇️ Menor | 🔴 Alto | Delay entre comando read e dados disponíveis |
| **tRCD** | RAS to CAS Delay | ⬇️ Menor | 🔴 Alto | Delay para ativar uma row e acessar uma coluna |
| **tRP** | Row Precharge | ⬇️ Menor | 🔴 Alto | Tempo para desativar (fechar) uma row |
| **tRAS** | Active to Precharge | ⬇️ Menor | 🟡 Médio | Tempo mínimo que uma row fica ativa |
| **tRC** | Row Cycle | ⬇️ Menor | 🟡 Médio | `tRAS + tRP` — Ciclo completo de uma row |
| **tRFC** | Refresh Cycle | ⬇️ Menor | 🔴 Alto | Tempo de um ciclo de refresh |
| **tREFI** | Refresh Interval | ⬆️ Maior | 🔴 Alto | Intervalo entre refreshes |
| **tRRD_S** | Row-to-Row (diff) | ⬇️ Menor | 🟡 Médio | Delay entre activates em bank groups diferentes |
| **tRRD_L** | Row-to-Row (same) | ⬇️ Menor | 🟡 Médio | Delay entre activates no mesmo bank group |
| **tFAW** | Four Activate Window | ⬇️ Menor | 🟡 Médio | Janela máxima para 4 activates |
| **tWR** | Write Recovery | ⬇️ Menor | 🟢 Baixo | Tempo entre fim de write e precharge |
| **tCWL** | CAS Write Latency | ⬇️ Menor | 🟡 Médio | Delay entre comando write e dados escritos |
| **tWTR_S** | Write to Read (diff) | ⬇️ Menor | 🟢 Baixo | Delay write→read, bank groups diferentes |
| **tWTR_L** | Write to Read (same) | ⬇️ Menor | 🟢 Baixo | Delay write→read, mesmo bank group |
| **CR** | Command Rate | ⬇️ 1T | 🟡 Médio | Ciclos para enviar comando (1T vs 2T) |

---

## 14. 🧪 Softwares de Teste de Estabilidade

> [!CAUTION]
> **NUNCA** use apenas um teste de estabilidade! Rode pelo menos 2-3 testes diferentes para ter confiança no seu OC.

### ❌ Evitar

| Software | Por que evitar |
|----------|----------------|
| AIDA64 Memory Test | Não é bom para encontrar erros de memória |
| MemTest64 (TechPowerUp) | Também não detecta erros com eficiência |

### ✅ Recomendados (em ordem de prioridade)

#### 1. TestMem5 (TM5) — O Melhor
- **Download:** [TM5](https://mega.nz/file/vLhxBahB#WwJIpN3mQOaq_XsJUboSIcaMg3RlVBWvFnVspgJpcLY)
- **Config recomendada:** Extreme by anta777 (a mais estressante)
- O programa deve mostrar "Customize: Extreme1 @anta777" quando a config está carregada
- **Tempo mínimo:** 3 ciclos completos (20-30 min dependendo da RAM)
- **Problema comum:** Se todas as threads crasham ao iniciar com a config extreme, edite `Testing Window Size (Mb)=1408`. Substitua por: `(RAM total - margem para Windows) / threads do CPU`. Ex: `(16384 - 3000) / 16 = ~837`

#### 2. OCCT — Teste Dedicado de Memória
- **Download:** [OCCT](https://www.ocbase.com/)
- Use o teste de memória dedicado com instruções **SSE** ou **AVX**
- **Dica Intel:** SSE é melhor para testar voltagens do IMC, AVX é melhor para testar VDIMM
- O teste **Large AVX2 CPU** também estressa RAM significativamente — modo Normal (não Extreme)
- Para testar FCLK (AMD): Use VRAM test no máximo + Prime95 Large FFTs simultaneamente

#### 3. Karhu RAM Test (pago)
- [Karhu RAM Test](https://www.karhusoftware.com/ramtest/)
- Simples e eficiente. ~6000% coverage é considerado estável
- Custa ~$10 mas vale cada centavo

#### 4. GSAT (Google Stress App Test)
- Via WSL (Windows Subsystem for Linux):
  ```bash
  sudo apt update
  sudo apt-get install stressapptest
  stressapptest -M 13000 -s 3600 -W --pause_delay 3600
  ```
  - `-M` = quantidade de memória em MB
  - `-s` = duração em segundos
  - `--pause_delay` = deve ser igual a `-s` para pular testes de power spike

#### 5. y-cruncher
- [Download](http://www.numberworld.org/y-cruncher/)
- Muito sensível a erros de memória e bom para teste complementar

#### 6. Prime95 Large FFTs
- [Download](https://www.mersenne.org/download/)
- Use FFT range customizado de 800K-800K
- ⚠️ Desmarque "Run FFTs in place"
- Em `prime.txt`, adicione `TortureAlternateInPlace=0` abaixo de `TortureWeak`

### Comparação de Eficácia

```
TM5 Extreme > Karhu > GSAT > OCCT > Prime95 > y-cruncher
  (mais rápido para encontrar erros)    (mais lento)
```

> [!NOTE]
> O TM5 é o mais rápido e estressante, mas é possível passar 30 min de TM5 e falhar em 10 min de Karhu. Sempre rode **múltiplos testes** para confirmar estabilidade.

---

## 15. 📈 Softwares de Benchmark

Após estabilizar seu OC, use benchmarks para medir o ganho de performance:

| Software | Tipo | Custo | Métricas |
|----------|------|-------|----------|
| **AIDA64** | Cache & Memory Benchmark | Trial 30 dias | Latência, Read/Write/Copy bandwidth |
| **Intel MLC** | Memory Latency Checker | Gratuito | Latência detalhada, bandwidth sob diferentes cargas |
| **Intel MLC GUI** | GUI para Intel MLC | Gratuito | Interface gráfica para Intel MLC |
| **xmrig** | Benchmark (mineração) | Gratuito | Muito sensível a RAM — rode com `--bench=1M` como admin |
| **MaxxMEM2** | Alternativa ao AIDA64 | Gratuito | Bandwidth (valores mais baixos que AIDA64, não comparáveis diretamente) |
| **Super Pi Mod 1.5** | Benchmark de cálculo | Gratuito | 1M-8M dígitos, tempo menor = melhor |
| **PYPrime 2.x** | Benchmark rápido | Gratuito | Escala muito bem com clock, cache, frequência e timings de RAM |

> [!TIP]
> O benchmark mais popular e padronizado é o **AIDA64 Cache & Memory Benchmark**. Use-o para comparar antes/depois do OC. Dica: clique direito no botão "Start Benchmark" e selecione "Memory tests only" para pular os testes de cache e ir direto ao que interessa.

---

## 16. 🖥️ Softwares para Visualizar Timings

### Intel

| Plataforma | Software |
|------------|----------|
| X99 | ASRock Timing Configurator v3.0.6 |
| Z370/Z390 | ASRock Timing Configurator v4.0.4 |
| Z170/Z270/Z490 | ASRock Timing Configurator v4.0.3 |
| Z590 (Rocket Lake) | ASRock Timing Configurator v4.0.10 |
| Z690 (Alder Lake DDR4) | ASRock Timing Configurator v4.0.14 ou MSI Dragon Ball |

> [!NOTE]
> Você **NÃO precisa** ter uma placa-mãe ASRock para usar o ASRock Timing Configurator. Ele funciona em qualquer placa Intel.

### AMD

| Plataforma | Software |
|------------|----------|
| Todas as plataformas AM4/AM5 | [ZenTimings](https://zentimings.protonrom.com/) |

---

## 17. 🔗 Links Úteis e Referências

### Guias e Referências
- 📘 [r/overclocking Wiki — RAM DDR4](https://www.reddit.com/r/overclocking/wiki/ram/ddr4) — Wiki comunitária com lista completa de ICs por fabricante
- 📘 [HardwareLuxx — Ryzen RAM OC Limitações](https://www.hardwareluxx.de/community/threads/ryzen-ram-oc-m%C3%B6gliche-limitierungen.1216557/) — Infográfico excelente sobre ICs
- 📘 [DDR4 OC Guide (JEDEC JESD79-4B)](http://www.softnology.biz/pdf/JESD79-4B.pdf) — Especificação oficial do padrão DDR4

### Vídeos Recomendados
- 🎥 [Buildzoid — Memory Trace Layouts](https://www.youtube.com/watch?v=3vQwGGbW1AE) — Daisy Chain vs T-Topology explicado
- 🎥 [Buildzoid — x8 vs x16 Memory](https://www.youtube.com/watch?v=k6SIdxq2yxE) — Por que x16 é pior
- 🎥 [Buildzoid — B-Die Binning](https://www.youtube.com/watch?v=rmrap-Jrfww) — Por que B-Die barato é ruim

### Ferramentas
- 🛠️ [Thaiphoon Burner](http://www.softnology.biz/files.html) — Identificação de ICs via SPD
- 🛠️ [DRAM Calculator for Ryzen](https://www.techpowerup.com/download/ryzen-dram-calculator/) — Calculadora de timings para AMD (use como ponto de partida, não como verdade absoluta)
- 🛠️ [Calculadora de Voltage Scaling (Desmos)](https://www.desmos.com/calculator/psisrpx3oh) — Preveja CL em diferentes voltagens

### Benchmark
- 📊 [kingfaris.co.uk/ram](https://kingfaris.co.uk/ram) — Comparação SR vs DR em jogos e sintéticos

---

## 📝 Notas Finais

> [!IMPORTANT]
> **Overclock de RAM é um processo iterativo e que requer paciência.** Não espere encontrar a configuração perfeita na primeira tentativa. O processo correto é:
>
> 1. Altere **UMA coisa por vez**
> 2. Teste estabilidade
> 3. Se estável, anote e vá para o próximo timing
> 4. Se instável, reverta e tente uma abordagem diferente
> 5. Repita até ficar satisfeito

Lembre-se: **um OC de RAM instável é PIOR que stock**. RAM instável causa:
- BSODs aleatórios (especialmente WHEA errors)
- Corrupção de arquivos (silenciosa — a pior espécie)
- Perda de dados
- Crashes em jogos e aplicações

**Sempre valide extensivamente com múltiplos testes de estabilidade antes de considerar o OC como "pronto para uso diário".**

---

*Última atualização: Julho 2026*
