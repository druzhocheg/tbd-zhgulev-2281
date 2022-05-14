# tbd-zhgulev-2281
1.Вывести всеми возможными способами имена и фамилии студентов, средний балл которых от 4 до 4.5
```SQL
SELECT * FROM students
WHERE score >= 4 and score <= 4.5

```
![Задание 1]()

2. Познакомиться с функцией CAST. Вывести при помощи неё студентов заданного курса (использовать Like)
```SQL
SELECT * FROM students st
WHERE CAST(st.n_group AS varchar) LIKE '2%'

```
pic resutl

3.Вывести всех студентов, отсортировать по убыванию номера группы и имени от а до я
```SQL
SELECT * FROM students st
ORDER BY st.n_group DESC, st.name

```

pic resutl

4.Вывести студентов, средний балл которых больше 4 и отсортировать по баллу от большего к меньшему
```SQL
SELECT st.name, st.surname, st.n_group, st.score
FROM students st
WHERE st.score >= 4
ORDER BY score DESC
```

pic resutl

5.Вывести на экран название и риск футбола и хоккея
```SQL
SELECT * FROM hobbies  hb
WHERE hb.name = 'Хоккей' OR hb.name = 'Футбол'

```

pic resutl


6.Вывести id хобби и id студента которые начали заниматься хобби между двумя заданными датами (выбрать самим) и студенты должны до сих пор заниматься хобби

```SQL
SELECT * FROM students_hobbies st
WHERE (CAST(st.date_start as varchar) LIKE '201%' or
	CAST(st.date_start as varchar) LIKE '2020%') and
	date_finish is null



```

pic resutl


7.Вывести студентов, средний балл которых больше 4.5 и отсортировать по баллу от большего к меньшему
```SQL
SELECT st.name, st.surname, st.n_group, st.score
FROM students st
WHERE st.score > 4.5
ORDER BY st.score DESC

```

pic resutl


8.Из запроса №7 вывести несколькими способами на экран только 5 студентов с максимальным баллом
```SQL
SELECT st.name, st.surname, st.n_group, st.score
FROM students st
WHERE st.score > 4.5
ORDER BY st.score DESC
LIMIT 5

```

pic resutl



9.Выведите хобби и с использованием условного оператора сделайте риск словами:
```SQL
SELECT h.risk, h.name,
CASE
	WHEN risk >= 8 THEN 'Очень высокий'
	WHEN risk >= 6 and risk < 8 THEN 'Высокий'
	WHEN risk >= 4 and risk < 6 THEN 'Среднний'
	WHEN risk >= 2 and risk < 4 THEN 'Низкий'
	WHEN risk < 2 THEN 'Очень низкий'
END
FROM hobbies h

```

pic resutl


10.Вывести 3 хобби с максимальным риском
```SQL
SELECT * FROM hobbies
ORDER BY risk DESC
LIMIT 3

```

pic resutl