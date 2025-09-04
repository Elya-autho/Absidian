[[#Назначение срочного мониторинга]]

### Назначение срочного мониторинга
№25307 (ул. Барбюса, 3, Челябинск) 79220135563
1) находим id Мониторщика, кому назначать (данное id будет необходимо нам как: employee_id; assignee_id; assigner_id):

```sql
select id, "name" from public.employees
where active = true and name like '%25307%'

```

Выполнить:
employee_id = 253070
2) находим geo_points_id:

```sql 
select gp.id as geo_points_id, gp.address, c."name" from public.geo_points gp

join public.cities c on gp.city_id = c.id

join public.shop_networks sn on gp.shop_network_id = sn.id

where (gp.deleted is False and sn.is_online is False and gp.deleted is False) and (sn."name" like '%Монетка%' and gp.address like '%улица Рождественского, 13Б%' and c."name" like '%Челябинск%')
```
Выполнить:
geo_points_id = 370194
3) находим id задачи мониторщика:

```sql 
select st.id from public.shop_tasks st

join public.geo_points gp on st.geoshop_id = gp.id

join public.shop_networks sn on gp.shop_network_id = sn.id

join public.employees e on st.assignee_id = e.id

where (e.active is True and gp.deleted is False and sn.is_online is False and gp.deleted is False) and (sn."name" like '%Монетка%' and gp.address like '%улица Рождественского, 13Б%' and e.name like '%Челябинск%')
```
Выполнить:
shop_task_id = 556106

4) создаем срочную задачу (указав даты, автора задачи, конкретного конкурента и указав "ответственный магазин"):

  

```sql 
insert into public.tasks_forced (dt, dt_create, employee_id, shop_task_id, shop_id)

values (NOW(), NOW() - INTERVAL '1 DAY', '253070', '556106', '25307') RETURNING id as tasks_forced_id;
```
Выполнить:
tasks_forced_id = 8413988

  

5) создаем расписание задачи для мониторщика:

```sql 
insert into public.task_schedule (id, date_week_start, mon, tue, wen, thu, fri, sat, sun, dt_change, employee_id, shop_task_id)

values (nextval('task_schedule_id_seq'::regclass), now(), false, false, false, false, false, false, false, now(), '253070', '1799097') RETURNING id as task_schedule_id;
```

Выполнить:
task_schedule_id = 2780071

  

5.1) если уже есть. то через select:

```sql 
select id, date_week_start, mon, tue, wen, thu, fri, sat, sun, dt_change, employee_id, shop_task_id from public.task_schedule

where shop_task_id = '1799097'

order by id asc

limit 1
```

6) находим product_id и учитываем last_parse = true:

  

```sql 
select product_id, "name", min_forced_price, base_price, (base_price - min_forced_price) as difference from products

where min_forced_price < base_price and last_parse = true and min_forced_price <> 0

order by difference desc
```

  Выполнить:
product_id = 

  

7) создаем в таблице "сами продукты" назначенные в "Срочной задаче":

  

```sql 
insert into public.provisional_list_forced (product_id, task_forced_id)

values ('16482', '8414022') RETURNING id as provisional_list_forced_id;

  

insert into public.provisional_list_forced (product_id, task_forced_id)

values ('18809', '8414022') RETURNING id as provisional_list_forced_id;
```

Выполнить:
provisional_list_forced_id =

8) создаем "Предварительный список товаров" назначенные в "Срочной задаче":

  

```sql 
insert into public.provisional_list (product_id, shop_task_id)

values ('16482', '556106') RETURNING id as provisional_list_id;

  

insert into public.provisional_list (product_id, shop_task_id)

values ('18809', '556106') RETURNING id as provisional_list_id;

```
  
Выполнить:
provisional_list_id =


