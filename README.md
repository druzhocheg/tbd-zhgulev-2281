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
![](/Multi/ex_5.png)
-----------------------
<br><br>

6. Найти средний балл в каждой группе, учитывая только баллы студентов, которые имеют хотя бы одно действующее хобби.

```SQL
SELECT st.n_group, ROUND(AVG(st.score),2)
FROM students st
INNER JOIN
  (SELECT *
    FROM students_hobbies st_h
    WHERE st_h.date_finish IS NULL) cr_h
ON st.id = cr_h.id
GROUP BY st.n_group


```
![](/Multi/ex_6.png)
-----------------------
<br><br>

7. Найти название, риск, длительность в месяцах самого продолжительного хобби из действующих, указав номер зачетки студента.

```SQL
SELECT h.name, h.risk, pod.months, pod.id
FROM hobbies h
INNER JOIN (
	SELECT pod.hobby_id , st.id, pod.months
	FROM students st, (
		SELECT st_h.hobby_id ,st_h.students_id, 12 * extract(year from age(CURRENT_TIMESTAMP, st_h.date_start)) as months
		FROM students_hobbies st_h
		WHERE st_h.date_finish IS NULL
		ORDER BY months DESC
		LIMIT 1
	) pod
	WHERE st.id = pod.students_id
) pod on pod.hobby_id = h.id


```
![](/Multi/ex_7.png)

-----------------------
<br><br>

8. Найти все хобби, которыми увлекаются студенты, имеющие максимальный балл.


```SQL
SELECT h.* FROM students st
INNER JOIN students_hobbies st_h
ON st_h.id = st.id
INNER JOIN hobbies h
ON st_h.hobby_id = h.id
WHERE 
  st.score = (SELECT st.score
  FROM students st
  GROUP BY st.score
  ORDER BY st.score DESC
  LIMIT 1)

```
![](/Multi/ex_8.png)

-----------------------
<br><br>

9. Найти все действующие хобби, которыми увлекаются троечники 2-го курса.

```SQL
SELECT h.name FROM students st
INNER JOIN students_hobbies st_h
ON st_h.id = st.id
INNER JOIN hobbies h
ON st_h.hobby_id = h.id
WHERE 
  cast(st.n_group as varchar) LIKE '2%' AND 
  st.score >= 2.5 AND 
  st.score <= 4.0 AND
  st_h.date_finish IS NULL

```
![](/Multi/ex_9.png)

-----------------------
<br><br>

10. Найти номера курсов, на которых более 50% студентов имеют более одного действующего хобби.

```SQL

```


-----------------------
<br><br>

11. Вывести номера групп, в которых не менее 60% студентов имеют балл не ниже 4

```SQL
SELECT sub.n_group
FROM
  (SELECT 
    st.n_group, 
    COUNT(st.id) total_count, 
    COUNT(st.score) FILTER (WHERE st.score > 4) above_score_count
  FROM students st
  GROUP BY st.n_group) sub
WHERE sub.total_count*0.6 < above_score_count
```
![](/Multi/ex_11.png)

-----------------------
<br><br>

12.Для каждого курса подсчитать количество различных действующих хобби на курсе.

```SQL
SELECT LEFT(st.n_group::VARCHAR,1) course, COUNT(DISTINCT h.id) count_dif
FROM students st
INNER JOIN students_hobbies st_h
ON st_h.id = st.id
INNER JOIN hobbies h
ON st_h.hobby_id = h.id
GROUP BY LEFT(st.n_group::VARCHAR,1)
```
![](/Multi/ex_12.png)

-----------------------
<br><br>

13. Вывести номер зачётки, фамилию и имя, дату рождения и номер курса для всех отличников, не имеющих хобби. Отсортировать данные по возрастанию в пределах курса по убыванию даты рождения.

```SQL
SELECT st.id, st.name, st.surname, st.date_birth, 
	LEFT(st.n_group::VARCHAR,1) course
FROM students st
WHERE 
  st.score >= 4.5 AND
  st.id IN
    (SELECT st_h.id
    FROM students_hobbies st_h
    GROUP BY st_h.id
    HAVING COUNT(st_h.date_finish) = COUNT(st_h.date_start))
ORDER BY
  LEFT(st.n_group::VARCHAR,1),
  st.date_birth DESC

```
![](/Multi/ex_13.png)

-----------------------
<br><br>

14. Создать представление, в котором отображается вся информация о студентах, которые продолжают заниматься хобби в данный момент и занимаются им как минимум 5 лет.

```SQL
CREATE OR REPLACE VIEW hobby_more5years AS
SELECT st.*
FROM students st
INNER JOIN students_hobbies st_h
ON st_h.id = st.id
WHERE
  st_h.date_finish IS NULL AND
  EXTRACT(YEAR FROM AGE(NOW(),st_h.date_start)) >= 5

```
![](/Multi/ex_14.png)

-----------------------
<br><br>

15. Для каждого хобби вывести количество людей, которые им занимаются.

