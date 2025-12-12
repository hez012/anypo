# AnyPO – Domain Architecture Map  
**Version:** 1.0  
**Status:** Approved / Fixed  

---

## Table of Contents
1. [Purpose / Цель](#purpose--цель)  
2. [Scope / Область применения](#scope--область-применения)  
3. [Architecture Levels (L0–L3)](#architecture-levels-l0l3)  
4. [Domain Map Overview](#domain-map-overview)  
5. [A. Core Operational Domains](#a-core-operational-domains)  
    - D01. Catalog & Product  
    - D02. Clients & Counterparties  
    - D03. Cash & Payments  
    - D04. Documents  
    - D05. Warehouse & Inventory  
    - D06. Replenishment & Procurement  
    - D07. Credit & Installments  
    - D08. HR & KPI  
    - D09. Loyalty & Bonuses  
6. [B. Extended Domains](#b-extended-domains)  
7. [C. Platform & SaaS Domains](#c-platform--saas-domains)  
8. [Standard Artefacts per Domain](#standard-artefacts-per-domain)  
9. [Recommended Development Order](#recommended-development-order)  
10. [Notes & Recommendations](#notes--recommendations)

---

## Purpose / Цель

Документ устанавливает **официальную карту доменов системы AnyPO** – основу архитектуры уровня Enterprise.  

Цель: предоставить структурированную модель всех модулей системы, чтобы обеспечить:

- модульность,
- масштабируемость,  
- согласованность архитектуры,
- предсказуемый процесс разработки,  
- единый подход к документации и API.

---

## Scope / Область применения

Документ применяется к:

- проектированию архитектуры,
- моделированию базы данных,
- подготовке ERD,
- разработке backend API,
- формированию технического задания,
- созданию документации и GitHub-структуры,
- планированию Roadmap продукта.

---

## Architecture Levels (L0–L3)

### **L0 — Product Level (Видение продукта)**
Описание AnyPO как SaaS-решения, цели, миссия, рынок, стратегические возможности.

### **L1 — Domain Map (Карта доменов)**
Крупнейшие бизнес-области системы: Catalog, Clients, Warehouse, HR, API и др.

### **L2 — Domain Modules**
Внутренние модули каждого домена.  
Например, в Catalog & Product входят Pricing, Barcodes, Variants, Recipes, Services.

### **L3 — Artefacts**
Каждый домен имеет набор обязательных артефактов:

- Logical ERD  
- Physical ERD  
- DBML  
- SQL DDL  
- API Specification  
- Test Data (INSERT)  
- Markdown Documentation  
- Use Cases  

---

# Domain Map Overview

Домены разбиты на три группы:

- **A — Core Operational Domains**  
- **B — Extended Domains**  
- **C — Platform & SaaS Domains**

Это официальная архитектура AnyPO, на которой строится система.

---

# A. Core Operational Domains

---

## **D01. Catalog & Product**  
*“Товары, услуги, цены, атрибуты, рецепты, публикации, весы и штрихкоды.”*

Основной модуль ядра AnyPO.  
Отвечает за:

- справочник товаров (simple / variant / bundle / service),
- услуги,
- ингредиенты и полуфабрикаты,
- категории (отдельно для товаров и услуг),
- единицы измерения + конвертации,
- штрихкоды, PLU, внутренние коды,
- динамические атрибуты,
- правила ценообразования (markup/margin),
- фактические цены по точкам,
- техкарты и рецепты (HoReCa),
- медиа и галереи,
- публикация в каналы (ECOM, агрегаторы, QR-меню),
- подключение весов и принтеров этикеток.

---

## **D02. Clients & Counterparties**  
*“Клиенты, поставщики, их группы, филиалы, контактные данные.”*

Содержит:

- юридические и физические лица,
- под-клиенты (altmus),
- адреса, телефоны, зоны,
- кредитные лимиты,
- задолженности,
- сегментация.

---

## **D03. Cash & Payments**  
*“Кассы, платежи, банковские операции, фискализация.”*

Включает:

- кассы (наличные, POS, банк),
- ПКО/РКО,
- эквайринг,
- фискальные аппараты,
- подарочные карты.

---

## **D04. Documents**  
*“Накладные, чеки, перемещения, возвраты, списания.”*

Описание:

- единая модель документа,
- нумерация по магазинам,
- контроль последовательности,
- состояние документа.

---

## **D05. Warehouse & Inventory**  
*“Склады, остатки, партии, ячейки, инвентаризации.”*

Включает:

- магазины / склады / зоны,
- адресное хранение,
- партии и expiry date,
- пересортица,
- списания,
- движения.

---

## **D06. Replenishment & Procurement**  
*“Закупки, заявки, автоматическое пополнение.”*

Описание:

- min/max,
- аналитика потребления,
- рекомендации поставщикам,
- автоматическое формирование заявок.

---

## **D07. Credit & Installments**  
*“Рассрочка для клиентов.”*

Функции:

- график платежей,
- первоначальный взнос,
- просрочка,
- договоры,
- расчёт будущей нагрузки.

---

## **D08. HR & KPI**  
*“Персонал, зарплата, рабочие смены, KPI продавцов и экспедиторов.”*

Описание:

- сотрудники,
- табель рабочего времени,
- дисциплинарные действия,
- премии/штрафы,
- KPI модели.

---

## **D09. Loyalty & Bonuses**  
*“Уровни, бонусы, черные списки, расчёт правил начисления.”*

Функции:

- уровни Standart/Silver/Gold/Platinum,
- бонусные карты,
- начисление по правилам,
- интеграция с документами.

---

# B. Extended Domains

---

## **D11. Pharma Extension**  
*“Аптечный модуль (МНН, аналоги, действующие вещества).”*

---

## **D12. Assets & Equipment**  
*“POS-оборудование, инвентарь, внутренние активы.”*

---

## **D13. Serial & Warranty**  
*“Серийники, IMEI, гарантия клиентов.”*

---

# C. Platform & SaaS Domains

---

## **D14. API & Integration Layer**  
*“REST / GraphQL, TSD, e-commerce, банки, KKM.”*

---

## **D15. Scale & Label Integration**  
*“Весы, принтеры этикеток, PLU, шаблоны.”*

---

## **D16. Reporting & Analytics**  
*“Аналитические отчеты, интеграции с BI.”*

---

## **D17. Licensing & SaaS Plans**  
*“Тарифы, ограничения, биллинг, кабинет клиента.”*

---

## **D18. Settings, Security & Backup**  
*“Права доступа, настройки системы, резервное копирование.”*

---

# Standard Artefacts per Domain

Каждый домен имеет обязательные артефакты:

1. Domain Specification (Confluence/GitHub)  
2. Logical ERD  
3. Physical ERD  
4. DBML  
5. SQL DDL  
6. API Specification  
7. Test Data  
8. Markdown Documentation  
9. Non-functional requirements  

---

# Recommended Development Order

1. **D01 — Catalog & Product** *(текущий)*  
2. D05 — Warehouse & Inventory  
3. D02 — Clients  
4. D03 — Cash  
5. D04 — Documents  
6. HR, Loyalty, HoReCa, API, Scale  
7. Extensions (Pharma, Assets, Serial)

---

# Notes & Recommendations

- Этот документ является официальной архитектурной основой AnyPO.  
- Любые изменения должны попадать в changelog.  
- Доменная модель должна оставаться стабильной и понятной для всех разработчиков.  
- Все артефакты должны соответствовать Naming Conventions и стандартам AnyPO.

---
