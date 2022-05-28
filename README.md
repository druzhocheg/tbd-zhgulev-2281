# tbd-zhgulev-2281
# Однотабличные запросы
1. Вывести всеми возможными способами имена и фамилии студентов, средний балл которых от 4 до 4.5
```SQL
SELECT * FROM students
WHERE score >= 4 and score <= 4.5
```
![Задание 1](/Single/ex_1.png)

-----------------------
<br><br>

2. Познакомиться с функцией CAST. Вывести при помощи неё студентов заданного курса (использовать Like)
```SQL
SELECT * FROM students st
WHERE CAST(st.n_group AS varchar) LIKE '3%'
```
![Задание 2](/Single/ex_2.png)

-----------------------
<br><br>

3. Вывести всех студентов, отсортировать по убыванию номера группы и имени от а до я
```SQL
SELECT * FROM students st
ORDER BY st.n_group DESC, st.name ASC
```
![Задание 3](/Single/ex_3.png)

-----------------------
<br><br>

4. Вывести студентов, средний балл которых больше 4 и отсортировать по баллу от большего к меньшему
```SQL
SELECT st.name, st.surname, st.n_group, st.score
FROM students st
WHERE st.score >= 4
ORDER BY score DESC
```
![Задание 4](/Single/ex_4.png)

-----------------------
<br><br>

5. Вывести на экран название и риск футбола и хоккея
```SQL
SELECT * FROM hobbies  hb
WHERE hb.name = 'Хоккей' OR hb.name = 'Футбол'
```

![Задание 5](/Single/ex_5.png)

-----------------------
<br><br>


6. Вывести id хобби и id студента которые начали заниматься хобби между двумя заданными датами (выбрать самим) и студенты должны до сих пор заниматься хобби

```SQL
SELECT * FROM students_hobbies st
WHERE (CAST(st.date_start as varchar) LIKE '201%' or
	CAST(st.date_start as varchar) LIKE '2020%') and
	date_finish is null
```

![Задание 6](/Single/ex_6.png)

-----------------------
<br><br>


7. Вывести студентов, средний балл которых больше 4.5 и отсортировать по баллу от большего к меньшему
```SQL
SELECT st.name, st.surname, st.n_group, st.score
FROM students st
WHERE st.score > 4.5
ORDER BY st.score DESC
```

![Задание 7](/Single/ex_7.png)

-----------------------
<br><br>


8. Из запроса №7 вывести несколькими способами на экран только 5 студентов с максимальным баллом
```SQL
SELECT st.name, st.surname, st.n_group, st.score
FROM students st
WHERE st.score > 4.5
ORDER BY st.score DESC
LIMIT 5
```

![Задание 8](/Single/ex_8.png)

-----------------------
<br><br>



9. Выведите хобби и с использованием условного оператора сделайте риск словами:
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

![Задание 9](/Single/ex_9.png)

-----------------------
<br><br>

10. Вывести 3 хобби с максимальным риском
```SQL
SELECT * FROM hobbies
ORDER BY risk DESC
LIMIT 3
```

![Задание 10](/Single/ex_10.png)

-----------------------
<br><br>



<br><br>
# Групповыее функции

1. Выведите на экран номера групп и количество студентов, обучающихся в них

```SQL
SELECT n_group,
       COUNT(n_group) AS stud_count
FROM students
GROUP BY n_group
ORDER BY n_group DESC;
```

![Задание 1](/Group/ex_1.png)

-----------------------
<br><br>
2. Выведите на экран для каждой группы максимальный средний балл

```SQL
SELECT n_group,
       MAX(score) AS stud_mx_score
FROM students
GROUP BY n_group
ORDER BY n_group DESC;
```

![Задание 2](/Group/ex_2.png)

-----------------------
<br><br>
3. Подсчитать количество студентов с каждой фамилией

```SQL
SELECT surname,
       COUNT(surname) AS surn_count
FROM students
GROUP BY surname
ORDER BY surname ASC;
```

![Задание 3](/Group/ex_3.png)

