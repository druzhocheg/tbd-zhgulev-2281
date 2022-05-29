# Сервис удалённого заказа еды

1) Пользователь. Регистрация
2) Пользователь. Авторизация
3) Пользователь. Просмотр меню.
4) Пользователь. Добавление в корзину
5) Пользователь. Оформление заказа (Считаем, что оплата
производится «автоматом»)
6) Пользователь. Просмотр оформленных ранее заказов с пагинацией и
поиском по диапазону дат и блюд из меню, а также с сортировкой по
ценнику и дате
7) Пользователь. Возможность повторить (добавить в корзину) все
блюда из ранее оформленного заказа
8) Пользователь. Просмотр состояния заказа
9) Управляющий. Добавление новых позиций в меню (должен быть
указан состав, вес, белки, жиры, углеводы, ккал)
10) Управляющий. Видеть список заказов
11) Управляющий. Менять статус заказа
---------------------------------------------------------------------

## Таблицы

### Еда

```SQL
CREATE TABLE  food(
	
	id serial PRIMARY KEY,
	name varchar(255) not null,
    price decimal not null,
    compound varchar(255) not null,
	weight_gr decimal not null,
	bel decimal,
	zhir decimal,
    ulg decimal,
    ccal decimal not null
	
)
CREATE sequence food_seq;
```

### Заказы

```SQL
CREATE TABLE  orders(
	id serial PRIMARY KEY,
	user_id not null,
	food_id not null,
    order_date not null,
	status int not null -- 0 в корзине, 1 обработка, 2 выполнение, 3 выполнен
);
CREATE sequence orders_seq;
alter table orders add
constraint status_CHECK
check (status >= 0 and status <= 3);
```

### Пользователи

```SQL
СREATE TABLE  users(
	id serial PRIMARY KEY,
	name varchar(255) not null,
	surname varchar(255) not null,
	access int not null 
    login varchar(255),
    password varchar(255)
    active boolean
);

CREATE sequence users_seq;
alter table users add
constraint access_CHECK
check (access >= 0 and access <= 1);
```

## Управляющий

9) Управляющий. Добавление новых позиций в меню (должен быть
указан состав, вес, белки, жиры, углеводы, ккал)
```SQL
INSERT INTO food (name, price, compound, weight_gr, bel, zhir, ulg, ccal)
VALUES ($1, $2, $3, $4, $5, $6, $7, $8);
```

10) Управляющий. Видеть список заказов
```SQL
SELECT user_id, food_id, order_date, status
FROM orders
WHERE status = $1 
```

11) Управляющий. Менять статус заказа
```SQL
UPDATE orders
SET status = $1
WHERE id = $2 or user_id = $3 or order_date = $4, food_id = $5, status = $6;
```

## Пользователь 

1) Пользователь. Регистрация
```SQL
INSERT INTO users (name, surname, access, login, password, active)
VALUES ($1, $2, $3, $4, $5, 0);
```

2) Пользователь. Авторизация
```SQL
UPDATE users
SET active = true
WHERE login = $1 and password = $2
```

3) Пользователь. Просмотр меню.
```SQL
SELECT name, price, compound, weight_gr, bel, zhir, ulg, ccal
FROM food
```

4) Пользователь. Добавление в корзину
```SQL
INSERT INTO orders (user_id, food_id, order_date, status)
VALUES ($1, $2, $3, 0);
```

5) Пользователь. Оформление заказа (Считаем, что оплата
производится «автоматом»)
```SQL
UPDATE orders 
SET status = 1
WHERE user_id = $1, food_id = $2, order_date = $3
```

6) Пользователь. Просмотр оформленных ранее заказов с пагинацией и
поиском по диапазону дат и блюд из меню, а также с сортировкой по
ценнику и дате
```SQL
SELECT us.name, f.name, o.order_date
FROM orders o
INNER JOIN food f
ON f.id = o.food_id
INNER JOIN users us
ON us.id = o.user_id
WHERE status = 3 and (us.name = $1 or o.id = $2 or o.order_date = $3)
ORDER BY f.price DESC, o.order_date DESC
LIMIT $4 OFFSET $5 
```

7) Пользователь. Возможность повторить (добавить в корзину) все
блюда из ранее оформленного заказа
```SQL
BEGIN 
	FOR food_id in
		SELECT food_id
		FROM orders o
		INNER JOIN food f
		ON f.id = o.food_id
		INNER JOIN users us
		ON us.id = o.user_id
		WHERE status = 3 and o.order_date = $1
		ORDER BY f.price DESC, o.order_date DESC
	LOOP 
		INSERT INTO orders (user_id, food_id, order_date, status)
		VLUE ($1, food_id, $2, 0)
	END LOOP;
END;

```

8) Пользователь. Просмотр состояния заказа
```SQL
SELECT food_id, order_date, status
FROM orders
WHERE user_id = $1 and order_date = $2
```