```SQL
SELECT h.name, COUNT(DISTINCT st_h.id) students
FROM hobbies h
INNER JOIN students_hobbies st_h
ON h.id = st_h.hobby_id
GROUP BY h.name

```
![](/Multi/ex_15.png)

-----------------------
<br><br>

16. Вывести ИД самого популярного хобби.

```SQL
SELECT st_h.hobby_id
FROM students_hobbies st_h
GROUP BY hobby_id
ORDER BY COUNT(st_h.students_id) DESC
LIMIT 1

```
![](/Multi/ex_16.png)

-----------------------
<br><br>

17. Вывести всю информацию о студентах, занимающихся самым популярным хобби.

```SQL
SELECT st.*
FROM students st
INNER JOIN students_hobbies st_h
ON st.id = st_h.id
WHERE
  st_h.hobby_id = (SELECT st_h.hobby_id
    FROM students_hobbies st_h
    GROUP BY st_h.hobby_id
    ORDER BY COUNT(st_h.id) DESC
    LIMIT 1) AND
  st_h.date_finish IS NULL

```
![](/Multi/ex_17.png)

-----------------------
<br><br>

18. Вывести ИД 3х хобби с максимальным риском.

```SQL
CREATE OR REPLACE VIEW hobby_top3 AS
SELECT h.id
FROM hobbies h
ORDER BY h.risk DESC
LIMIT 3

```
![](/Multi/ex_18.png)

-----------------------
<br><br>

19. Вывести 10 студентов, которые занимаются одним (или несколькими) хобби самое продолжительно время.

```SQL
SELECT st_h.students_id
FROM students_hobbies st_h
ORDER BY age(st_h.date_finish, st_h.date_start) DESC
LIMIT 10
```
![](/Multi/ex_19.png)

-----------------------
<br><br>

20. Вывести номера групп (без повторений), в которых учатся студенты из предыдущего запроса.

```SQL
SELECT DISTINCT st.n_group
FROM students st, (
	SELECT st_h.students_id
	FROM students_hobbies st_h
	ORDER BY age(st_h.date_finish, st_h.date_start) DESC
	LIMIT 10
) res
WHERE st.id = res.students_id
```
![](/Multi/ex_20.png)

-----------------------
<br><br>

21. Создать представление, которое выводит номер зачетки, имя и фамилию студентов, отсортированных по убыванию среднего балла.

```SQL
CREATE OR REPLACE VIEW students_top AS
SELECT st.id, st.name, st.surname
FROM students st
ORDER BY st.score DESC
```
![](/Multi/ex_21.png)

-----------------------
<br><br>

22. Представление: найти каждое популярное хобби на каждом курсе.

```SQL
CREATE OR REPLACE VIEW corse_top_pop AS
SELECT DISTINCT ON (1) substr(st.n_group::varchar,1,1), COUNT(st_h.hobby_id) hobby_count, st_h.hobby_id
FROM students st
INNER JOIN students_hobbies st_h ON st_h.students_id = st.id
GROUP BY substr(st.n_group::varchar,1,1), st_h.hobby_id
ORDER BY substr(st.n_group::varchar,1,1) DESC, hobby_count DESC
```
![](/Multi/ex_22.png)

-----------------------
<br><br>

23. Представление: найти хобби с максимальным риском среди самых популярных хобби на 2 курсе.

```SQL
CREATE OR REPLACE VIEW top_risk_h_2ndcourse AS
SELECT *
FROM hobbies h
WHERE h.id
IN
  (SELECT h.id
  FROM students st
  INNER JOIN students_hobbies st_h
  ON st.id = st_h.id
  INNER JOIN hobbies h
  ON st_h.hobby_id = h.id
  WHERE LEFT(st.n_group::VARCHAR,1) = '2'
  GROUP BY h.id
  ORDER BY COUNT(h.id))
ORDER BY h.risk DESC
LIMIT 1
```
![](/Multi/ex_23.png)

-----------------------
<br><br>

24. Представление: для каждого курса подсчитать количество студентов на курсе и количество отличников.

```SQL
CREATE OR REPLACE VIEW count_ideal_st AS
SELECT 
  LEFT(st.n_group::VARCHAR,1) course, 
  COUNT(st.id) total, 
  COUNT(st.id) FILTER (WHERE st.score >= 4.5) ideal_student
FROM students st
GROUP BY LEFT(st.n_group::VARCHAR,1)
```
![](/Multi/ex_24.png)

-----------------------
<br><br>

25. Представление: самое популярное хобби среди всех студентов.

```SQL
CREATE OR REPLACE VIEW top_h AS
SELECT *
FROM hobbies h
WHERE 
  h.id = 
    (SELECT h.id
    FROM students st
    INNER JOIN students_hobbies st_h
    ON st.id = st_h.id
    INNER JOIN hobbies h
    ON st_h.hobby_id = h.id
    GROUP BY h.id
    ORDER BY COUNT(h.id) DESC
    LIMIT 1)
```
![](/Multi/ex_25.png)

-----------------------
<br><br>