# tbd-zhgulev-2281
# Однотабличные запросы
1. Вывести всеми возможными способами имена и фамилии студентов, средний балл которых от 4 до 4.5
```SQL
SELECT * FROM students
WHERE score >= 4 and score <= 4.5
```
![Задание 1](/%D0%9E%D0%B4%D0%BD%D0%BE%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5/ex_1.png)

-----------------------
<br><br>

2. Познакомиться с функцией CAST. Вывести при помощи неё студентов заданного курса (использовать Like)
```SQL
SELECT * FROM students st
WHERE CAST(st.n_group AS varchar) LIKE '2%'
```
![Задание 2](/%D0%9E%D0%B4%D0%BD%D0%BE%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5/ex_2.png)

-----------------------
<br><br>

3. Вывести всех студентов, отсортировать по убыванию номера группы и имени от а до я
```SQL
SELECT * FROM students st
ORDER BY st.n_group DESC, st.name ASC
```
![Задание 3](/%D0%9E%D0%B4%D0%BD%D0%BE%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5/ex_3.png)

-----------------------
<br><br>

4. Вывести студентов, средний балл которых больше 4 и отсортировать по баллу от большего к меньшему
```SQL
SELECT st.name, st.surname, st.n_group, st.score
FROM students st
WHERE st.score >= 4
ORDER BY score DESC
```
![Задание 4](/%D0%9E%D0%B4%D0%BD%D0%BE%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5/ex_4.png)

-----------------------
<br><br>

5. Вывести на экран название и риск футбола и хоккея
```SQL
SELECT * FROM hobbies  hb
WHERE hb.name = 'Хоккей' OR hb.name = 'Футбол'
```

![Задание 5](/%D0%9E%D0%B4%D0%BD%D0%BE%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5/ex_5.png)

-----------------------
<br><br>


6. Вывести id хобби и id студента которые начали заниматься хобби между двумя заданными датами (выбрать самим) и студенты должны до сих пор заниматься хобби

```SQL
SELECT * FROM students_hobbies st
WHERE (CAST(st.date_start as varchar) LIKE '201%' or
	CAST(st.date_start as varchar) LIKE '2020%') and
	date_finish is null
```

![Задание 6](/%D0%9E%D0%B4%D0%BD%D0%BE%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5/ex_6.png)

-----------------------
<br><br>


7. Вывести студентов, средний балл которых больше 4.5 и отсортировать по баллу от большего к меньшему
```SQL
SELECT st.name, st.surname, st.n_group, st.score
FROM students st
WHERE st.score > 4.5
ORDER BY st.score DESC
```

![Задание 7](/%D0%9E%D0%B4%D0%BD%D0%BE%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5/ex_7.png)

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

![Задание 8](/%D0%9E%D0%B4%D0%BD%D0%BE%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5/ex_8.png)

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

![Задание 9](/%D0%9E%D0%B4%D0%BD%D0%BE%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5/ex_9.png)

-----------------------
<br><br>

10. Вывести 3 хобби с максимальным риском
```SQL
SELECT * FROM hobbies
ORDER BY risk DESC
LIMIT 3
```

![Задание 10](/%D0%9E%D0%B4%D0%BD%D0%BE%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5/ex_10.png)

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

![Задание 1](/%D0%93%D1%80%D1%83%D0%BF%D0%BF%D0%BE%D0%B2%D1%8B%D0%B5%20%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8/ex_1.png)

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

![Задание 2](/%D0%93%D1%80%D1%83%D0%BF%D0%BF%D0%BE%D0%B2%D1%8B%D0%B5%20%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8/ex_2.png)

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

![Задание 3](/%D0%93%D1%80%D1%83%D0%BF%D0%BF%D0%BE%D0%B2%D1%8B%D0%B5%20%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8/ex_3.png)

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

![Задание 4](/%D0%93%D1%80%D1%83%D0%BF%D0%BF%D0%BE%D0%B2%D1%8B%D0%B5%20%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8/ex_4.png)

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

![Задание 5](/%D0%93%D1%80%D1%83%D0%BF%D0%BF%D0%BE%D0%B2%D1%8B%D0%B5%20%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8/ex_5.png)

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

![Задание 6](/%D0%93%D1%80%D1%83%D0%BF%D0%BF%D0%BE%D0%B2%D1%8B%D0%B5%20%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8/ex_6.png)

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

![Задание 7](/%D0%93%D1%80%D1%83%D0%BF%D0%BF%D0%BE%D0%B2%D1%8B%D0%B5%20%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8/ex_7.png)

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

![Задание 8](/%D0%93%D1%80%D1%83%D0%BF%D0%BF%D0%BE%D0%B2%D1%8B%D0%B5%20%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8/ex_8.png)

-----------------------
<br><br>