-----------------------
<br><br>
4. Подсчитать студентов, которые родились в каждом году
```SQL
SELECT COUNT(extract(year from date_birth)) AS birth_y_count,
	   extract(year from date_birth) as years
FROM students
GROUP BY years
ORDER BY years ASC;
```

![Задание 4](/Group/ex_4.png)

-----------------------
<br><br>
5. Для студентов каждого курса подсчитать средний балл см. Substr
```SQL
SELECT left(n_group::varchar, 1) course,
	ROUND(avg(score),3) as avg_score
FROM students
GROUP BY course
ORDER BY course ASC;
```

![Задание 5](/Group/ex_5.png)

-----------------------
<br><br>

6. Для студентов заданного курса вывести один номер группы с максимальным средним баллом
```SQL
SELECT MAX(score) AS max_score, n_group
FROM students
WHERE LEFT(n_group::varchar, 1) = '2'
GROUP BY n_group
ORDER BY AVG(score) DESC
LIMIT 1
```

![Задание 6](/Group/ex_6.png)

-----------------------
<br><br>

7. Для каждой группы подсчитать средний балл, вывести на экран только те номера групп и их средний балл, в которых он менее или равен 3.5. Отсортировать по от меньшего среднего балла к большему.
```SQL
SELECT n_group,
       ROUND(AVG(score),2) AS stud_avg_score
FROM students
GROUP BY n_group
HAVING AVG(score) <= 3.5
ORDER BY stud_avg_score ASC
```

![Задание 7](/Group/ex_7.png)

-----------------------
<br><br>
8.Для каждой группы в одном запросе вывести количество студентов, максимальный балл в группе, средний балл в группе, минимальный балл в группе
```SQL
SELECT n_group, COUNT(n_group) AS num_stud, 
    MAX(score) AS mx_score, 
    ROUND(AVG(score),2) AS avg_score, 
    MIN(score) AS mn_score
FROM students
GROUP BY n_group
```

![Задание 8](/Group/ex_8.png)

-----------------------
<br><br>

9. Вывести студента/ов, который/ые имеют наибольший балл в заданной группе
```SQL
SELECT st.*
FROM (SELECT n_group, MAX(score) AS mx_score
	  FROM students
	GROUP BY n_group
) res, students st	
WHERE st.score = res.mx_score AND st.n_group = '3046'
LIMIT 1
```

![Задание 9](/Group/ex_9.png)

-----------------------
<br><br>


10. Аналогично 9 заданию, но вывести в одном запросе для каждой группы студента с максимальным баллом.
```SQL
SELECT st.*
FROM (SELECT n_group, MAX(score) AS mx_score
	  FROM students
	GROUP BY n_group
) res, students st	
WHERE res.n_group = st.n_group AND st.score = res.mx_score 
```

![Задание 10](/Group/ex_10.png)

-----------------------
<br><br>

# Многотабличные запросы
1. Вывести все имена и фамилии студентов, и название хобби, которым занимается этот студент.

```SQL
SELECT 
  st.name, st.surname, h.name 
FROM students st, students_hobbies sth, hobbies h 
WHERE 
  st.id = sth.id AND 
  sth.hobby_id = h.id AND 
  sth.date_finish IS NULL


```
![Задание 1](/Multi/ex_1.png)

-----------------------
<br><br>

2. Вывести информацию о студенте, занимающимся хобби самое продолжительное время

```SQL
SELECT st.id, st.name, st.surname, st.n_group,
	CASE
		WHEN st_h.date_finish IS NULL THEN (CURRENT_DATE-st_h.date_start)
		WHEN st_h.date_finish IS NOT NULL THEN st_h.date_finish - st_h.date_start
	END as hobby_day

FROM students_hobbies st_h, students st
WHERE st_h.students_id = st.id
ORDER BY hobby_day DESC
LIMIT 1


```
![Задание 2](/Multi/ex_2.png)

-----------------------
<br><br>

3. Вывести имя, фамилию, номер зачетки и дату рождения для студентов, средний балл которых выше среднего, а сумма риска всех хобби, которыми он занимается в данный момент, больше 5

