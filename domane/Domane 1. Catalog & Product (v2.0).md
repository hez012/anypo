# AnyPO — Домен 1. Catalog & Product (v2.0) - Концептуальная архитектура



# 1. Назначение и роль домена

Catalog & Product — это центральный домен AnyPO, который отвечает за:

единую номенклатуру:

товары (SKU),

ингредиенты и сырьё (HoReCa),

полуфабрикаты,

наборы/комбо,

услуги (обычные и HoReCa),

категории и дерево каталога,

единицы измерения и упаковки,

штрихкоды и PLU (включая внутренние, весовые),

цены и правила ценообразования,

изображения и медиа,

рецептуры/техкарты (для HoReCa),

публикации в внешние каналы (сайт, агрегаторы, QR-меню, т.д.).

Этот домен — единый источник правды обо всех “продуктах” системы: от сахара на полке до комплексного ланча и доставки.

2. Границы и зависимости домена
2.1. Catalog & Product предоставляет данные для:

Домен 2. Clients & Contracts (типы товаров для расчёта условий, прайсы по группам).

Домен 3. Cash & Payments (продажа товаров и услуг в чеках).

Домен 4. Documents (приход/расход/возврат/перемещение).

Домен 5. Warehouse & Inventory (учёт остатков, партия/срок годности).

Домен 6. Loyalty & Bonuses (бонусные ставки по товарам/группам).

Домен 7. HR & KPI (KPI продавцов/экспедиторов по товарам/группам).

Домен 8. HoReCa:

меню,

KDS/кухня,

сервисный сбор,

рецептуры и списание ингредиентов.

Домен 9. Replenishment & Procurement (min/max, заказ поставщикам).

Домен 10. Asset Registry (типовые позиции активов).

Домен 11. Pharma Extension (лекарства и медикаменты).

Домен 12. API & Integration:

e-commerce, агрегаторы (Wolt и т.п.),

мобильные приложения,

ТСД,

весы,

принтеры этикеток.

2.2. Catalog & Product НЕ отвечает за:

движения товара (остатки, партии) — это домен Warehouse.

документы (приход/расход/чеки) — домен Documents & POS.

гарантийные случаи и сервис — домен Serial & Warranty.

активы и инвентарь — Asset Registry.

планирование закупок — Replenishment.

HoReCa-меню, столы, депозиты — HoReCa-домен.

Но он даёт все атрибуты товаров/услуг, на которых те домены строятся.

3. Основные сущности и поддомены внутри Catalog

Внутри домена выделяются логические блоки:

Категории и дерево каталога

Единицы измерения и упаковки

Продукт (Product Core) + локализация

Варианты (Product Variants)

Штрихкоды и PLU (Barcodes & PLUs)

Динамические атрибуты (Attributes)

Дополнительные единицы измерения (Product Units)

Типы цен и правила ценообразования

Цены (фактические)

Изображения и медиа

Наборы/комбо (Bundles)

Рецепты и техкарты (HoReCa Recipes)

Каналы продаж и публикации (Sales Channels & Listings)

Услуги (Services) — базовые и HoReCa-сервисный сбор

Флаги и настройки интеграций (весы, этикетки и т.д.)

Флаги для расширенных доменов (pharma, asset, replenishment)

Ниже — краткое, но структурированное описание каждого блока.

4. Категории и дерево каталога
4.1. Category Core

product_category

иерархия (parent_id)

тип категории:

PRODUCT — категории товаров,

SERVICE — категории услуг,

MIXED — комбинированные/витринные (по необходимости).

код, флаги активности.

product_category_translation

поддержка AZ / RU / EN.

product_category_tree
(closure table, для быстрого поиска всех потомков/предков)

4.2. Особое:

Отдельное поддерево для услуг (category_type = SERVICE).

Категории используются:

в UI (группировка, фильтры),

в прайс-листах,

в бонусных схемах,

в отчётах.

5. Единицы измерения и упаковки
5.1. Unit of Measure

unit_of_measure

код (шт, кг, л, м, упаковка, коробка и т.д.),

наименование.

5.2. Product Units (упаковки, уровни)

product_unit

product_id → product

uom_id → unit_of_measure

conversion_factor — сколько базовых единиц содержит (1 коробка = 24 шт.)

is_default — базовая UoM для продукта.

Используется:

в весах,

в WMS,

в печати этикеток,

для разных уровней штрихкодов.

6. Продукт (Product Core) и локализация
6.1. Product Core

product

id

category_id → product_category

brand_id