### Назначение планово-срочного мониторинга
https://test.kb-monita.ru/?#schedule?shops=25307
1. В разделе "График мониторингов" в фильтре "Наш магазин "выбираем нужный магазин (пример 25307)
2. нажимаем на кнопку "Показать".
3. Выводятся записи в таблице.
4. Выбираем нужного конкурента (пример: Пятерочка). Выставляем зеленую метку на нужный день.
![[Pasted image 20250728170415.png]]

5. Нажимаем на кнопку "Применить".
В результате получаем установку планового мониторинга на нужного конкурента

Далее применяем скрипт назначения срочного мониторинга
1) находим id Мониторщика, кому назначать (данное id будет необходимо нам как: employee_id; assignee_id; assigner_id):

```slq 
select id, "name" from public.employees where active = true and name like '%25307%'
```
Выполнить

employee_id = 253070

  2) находим geo_points_id:
```sql
select gp.id as geo_points_id, gp.address, c."name" from public.geo_points gp

join public.cities c on gp.city_id = c.id

join public.shop_networks sn on gp.shop_network_id = sn.id

where (gp.deleted is False and sn.is_online is False and gp.deleted is False) and (sn."name" like '%Пятерочка%' and gp.address like '%улица Рождественского, 7Б%' and c."name" like '%Челябинск%')
```

  Выполнить

geo_points_id = 370202

3) находим id задачи мониторщика:

  

```sql 
select st.id from public.shop_tasks st

join public.geo_points gp on st.geoshop_id = gp.id

join public.shop_networks sn on gp.shop_network_id = sn.id

join public.employees e on st.assignee_id = e.id

where (e.active is True and gp.deleted is False and sn.is_online is False and gp.deleted is False) and (sn."name" like '%Пятерочка%' and gp.address like '%улица Рождественского, 7Б%' and e.name like '%Челябинск%')
```

  Выполнить

shop_task_id = 556114

4) создаем срочную задачу (указав даты, автора задачи, конкретного конкурента и указав "ответственный магазин"):

  

```slq 
insert into public.tasks_forced (dt, dt_create, employee_id, shop_task_id, shop_id)

values (NOW(), NOW() - INTERVAL '1 DAY', '253070', '556114', '25307') RETURNING id as tasks_forced_id;
```

Выполнить

tasks_forced_id = 8413989

  5) создаем расписание задачи для мониторщика:

```sql
insert into public.task_schedule (id, date_week_start, mon, tue, wen, thu, fri, sat, sun, dt_change, employee_id, shop_task_id)

values (nextval('task_schedule_id_seq'::regclass), now(), false, false, false, false, false, false, false, now(), '253070', '1799097') RETURNING id as task_schedule_id;

```
Выполнить

task_schedule_id = 2780071

  

5.1) если уже есть. то через select:

```sql 
select id, date_week_start, mon, tue, wen, thu, fri, sat, sun, dt_change, employee_id, shop_task_id from public.task_schedule

where shop_task_id = '1799097'

order by id asc

limit 1
```

  

6) находим product_id и учитываем last_parse = true:

  

```sql
select product_id, "name", min_forced_price, base_price, (base_price - min_forced_price) as difference from products

where min_forced_price < base_price and last_parse = true and min_forced_price <> 0

order by difference desc
```

  Выполнить

product_id =

  

7) создаем в таблице "сами продукты" назначенные в "Срочной задаче":

  

```sql 
insert into public.provisional_list_forced (product_id, task_forced_id)

values ('17686', '8413989') RETURNING id as provisional_list_forced_id;

  

insert into public.provisional_list_forced (product_id, task_forced_id)

values ('16306', '8413989') RETURNING id as provisional_list_forced_id;
```

Выполнить

provisional_list_forced_id =

8) создаем "Предварительный список товаров" назначенные в "Срочной задаче":

  

```sql 
insert into public.provisional_list (product_id, shop_task_id)

values ('17686', '556114') RETURNING id as provisional_list_id;

  

insert into public.provisional_list (product_id, shop_task_id)

values ('16306', '556114') RETURNING id as provisional_list_id;
```

  

  

Выполнить

provisional_list_id =






