recognize_images - для поиска в логах

mv_products_region_price - Каталог КБ таблица для остатков продуктов по регионам






Каталог КБ скрипт

SELECT pp."name" FROM public.products_shops pps

join public.products pp on pps.product_id = pp.product_id

join public.categories pc on pp.image_category_id = pc.category_id

WHERE pps.shop_id = 2777 AND pps.city_id = 627 and pc.parent_id = 100006

order by 1

Каталог кб - скрипт отбора наименования товара в категории по городу
select

distinct "p" .*,

COUNT(*) over() as count_rows,

627 as selected_city_id,

coalesce(min(pc.min_price), min(prp.min_price), p.price) as min_city_price,

sum(pc.quantity) as city_quantity,

coalesce(ANY_VALUE(pc.diff_price), ANY_VALUE(prp.diff_price)) as diff_price

from

"products" as "p"

left join "mv_products_region_price" as "prp" on

"prp"."product_id" = "p"."product_id"

and "prp"."region_id" in (

select

region_id

from

"cities"

where

"city_id" = 627)

left join "mv_products_cities" as "pc" on

"pc"."product_id" = "p"."product_id"

and pc.city_id = 627

where

exists (

select

*

from

"products_categories"

where

"product_id" = p.product_id

and "category_id" in (

select

"category_id"

from

"categories"

where

"category_id" = 100006

or "parent_id" = 100006))

and p.has_analog is not true

and p.invisible is false

and prp.quantity > 0

group by

"p"."product_id"

order by

"min_city_price" asc,

"p"."name" asc,

"p"."product_id" asc

limit 100 offset 0


Скрипт выбора наименования в категории по выбранному магазину в городе
select

distinct "p" .*,

COUNT(*) over() as count_rows,

2777 as selected_shop_id,

ps.price as shop_price,

ps.quantity as shop_quantity,

ps.is_sale as is_sale_on_shop

from

"products" as "p"

left join "products_shops" as "ps" on

"ps"."product_id" = "p"."product_id"

and "ps"."shop_id" = 2777

where

exists (

select

*

from

"products_categories"

where

"product_id" = p.product_id

and "category_id" in (

select

"category_id"

from

"categories"

where

"category_id" = 100006

or "parent_id" = 100006))

and p.has_analog is not true

and p.invisible is false

and "ps"."quantity" > 0

group by

"p"."product_id",

"ps"."shop_id",

"ps"."price",

"ps"."quantity",

"ps"."is_sale"

order by

"shop_price" asc,

"p"."name" asc,

"p"."product_id" asc

limit 100 offset 0