```SQL
SELECT st.name, st.surname, st.id, st.date_birth
	FROM students st,
  	(SELECT st_h.id, SUM(h.risk)
		FROM hobbies h
		INNER JOIN students_hobbies st_h
		ON h.id = st_h.hobby_id
		GROUP BY st_h.id
		HAVING SUM(h.risk) > 5) h_risk
WHERE 
	st.score > (SELECT ROUND(AVG(st.score),2) FROM students st) 
	AND h_risk.id = st.id


```
![Задание 3](/Multi/ex_3.png)

-----------------------
<br><br>

4. Вывести фамилию, имя, зачетку, дату рождения, название хобби и длительность в месяцах, для всех завершенных хобби.

```SQL
SELECT st.id, st.name, st.surname, st.date_birth, h.name, st_h.do_time FROM students st 
INNER JOIN 
  (SELECT 
    st_h.id, 
    st_h.hobby_id,
    st_h.date_finish - st_h.date_start do_time 
  FROM students_hobbies st_h 
  WHERE st_h.date_finish - st_h.date_start IS NOT NULL) st_h 
ON st.id = st_h.id
INNER JOIN hobbies h
ON st_h.hobby_id = h.id

```
![Задание 4](/Multi/ex_4.png)

-----------------------
<br><br>

5. Вывести фамилию, имя, зачетку, дату рождения студентов, которым исполнилось N полных лет на текущую дату, и которые имеют более 1 действующего хобби.

```SQL
SELECT st.surname, st.name, st.id, st.date_birth 
FROM students st, (
	SELECT st_h.students_id, COUNT(st_h.hobby_id)
	FROM students_hobbies st_h
	GROUP BY st_h.students_id 
	HAVING COUNT(st_h.hobby_id) > 1
) pod
WHERE st.id = pod.students_id AND extract(year from age(CURRENT_TIMESTAMP,date_birth)) > 19


```
![Задание 5](/Multi/ex_5.png)

-----------------------
<br><br>

6. Найти средний балл в каждой группе, учитывая только баллы студентов, которые имеют хотя бы одно действующее хобби.

```SQL
SELECT st.surname, st.name, st.id, st.date_birth 
FROM students st, (
	SELECT st_h.students_id, COUNT(st_h.hobby_id)
	FROM students_hobbies st_h
	GROUP BY st_h.students_id 
	HAVING COUNT(st_h.hobby_id) > 1
) pod
WHERE st.id = pod.students_id AND extract(year from age(CURRENT_TIMESTAMP,date_birth)) > 19


```
![Задание 5](/Multi/ex_5.png)

-----------------------
<br><br>

7.

```SQL
SELECT st.surname, st.name, st.id, st.date_birth 
FROM students st, (
	SELECT st_h.students_id, COUNT(st_h.hobby_id)
	FROM students_hobbies st_h
	GROUP BY st_h.students_id 
	HAVING COUNT(st_h.hobby_id) > 1
) pod
WHERE st.id = pod.students_id AND extract(year from age(CURRENT_TIMESTAMP,date_birth)) > 19


```
![Задание 5](/Multi/ex_5.png)

-----------------------
<br><br>

8.

```SQL
SELECT st.surname, st.name, st.id, st.date_birth 
FROM students st, (
	SELECT st_h.students_id, COUNT(st_h.hobby_id)
	FROM students_hobbies st_h
	GROUP BY st_h.students_id 
	HAVING COUNT(st_h.hobby_id) > 1
) pod
WHERE st.id = pod.students_id AND extract(year from age(CURRENT_TIMESTAMP,date_birth)) > 19


```
![Задание 5](/Multi/ex_5.png)

-----------------------
<br><br>

9.

```SQL
SELECT st.surname, st.name, st.id, st.date_birth 
FROM students st, (
	SELECT st_h.students_id, COUNT(st_h.hobby_id)
	FROM students_hobbies st_h
	GROUP BY st_h.students_id 
	HAVING COUNT(st_h.hobby_id) > 1
) pod
WHERE st.id = pod.students_id AND extract(year from age(CURRENT_TIMESTAMP,date_birth)) > 19


```
![Задание 5](/Multi/ex_5.png)

-----------------------
<br><br>