# 🍓🥬 SmartMorangos

O **[SmartMorangos](https://smartmorangos.com.br)** é uma plataforma SaaS completa de gestão agrícola e caderno de campo digital. Inicialmente desenhada para a complexidade do cultivo de morangos, a plataforma evoluiu e hoje atende com excelência tanto o cultivo de morango em solo/substrato quanto **sistemas hidropônicos** (alface, rúcula, manjericão, etc.). O sistema oferece controle total desde o plantio até a venda, passando por gestão de soluções nutritivas, rastreabilidade e controle financeiro.

> **Nota:** Este é um projeto proprietário (Closed Source). Este repositório serve como vitrine pública para demonstração de arquitetura, padrões de código e portfólio técnico.

---

## 🌟 A Origem do Projeto

O **SmartMorangos** não nasceu de um escopo de software imaginário, mas sim da agricultura familiar e das dores reais do campo. Quando meus pais iniciaram a produção de morangos e, posteriormente, investiram em hidroponia, sentimos na pele a falta de ferramentas que realmente entendessem a rotina e as dificuldades do produtor.

Foi nesse cenário que decidi unir minhas duas realidades: **desenvolvedor de software e produtor rural**. O projeto ganhou vida de forma totalmente iterativa e prática. À medida que sofríamos com algum gargalo no dia a dia da estufa, uma nova funcionalidade, automação ou ajuste era implementado no sistema. 

A homologação e a criação das regras de negócio foram baseadas em conversas familiares e no suporte técnico do nosso agrônomo, buscando uma solução completa e proprietária para a **Chácara São José** ([@ChacaraSaoJoseSJP](https://www.instagram.com/ChacaraSaoJoseSJP)). 

Hoje, o que aprendo no campo, implemento no código. E, embora tenha nascido para resolver nossos problemas internos, o sistema foi estruturado com uma arquitetura **multi-tenant** robusta, permitindo que seja disponibilizado publicamente para outros produtores que buscam o mesmo nível de controle, rastreabilidade e eficiência.

## ✨ Funcionalidades Principais

### 🏡 Gestão de Propriedades e Estruturas

* Gestão organizacional de Múltiplas Propriedades.
* Gestão de acesso aos funcionários e colaboradores e autorizações de acesso.
* Mapeamento de **Talhões** e infraestrutura física.
* Controle físico e de ocupação de **Bancadas** hidropônicas.
* Gestão de Equipamentos e plano de Manutenções.

### 🌱 Operações Agrícolas e Hidroponia

* **Safras (Morango):** Gestão de longo prazo, do transplante das mudas à finalização do talhão.
* **Ciclos de Cultivo (Hidroponia):** Gestão de giro rápido nas bancadas (germinação, vegetativo, colheita e pós-colheita).
* **Soluções Nutritivas:** Formulação de macronutrientes, micronutrientes e monitoramento de EC/pH.
* **Prescrições Agronômicas:** Receituário técnico e controle fitossanitário.
* **Manejos e Aplicações:** Composição de caldas, registro de defensivos e manejos culturais.
* **Colheitas:** Registro de produtividade integrado ao ciclo exato da planta.

### 📦 Gestão de Insumos e Logística

* Controle de estoque e movimentações em tempo real.
* Planejamento de limpezas de bancada e utilização de insumos.
* Gestão de Compras e Fornecedores.
* Criação de **Rotas e Entregas** logísticas para as vendas.

### 💰 Faturamento e Financeiro

* Gestão de Despesas e Categorização.
* Controle de Horas e Pagamento de Funcionários.
* CRM básico de Clientes e Catálogo de Produtos.
* Ponto de Venda (PDV) e registro de Vendas e Pagamentos.
* Configurações de Faturamento personalizadas por propriedade.

---

## 🛠️ Tecnologias Utilizadas

### Backend

* **.NET 10** - Framework principal
* **ASP.NET Core** - Web API
* **Entity Framework Core 10** - ORM
* **SQL Server** - Banco de dados relacional
* **AWS Lambda** - Serverless computing (Hospedagem da API)

### Frontend

* **Blazor WebAssembly** - SPA Framework
* **MudBlazor** - Component Library

### Integrações e Serviços

* **AWS SES** - Envio de e-mails transacionais
* **Stripe** - Gestão de assinaturas (SaaS) e processamento de pagamentos
* **QuestPDF** - Geração de PDFs de alta performance
* **QRCoder** - Geração de QR Codes para rastreabilidade

---

## 🏗️ Arquitetura do Sistema

O sistema segue os princípios da **Clean Architecture** e **Domain-Driven Design (DDD)**, garantindo que o core do negócio (o campo e a estufa) não dependa de frameworks de UI ou banco de dados.

```
┌─────────────────────────────────────────────────────────┐
│                    Presentation Layer                   │
│            (SmartMorangos.WebUI - Blazor WASM)          │
└────────────────────┬────────────────────────────────────┘
                     │ HTTP/REST
┌────────────────────▼────────────────────────────────────┐
│                    API Layer                            │
│          (SmartMorangos.Lambda - AWS Lambda)            │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│                Application Layer                        │
│         (SmartMorangos.Application - Services)          │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│                  Domain Layer                           │
│            (SmartMorangos.Domain - Core)                │
│  • Entities (Safra, Bancada, CicloDeCultivo, etc.)      │
│  • Value Objects (Endereco, CondicoesClimaticas, etc.)  │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│              Infrastructure Layer                       │
│      (SmartMorangos.Infrastructure - Data Access)       │
│  • EF Core DbContext (SQL Server)                       │
│  • Unit of Work & Repositories                          │
└─────────────────────────────────────────────────────────┘

```

---

## 🧬 Estrutura do Domínio (Visão Arquitetural)

O banco de dados e as entidades do SmartMorangos foram desenhados para refletir a complexidade de uma operação agrícola real. A arquitetura separa claramente a infraestrutura física (onde se planta) do ciclo biológico (o que se planta), integrando todas as operações ao controle de estoque e financeiro.

O domínio está dividido em 4 grandes pilares:

### 1. Organização Físico-Geográfica (Infraestrutura)
Mapeia a estrutura perene e os ativos da fazenda.
* **Hierarquia de Área:** A `Propriedade` é a raiz do sistema. Ela é dividida em `Talhões` (áreas georreferenciadas), que por sua vez podem conter `Bancadas` (estruturas físicas para sistemas suspensos ou hidroponia).
* **Ativos e Máquinas:** O maquinário é controlado na entidade `Equipamentos`, que possui cronogramas de `ManutencoesEquipamentos` atrelados aos `Funcionarios`.

### 2. Ciclo Biológico e Produtivo
Separa a estrutura de terra/bancada da planta em si, permitindo rotatividade.
* **Planejamento Macro (`Safras`):** O guarda-chuva que agrupa todo um ciclo produtivo e define os fornecedores de mudas e o padrão genético.
* **Gestão Micro (`Ciclo de Cultivo / CultivoBancadas`):** O lote específico de plantas. Ele carrega a data de transplante, a fase atual do ciclo biológico e a expectativa de colheita.
* **Resultados (`Colheitas`):** O ponto final do ciclo biológico, onde a produção é pesada, classificada (descarte, processamento, comércio) e disponibilizada para venda.

### 3. Operações Agronômicas e Fitossanitárias
O coração técnico da fazenda, garantindo o padrão de qualidade e a rastreabilidade exigida por certificações.
* **Controle de Pragas e Doenças:** O fluxo se inicia nas `InspecoesFitossanitarias`, que detectam anomalias e geram `PrescricoesAgronomicas` (o receituário técnico).
* **Intervenções:** A prescrição se desdobra em `Aplicações` (pulverização de defensivos com controle de método, bico e condições climáticas) e `ManejosCulturais` (poda, desbrota, liberação de agentes biológicos).
* **Nutrição Hídrica:** Gestão pesada de `SolucoesNutritivas` (com suas fases específicas), medindo ativamente o EC (Condutividade Elétrica) e pH, tanto da calda injetada quanto da solução drenada (`ControlesDeSolucaoNutritiva` e `Fertirrigacoes`).

### 4. Backoffice: Estoque, RH e Financeiro
Todo evento no campo reflete automaticamente no escritório.
* **Gestão de Materiais:** `Compras` abastecem os `Insumos` (estoque). Cada intervenção agronômica (aplicações, limpezas de bancada) gera baixas rastreáveis nas `MovimentacoesDeEstoque`.
* **Esforço Humano:** O trabalho da equipe é medido por `ControlesDeHoras` e `PlanejamentosDeAtividades`, que alimentam a base de `PagamentosDeFuncionarios`.
* **Fluxo de Caixa:** A operação deságua no financeiro. Insumos e salários geram `Despesas` (categorizadas), enquanto o escoamento da colheita acontece através de `Vendas` aos `Clientes`, gerenciando até mesmo as rotas de `Entregas` físicas.

---

## 📐 Padrões de Código e Boas Práticas

* **Repository & Unit of Work:** Centralização do acesso a dados e garantia de atomicidade nas transações do SQL Server.
* **Value Objects mapeados via EF Core (`.OwnsOne.ToJson()`):** Utilização intensiva de colunas JSON nativas do SQL Server para armazenar dados complexos (ex: `Macronutrientes` dentro de `SolucaoNutritiva`, `CondicoesClimaticas` em `Aplicacao`) sem explodir a modelagem em dezenas de tabelas de relacionamento 1:1.
* **Asynchronous Programming:** Uso obrigatório de `async/await` em toda a cadeia de I/O para otimização de recursos na AWS Lambda.
* **Fail-Fast Validations:** Validações rigorosas na camada de Application e Domain para proteger a integridade agronômica dos dados (ex: impedir colheita em bancada vazia ou aplicação de defensivo sem prescrição válida).

## 📂 Estrutura do Projeto

```
SmartMorangos/
│
├── SmartMorangos.Domain/              # Camada de Domínio
│   ├── Entities/                      # Entidades do domínio
│   │   ├── Safra.cs
│   │   ├── Talhao.cs
│   │   ├── Colheita.cs
│   │   ├── Venda.cs
│   │   └── ...
│   ├── Enums/                         # Enumeradores
│   │   ├── TipoMuda.cs
│   │   ├── StatusPagamento.cs
│   │   └── ...
│   └── Interfaces/
│       └── Repositories/              # Contratos de repositórios
│
├── SmartMorangos.Application/         # Camada de Aplicação
│   ├── Features/                      # Organizados por feature
│   │   ├── Safras/
│   │   │   └── DTOs/
│   │   ├── Colheitas/
│   │   └── ...
│   ├── Interfaces/
│   │   └── Services/                  # Contratos de serviços
│   ├── Services/                      # Implementação de serviços
│   │   ├── SafraService.cs
│   │   ├── ColheitaService.cs
│   │   └── ...
│   └── Reports/                       # Geração de relatórios
│       └── Documents/
│
├── SmartMorangos.Infrastructure/      # Camada de Infraestrutura
│   ├── Contexts/
│   │   └── AppDbContext.cs           # DbContext do EF Core
│   ├── Repositories/                  # Implementação de repositórios
│   │   ├── SafraRepository.cs
│   │   ├── ColheitaRepository.cs
│   │   └── ...
│   ├── Services/                      # Serviços externos
│   │   ├── AwsSesEmailService.cs
│   │   └── StripeService.cs
│   ├── Migrations/                    # Migrações do banco
│   └── UnitOfWork.cs
│
├── SmartMorangos.Lambda/              # API (AWS Lambda)
│   ├── Controllers/                   # Controllers REST
│   │   ├── SafraController.cs
│   │   ├── ColheitaController.cs
│   │   └── ...
│   ├── serverless.template            # CloudFormation template
│   └── LambdaEntryPoint.cs
│
└── SmartMorangos.WebUI/               # Frontend Blazor WASM
    ├── Pages/                         # Páginas/Rotas
    │   ├── safras/
    │   ├── colheitas/
    │   ├── vendas/
    │   └── ...
    ├── Shared/                        # Componentes compartilhados
    │   ├── NavMenu.razor
    │   └── Components/
    ├── wwwroot/                       # Assets estáticos
    └── Program.cs
 
```

---
---

## 📞 Contato

**Vinícius H. S. Araújo**

* Desenvolvedor .NET Sênior e Idealizador do SmartMorangos
* LinkedIn: [linkedin.com/in/viniciushsaraujo](https://linkedin.com/in/viniciushsaraujo)
* GitHub: [@ViniciusHSAraujo](https://github.com/ViniciusHSAraujo)
* Website: [smartmorangos.com.br](https://smartmorangos.com.br)

---

<div align="center">

**Engenharia de software aplicada à inteligência do agronegócio.**

</div>