sku (внутренний код)

product_type:

GOOD_RESALE (товар на продажу)

INGREDIENT (ингредиент / сырьё)

CONSUMABLE (расходные материалы)

SERVICE (услуги)

BUNDLE (набор/комбо)

SEMI_FINISHED (полуфабрикат)

MEDICINE (при включённом pharma-модуле)

ASSET_TEMPLATE (тип актива)

is_batch_tracked — есть ли партии

is_expiry_tracked — есть ли срок годности

default_uom_id → unit_of_measure

is_active — можно ли использовать в документах

is_sellable — продаётся ли в рознице/HoReCa

is_purchasable — закупается ли

is_producible — участвует в рецептах/производстве

is_weight_item — весовой товар

is_serialized — требуются серийные номера/IMEI

is_medicine — флаг лекарства (для фарм-модуля)

is_asset_template — используется как шаблон для активов

created_at, updated_at, created_by, updated_by

6.2. Локализация продукта

product_translation

product_id

lang_code (az/ru/en)

name

short_name (для чеков/экрана)

description

дополнительные поля при необходимости.

7. Варианты (Product Variants)

Для случаев:

размеры,

цвета,

упаковки,

разновидности с общим “родителем”.

product_variant

product_id → базовый товар

variant_sku

variant_code

is_active

Варианты используют те же:

атрибуты,

barcode,

цены,

публикации.

8. Штрихкоды и PLU (включая внутренние и весовые)

Ключевой блок, с учётом:

нескольких штрихкодов на товар,

упаковочных уровней,

внутренних кодов (начинающихся на 2),

весовых этикеток.

product_barcode

id

product_id → product

variant_id → product_variant (опционально)

product_unit_id → product_unit (опционально)

barcode (строка)

barcode_type:

SUPPLIER

INTERNAL

WEIGHT_LABEL

EAN13 / UPC / QR / DATAMATRIX и т.д.

is_primary — основной

is_auto_generated — сгенерирован системой

conversion_factor — дублирует product_unit либо используется, если unit не указан

notes — примечания

В Catalog & Product хранятся только данные;
генерация внутренних баркодов и логика работы с весами/принтерами реализуется в модуле Scale & Label Integration (но опирается на эти поля).

9. Динамические атрибуты (Product Attributes)

Позволяют конфигурировать:

цвет, размер, материал и т.п.

технические параметры,

кастомные поля клиента.

product_attribute_definition

код

data_type (string/number/bool/date/json)

is_required

is_variant_level (атрибут варианта, а не базового продукта)

is_filterable (для каталога/поиска)

is_searchable

product_attribute_value

product_id

variant_id (если привязка к варианту)

attribute_id

value_* (разные типы)

10. Типы цен и правила ценообразования
10.1. Типы цен

price_type

code:

RETAIL

WHOLESALE

DEALER

VIP

PROMO

HORECA_MENU

name

10.2. Правила ценообразования

product_price_rule

product_id / variant_id

price_type_id

base_type:

COST — от себестоимости

PRICE_TYPE — от другой цены

base_price_type_id (если base_type = PRICE_TYPE)

calc_mode:

MARKUP — наценка

MARGIN — маржа

rate_percent (ставка)

is_active

10.3. Фактические цены

product_price

product_id / variant_id

price_type_id

branch_id (магазин/объект)

currency

amount

11. Изображения и медиа

product_image

product_id

image_url

thumb_url

is_main

sort_order

Поддержка:

1 фото,

галерея,

миниатюры.

12. Наборы и комбо (Bundles)

product_bundle

product_id → товар-набор (product_type = BUNDLE)

bundle_type:

FIXED (фиксированный состав)

CONFIGURABLE (выбор элементов)

product_bundle_item

bundle_id

component_product_id

min_qty

max_qty

default_qty

Используется в:

акциях,

HoReCa (комбо-меню),

прайсе.

13. Рецепты и техкарты (HoReCa Recipes)

Вариант C:
рецепты — в Catalog & Product, меню и кухня — в HoReCa-домене.

product_recipe

product_id (готовое блюдо)

yield_qty (выход)

yield_uom_id

wastage_percent (усушка/потери)

product_recipe_item

recipe_id

ingredient_product_id (product_type = INGREDIENT / SEMI_FINISHED)

qty

uom_id

is_semi_finished

Склад/HoReCa будут использовать эти рецепты:

для автоматического списания ингредиентов,

для расчёта себестоимости.

14. Каналы продаж и публикации

Поддержка:

сайт,

маркетплейсы,

агрегаторы (Wolt и т.п.),

мобильные приложения,

QR-меню.

sales_channel

code (ECOM_FRONT, WOLT, QR_MENU, MOBILE_APP_OWNER и т.п.)

name

channel_type:

ECOMMERCE

AGGREGATOR

MARKETPLACE

MOBILE_APP

QR_MENU

is_active

product_channel_listing

product_id / variant_id

channel_id

is_published (галочка “публиковать/не публиковать”)

allow_order

external_sku

external_category

sync_status (OK / ERROR / PENDING)

Через это реализуется:

выбор 200 товаров из 1000 для сайта/агрегатора,

управление видимостью в каналах,

связка по внешним SKU.

15. Услуги (Services) — товарные услуги и HoReCa-сбор
15.1. Базовые услуги (A)

product.product_type = SERVICE

используются для:

доставки,

монтажа,

настройки,

обслуживания.

Имеют:

свою категорию (дерево Services),

цену (через price_rule / price),

описания и фото,

динамические атрибуты при необходимости.

Не участвуют:

в складских остатках,

в партиях,

в сроках годности.

15.2. HoReCa-услуги (C) — сервисный сбор

product.product_type = SERVICE

product.service_mode = 'HORECA_CHARGE' (например)

Дополнительно — через мета-таблицу:

product_service_settings

product_id

charge_calc_type: 'PERCENT_OF_CHECK'

percent_value

apply_scope (PER_CHECK, в перспективе PER_GUEST)

apply_automatically (true/false)

Реальная логика применения (к какому чеку, когда, как выводится в UI и отчётах) — в HoReCa-домене;
но инициализация и данные услуги — в Catalog.

16. Интеграции: весы и принтеры этикеток

Catalog предоставляет данные, а модуль Scale & Label Integration реализует обмен.

16.1. Флаги на уровне продукта

Для весов:

product.is_weight_item — весовой или штучный.

product.allow_export_to_scale — можно ли выгружать на весы.

привязка к product_unit (какая единица для весов).

Для этикеток:

связывание с шаблоном:

либо через поле label_template_code,

либо через внешнюю таблицу (будем детализировать позже).

16.2. PLU-коды и “весовые” штрихкоды

PLU/Label-коды (весы):

либо в product_barcode с barcode_type = WEIGHT_LABEL,

либо отдельное поле scale_plu_code (если будет удобнее).

Сам домен Catalog не хранит драйверы и SDK-весов — только абстрактные данные.

17. Флаги и расширения для других доменов
17.1. Serial & Warranty

В Catalog:

product.is_serialized — флаг серийного товара;

default_warranty_period_months (если нужно).

В домене Serial & Warranty:

фактические серийники/IMEI,

гарантийные кейсы.

17.2. Pharma Extension

В Catalog:

product.product_type = MEDICINE

product.is_medicine = true

В фарм-модуле:

действующее вещество,

классификатор,

взаимозаменяемость.

17.3. Asset Registry

В Catalog:

product.product_type = ASSET_TEMPLATE

product.is_asset_template = true

В Asset-домене:

конкретные экземпляры активов,

местоположение, статусы, амортизация.

17.4. Replenishment & Planning

В Catalog:

product.is_replenishable

возможные базовые параметры (например, дефолтный поставщик, базовая единица закупки).

В Replenishment:

min/max per warehouse,

политики пополнения.

18. Cross-cutting: языки, активность, аудит

Для всех ключевых сущностей:

флаги активности: is_active

временные метки: created_at, updated_at

авторы: created_by, updated_by

локализация через *_translation для AZ/RU/EN

19. Итог: что мы зафиксировали

Мы с тобой:

чётко определили границы домена Catalog & Product v2.0;

расписали сущности и их смысл;

разделили, что относится к каталогу, а что к другим доменам;

учли:

магазины, опт, HoReCa,

весы, этикетки,

услуги,

рецепты,

интеграции,

будущие модули (pharma, assets, replenishment).

Это — “карта местности”.

20. Дальнейшие шаги (предлагаю по порядку)

Сделать Logical ERD для Catalog & Product v2.0

в dbdiagram (DBML),

без привязки к типов PostgreSQL (логический уровень).

Затем:

Physical ERD (с типами Postgres),

DDL для схемы product в БД anypo,

тестовые данные,

Markdown-документ для GitHub (AnyPO_Domain_01_Catalog_Product.md),

и только после этого — переходить к следующему домену (Warehouse & Inventory).
