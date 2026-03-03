# 🛒 Automação SauceDemo - Fluxo de Compra com Observabilidade

Este projeto automatiza o fluxo de compra da mochila (**Sauce Labs Backpack**) no site **SauceDemo**. Diferente de automações convencionais, este repositório implementa um motor de **Log Customizado** e **Gestão de Evidências (Screenshots)** organizada por carimbos de tempo e integração com CI.

---

## 📋 Funcionalidades Automatizadas

### 🎒 Jornada de Compra (End-to-End)

* **Cenário:** Login, seleção de produto, conferência de carrinho, preenchimento de checkout e confirmação final.
* **Validação:** Checagem rigorosa de preços ($29.99 / $32.39 total), nomes de produtos, quantidades e IDs de pagamento (`SauceCard #31337`).
* **Estrutura:** Utiliza `test.step` do Playwright para segmentar as fases do teste, facilitando a leitura e o debug no relatório.

---

## 🛠️ Tecnologias e Utilitários

* **Playwright Core** — Automação de alta performance.
* **Custom Logger (`logger.js`)** — Estende o Playwright para gerar um arquivo `steps.log` com timestamps (`ISO 8601`) de cada ação.
* **Path Tools (`path_tools.js`)** — Gerencia a criação dinâmica de pastas baseada na data/hora da execução ou em `RUN_TAG` de sistemas de CI.
* **Snap System (`snap.js`)** — Captura prints de página inteira (`fullPage: true`) com nomes amigáveis para documentação de cada passo.

---

## 🏗️ Arquitetura de Evidências

O projeto organiza automaticamente os resultados para evitar sobrescrita:

```text
artifacts/
├── logs/
│   └── steps.log           # Histórico textual da execução
└── screenshots/
    └── 2026/
        └── 03/
            └── 02/
                └── 16-40-30/ # Pasta da execução (HH-mm-ss)
                    └── TC001-Passo01-Home.png

```

---

## 📁 Estrutura do Projeto

```text
├── tests/
│   └── comprar_mochila.spec.js  # Script principal (Step-by-Step)
├── utils/
│   ├── logger.js               # Extensão do Test/Expect com Logs
│   ├── path_tools.js           # Inteligência de diretórios e datas
│   └── snap.js                 # Motor de captura de screenshots
├── artifacts/                  # ⚠️ Pasta gerada na execução (Logs/Snaps)
├── playwright.config.js        # Configurações globais
└── README.md                   # Documentação

```

---

## 🚀 Como Executar o Projeto

1. **Instale as dependências:**
```bash
npm install

```


2. **Rode os testes com rastreabilidade:**
```bash
npx playwright test

```


3. **Verifique os Logs e Snaps:**
Navegue até a pasta `artifacts/` para visualizar as evidências geradas automaticamente com base no horário da sua execução.

---

## 💡 Diferenciais Técnicos

* **Logger Injetado:** O log não é apenas um `console.log`, ele é uma função injetada via `base.extend` que sabe o nome do teste que está rodando.
* **Sanitização de Nomes:** O `snap.js` limpa caracteres especiais dos nomes dos arquivos para garantir compatibilidade entre Windows, Linux e Mac.
* **Flexibilidade de Timeout:** O script ajusta dinamicamente o `setTimeout` para processos mais longos, garantindo estabilidade no fluxo de checkout.
