# Решение заданий из тренажера [SQL Academy](https://sql-academy.org/ru)

1. Вывести имена всех людей, которые есть в базе данных
   авиакомпаний [(сайт)](https://sql-academy.org/ru/trainer/tasks/1)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM Passenger
```

</details>

2. Вывести названия всеx авиакомпаний [(сайт)](https://sql-academy.org/ru/trainer/tasks/2)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM Company;
```

</details>

3. Вывести все рейсы, совершенные из Москвы [(сайт)](https://sql-academy.org/ru/trainer/tasks/3)

<details>
  <summary>Решение</summary>

```mysql
SELECT *
FROM Trip
WHERE town_from = 'Moscow'
```

</details>

4. Вывести имена людей, которые заканчиваются на "man" [(сайт)](https://sql-academy.org/ru/trainer/tasks/4)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM Passenger
WHERE name REGEXP 'man$'
```

</details>

5. Вывести количество рейсов, совершенных на TU-134 [(сайт)](https://sql-academy.org/ru/trainer/tasks/5)

<details>
  <summary>Решение</summary>

```mysql
SELECT COUNT(plane) AS COUNT
FROM Trip
WHERE plane REGEXP 'TU-134'
```

</details>

6. Какие компании совершали перелеты на Boeing [(сайт)](https://sql-academy.org/ru/trainer/tasks/6)

<details>
  <summary>Решение</summary>

```mysql
SELECT DISTINCT name
FROM Company
	JOIN Trip ON Trip.company = Company.id
WHERE plane = 'Boeing'
```

</details>

7. Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)
   [(сайт)](https://sql-academy.org/ru/trainer/tasks/7)

<details>
  <summary>Решение</summary>

```mysql
SELECT DISTINCT plane
FROM Trip
WHERE town_from = 'Moscow'
```

</details>

8. В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?
   [(сайт)](https://sql-academy.org/ru/trainer/tasks/8)

<details>
  <summary>Решение</summary>

```mysql
SELECT town_to,
	TIMEDIFF(time_in, time_out) AS flight_time
FROM Trip
WHERE town_from = 'Paris'
```

</details>

9. Какие компании организуют перелеты из Владивостока (Vladivostok)?
   [(сайт)](https://sql-academy.org/ru/trainer/tasks/9)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM Company
	JOIN Trip ON Trip.company = Company.id
WHERE town_from = "Vladivostok"
```

</details>

10. Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г. [(сайт)](https://sql-academy.org/ru/trainer/tasks/10)

<details>
  <summary>Решение</summary>

```mysql
SELECT *
FROM Trip
WHERE DATE(time_out) = '1900-01-01'
	AND TIME_FORMAT(time_out, '%H:%i') BETWEEN '10:00' AND '14:00'
```

</details>

11. Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.
    [(сайт)](https://sql-academy.org/ru/trainer/tasks/11)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM Passenger
WHERE LENGTH(name) = (
		SELECT MAX(LENGTH(name))
		FROM Passenger
	)
```

</details>

12. Вывести id и количество пассажиров для всех прошедших полётов [(сайт)](https://sql-academy.org/ru/trainer/tasks/12)

<details>
  <summary>Решение</summary>

```mysql
SELECT trip,
	COUNT(*) AS COUNT
FROM Passenger
	JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
GROUP BY trip
```

</details>

13. Вывести имена людей, у которых есть полный тёзка среди пассажиров
    [(сайт)](https://sql-academy.org/ru/trainer/tasks/13)

<details>
  <summary>Решение</summary>

```mysql
SELECT name
FROM Passenger
GROUP BY name
HAVING COUNT(*) > 1
```

</details>

14. В какие города летал Bruce Willis [(сайт)](https://sql-academy.org/ru/trainer/tasks/14)

<details>
  <summary>Решение</summary>

```mysql
SELECT town_to
FROM Passenger
	JOIN Pass_in_trip ON Pass_in_trip.passenger = Passenger.id
	JOIN Trip ON Trip.id = Pass_in_trip.trip
WHERE name = 'Bruce Willis'
```

</details>

15. Выведите дату и время прилёта пассажира Стив Мартин (Steve Martin) в Лондон (London) 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/15)

<details>
  <summary>Решение</summary>

```mysql
SELECT time_in
FROM Passenger
	JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
	JOIN trip ON trip.id = Pass_in_trip.trip
WHERE name = 'Steve Martin'
	and town_to = 'London';
```

</details>

16. Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, 
совершивших хотя бы 1 полет. [(сайт)](https://sql-academy.org/ru/trainer/tasks/16)

<details>
  <summary>Решение</summary>

```mysql
SELECT name,
	COUNT(*) AS COUNT
FROM Passenger
	JOIN Pass_in_trip ON Pass_in_trip.passenger = Passenger.id
GROUP BY name
ORDER BY COUNT DESC,
	name
```

</details>

17. Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов 
семьи, которые ничего не потратили. [(сайт)](https://sql-academy.org/ru/trainer/tasks/17)   

<details>
  <summary>Решение</summary>

```mysql
SELECT member_name,
	status,
	SUM(unit_price * amount) AS costs
FROM FamilyMembers
	JOIN Payments ON Payments.family_member = FamilyMembers.member_id
WHERE YEAR(date) = "2005"
GROUP by member_name,
	status
```

</details>

18. Узнать, кто старше всех в семьe [(сайт)](https://sql-academy.org/ru/trainer/tasks/18) 

<details>
  <summary>Решение</summary>

```mysql
SELECT member_name
FROM FamilyMembers
ORDER BY birthday
LIMIT 1
```

</details>

19. Определить, кто из членов семьи покупал картошку (potato) [(сайт)](https://sql-academy.org/ru/trainer/tasks/19)

<details>
  <summary>Решение</summary>

```mysql
SELECT status
FROM FamilyMembers
	JOIN Payments ON Payments.family_member = FamilyMembers.member_id
	JOIN Goods ON Payments.good = Goods.good_id
WHERE good_name = 'potato'
GROUP BY status
```

</details>

20. Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/20)

<details>
  <summary>Решение</summary>

```mysql
SELECT status,
	member_name,
	SUM(amount * unit_price) as costs
FROM FamilyMembers
	JOIN Payments ON Payments.family_member = FamilyMembers.member_id
	JOIN Goods ON Payments.good = Goods.good_id
	JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type
WHERE good_type_name = 'entertainment'
GROUP BY status,
	member_name
```

</details>

21. Определить товары, которые покупали более 1 раза [(сайт)](https://sql-academy.org/ru/trainer/tasks/21)

<details>
  <summary>Решение</summary>

```mysql
SELECT good_name
FROM Goods
	JOIN Payments ON Payments.good = Goods.good_id
GROUP BY good
HAVING COUNT(*) > 1
```

</details>

22. Найти имена всех матерей (mother) [(сайт)](https://sql-academy.org/ru/trainer/tasks/22)

<details>
  <summary>Решение</summary>

```mysql
select member_name
from FamilyMembers
where status = 'mother'
```

</details>

23. Найдите самый дорогой деликатес (delicacies) и выведите его цену  
[(сайт)](https://sql-academy.org/ru/trainer/tasks/23)

<details>
  <summary>Решение</summary>

```mysql
SELECT good_name,
	unit_price
FROM Goods
	JOIN Payments ON Payments.good = Goods.good_id
	JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type
WHERE good_type_name = 'delicacies'
ORDER BY unit_price DESC
LIMIT 1
```

</details>

24. Определить кто и сколько потратил в июне 2005 [(сайт)](https://sql-academy.org/ru/trainer/tasks/24)

<details>
  <summary>Решение</summary>

```mysql
SELECT member_name,
	sum(amount * unit_price) AS costs
FROM FamilyMembers
	JOIN Payments ON Payments.family_member = FamilyMembers.member_id
WHERE YEAR(date) = '2005'
	AND MONTH(date) = '6'
GROUP BY member_name
```

</details>

25. Определить, какие товары не покупались в 2005 году [(сайт)](https://sql-academy.org/ru/trainer/tasks/25)

<details>
  <summary>Решение</summary>

```mysql
SELECT good_name
FROM Goods
WHERE good_id NOT IN (
		SELECT good
		FROM Payments
		WHERE YEAR(date) = 2005
	)
```

</details>

26. Определить группы товаров, которые не приобретались в 2005 году 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/26)

<details>
  <summary>Решение</summary>

```mysql
SELECT good_type_name
FROM GoodTypes
WHERE good_type_id NOT IN (
		SELECT type
		FROM Goods
			JOIN Payments ON Payments.good = Goods.good_id
		WHERE YEAR(date) = 2005
	)
```

</details>

27. Узнать, сколько потрачено на каждую из групп товаров в 2005 году. Вывести название группы и сумму 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/27)

<details>
  <summary>Решение</summary>

```mysql
SELECT good_type_name,
	sum(amount * unit_price) AS costs
FROM GoodTypes
	JOIN Goods ON GoodTypes.good_type_id = Goods.type
	JOIN Payments ON Payments.good = Goods.good_id
WHERE YEAR(date) = 2005
GROUP BY good_type_name
```

</details>

28. Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow) ? 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/28)  

<details>
  <summary>Решение</summary>

```mysql
SELECT COUNT(*) AS COUNT
FROM Trip
WHERE town_from = 'Rostov'
	AND town_to = 'Moscow'
```

</details>

29. Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/29)  

<details>
  <summary>Решение</summary>

```mysql
SELECT DISTINCT name
FROM Passenger
	JOIN Pass_in_trip ON Pass_in_trip.passenger = Passenger.id
	JOIN Trip ON Trip.id = Pass_in_trip.trip
WHERE plane = 'TU-134'
	AND town_to = 'Moscow'
```

</details>

30. Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию
нагруженности. [(сайт)](https://sql-academy.org/ru/trainer/tasks/30)  

<details>
  <summary>Решение</summary>

```mysql
SELECT trip,
	COUNT(passenger) AS COUNT
FROM Pass_in_trip
GROUP BY trip
ORDER BY COUNT DESC
```

</details>

31. Вывести всех членов семьи с фамилией Quincey. [(сайт)](https://sql-academy.org/ru/trainer/tasks/31) 

<details>
  <summary>Решение</summary>

```mysql
SELECT *
FROM FamilyMembers
WHERE member_name REGEXP 'Quincey$'
```

</details>

32. Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.
[(сайт)](https://sql-academy.org/ru/trainer/tasks/32) 

<details>
  <summary>Решение</summary>

```mysql
SELECT FLOOR(
		AVG(
			TIMESTAMPDIFF(YEAR, birthday, CURDATE())
		)
	) AS age
FROM FamilyMembers
```

</details>

33. Найдите среднюю стоимость икры. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black 
caviar). [(сайт)](https://sql-academy.org/ru/trainer/tasks/33) 

<details>
  <summary>Решение</summary>

```mysql
SELECT AVG(Payments.unit_price) AS cost
FROM Payments
	JOIN Goods ON Goods.good_id = Payments.good
WHERE Goods.good_name IN('red caviar', 'black caviar')
```

</details>

34. Сколько всего 10-ых классов [(сайт)](https://sql-academy.org/ru/trainer/tasks/34) 

<details>
  <summary>Решение</summary>

```mysql
SELECT COUNT(name) AS COUNT
FROM Class
where name REGEXP '^10'
```

</details>

35. Сколько различных кабинетов школы использовались 2.09.2019 в образовательных целях ? 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/35)

<details>
  <summary>Решение</summary>

```mysql
SELECT COUNT(DISTINCT classroom) AS COUNT
FROM Schedule
WHERE DATE_FORMAT(date, '%e.%m.%Y') = '2.09.2019'
```

</details>

36. Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)? 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/36)

<details>
  <summary>Решение</summary>

```mysql
SELECT *
FROM Student
WHERE address REGEXP '^ul. Pushkina'
```

</details>

37. Сколько лет самому молодому обучающемуся ? [(сайт)](https://sql-academy.org/ru/trainer/tasks/37)

<details>
  <summary>Решение</summary>

```mysql
SELECT TIMESTAMPDIFF(YEAR, Max(birthday), NOW()) AS year
FROM Student
```

</details>

38. Сколько Анн (Anna) учится в школе ? [(сайт)](https://sql-academy.org/ru/trainer/tasks/38)

<details>
  <summary>Решение</summary>

```mysql
SELECT COUNT(*) AS COUNT
FROM Student
WHERE first_name = 'Anna'
```

</details>

39. Сколько обучающихся в 10 B классе ? [(сайт)](https://sql-academy.org/ru/trainer/tasks/39)

<details>
  <summary>Решение</summary>

```mysql
SELECT COUNT(*) AS COUNT
FROM Student_in_class sc
	JOIN Class cl ON cl.id = sc.class
WHERE cl.name = '10 B'
```

</details>

40. Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.) ? 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/40)

<details>
  <summary>Решение</summary>

```mysql
SELECT name AS subjects
FROM Subject sj
	JOIN Schedule sc ON sj.id = sc.subject
	JOIN Teacher tc ON tc.id = sc.teacher
WHERE last_name = 'Romashkin'
	AND first_name LIKE 'P%'
	AND middle_name LIKE 'P%'
```

</details>

41. Во сколько начинается 4-ый учебный предмет по расписанию ? [(сайт)](https://sql-academy.org/ru/trainer/tasks/41)

<details>
  <summary>Решение</summary>

```mysql
SELECT start_pair
FROM Timepair
WHERE id = '4'
```

</details>

42. Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет?
[(сайт)](https://sql-academy.org/ru/trainer/tasks/42)

<details>
  <summary>Решение</summary>

```mysql
SELECT TIMEDIFF(Max(end_pair), MIN(start_pair)) AS time
FROM Timepair
WHERE id BETWEEN 2 AND 4
```

</details>

43. Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Отсортируйте преподавателей 
по фамилии в алфавитном порядке. [(сайт)](https://sql-academy.org/ru/trainer/tasks/43)

<details>
  <summary>Решение</summary>

```mysql
SELECT last_name
FROM Teacher tc
	JOIN Schedule sc ON sc.teacher = tc.id
	JOIN Subject sj ON sj.id = sc.subject
WHERE name = 'Physical Culture'
ORDER BY last_name
```

</details>

44. Найдите максимальный возраст (колич. лет) среди обучающихся 10 классов ? 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/44)

<details>
  <summary>Решение</summary>

```mysql
SELECT TIMESTAMPDIFF(YEAR, MIN(birthday), NOW()) AS max_year
FROM Student sd
	JOIN Student_in_class sc ON sc.student = sd.id
	JOIN Class ON Class.id = sc.class
WHERE name LIKE '10%'
```

</details>

45. Какие кабинеты чаще всего использовались для проведения занятий? Выведите те, которые использовались максимальное 
количество раз. [(сайт)](https://sql-academy.org/ru/trainer/tasks/45)

<details>
  <summary>Решение</summary>

```mysql
SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING COUNT(classroom) = (
		SELECT COUNT(*) AS COUNT
		FROM Schedule
		GROUP BY classroom
		ORDER BY COUNT DESC
		LIMIT 1
	)
```

</details>

46. В каких классах введет занятия преподаватель "Krauze" ? [(сайт)](https://sql-academy.org/ru/trainer/tasks/46)

<details>
  <summary>Решение</summary>

```mysql
SELECT DISTINCT name
FROM Class
	JOIN Schedule sd ON sd.class = Class.id
	JOIN Teacher ON Teacher.id = sd.teacher
WHERE last_name = 'Krauze'
```

</details>

47. Сколько занятий провел Krauze 30 августа 2019 г.? [(сайт)](https://sql-academy.org/ru/trainer/tasks/47)

<details>
  <summary>Решение</summary>

```mysql
SELECT COUNT(class) AS COUNT
FROM Schedule
	JOIN Teacher ON Teacher.id = Schedule.teacher
WHERE last_name = 'Krauze'
	AND DATE_FORMAT(date, '%e.%m.%Y') = '30.08.2019'
```

</details>

48. Выведите заполненность классов в порядке убывания [(сайт)](https://sql-academy.org/ru/trainer/tasks/48)

<details>
  <summary>Решение</summary>

```mysql
SELECT name,
	COUNT(name) AS COUNT
FROM Class
	JOIN Student_in_class sc ON sc.class = Class.id
GROUP BY name
ORDER BY COUNT(name) DESC
```

</details>

49. Какой процент обучающихся учится в "10 A" классе? Выведите ответ в диапазоне от 0 до 100 без округления, например, 
96.0201. [(сайт)](https://sql-academy.org/ru/trainer/tasks/49)

<details>
  <summary>Решение</summary>

```mysql
SELECT COUNT(*) * 100 / (
		SELECT COUNT(*)
		FROM Student_in_class
	) AS percent
FROM Student_in_class sc
	JOIN Class cs ON cs.id = sc.class
WHERE name = '10 A'
```

</details>

50. Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону. 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/50)

<details>
  <summary>Решение</summary>

```mysql
SELECT FLOOR(
		COUNT(*) * 100 / (
			SELECT COUNT(*)
			FROM Student
		)
	) AS percent
FROM Student
WHERE YEAR(birthday) = 2000
```

</details>

51. Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods). 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/51)

<details>
  <summary>Решение</summary>

```mysql
INSERT INTO Goods
SELECT COUNT(*) + 1,
	'Cheese',
	(
		SELECT DISTINCT good_type_id
		FROM GoodTypes
		WHERE good_type_name = 'food'
	)
FROM Goods
```

</details>

52. Добавьте в список типов товаров (GoodTypes) новый тип "auto". [(сайт)](https://sql-academy.org/ru/trainer/tasks/52)

<details>
  <summary>Решение</summary>

```mysql
INSERT INTO GoodTypes
SELECT COUNT(*) + 1,
	'auto'
FROM GoodTypes
```

</details>

53. Измените имя "Andie Quincey" на новое "Andie Anthony". [(сайт)](https://sql-academy.org/ru/trainer/tasks/53)

<details>
  <summary>Решение</summary>

```mysql
UPDATE FamilyMembers
set member_name = 'Andie Anthony'
where member_name = 'Andie Quincey'
```

</details>

54. Удалить всех членов семьи с фамилией "Quincey". [(сайт)](https://sql-academy.org/ru/trainer/tasks/54)

<details>
  <summary>Решение</summary>

```mysql
DELETE FROM FamilyMembers
WHERE member_name LIKE '%Quincey'
```

</details>

55. Удалить компании, совершившие наименьшее количество рейсов. [(сайт)](https://sql-academy.org/ru/trainer/tasks/55)

<details>
  <summary>Решение</summary>

```mysql
DELETE FROM Company
WHERE id IN (
		SELECT company
		FROM Trip
		GROUP BY company
		HAVING COUNT(*) = (
				SELECT COUNT(*) AS COUNT
				FROM Trip
				GROUP BY company
				ORDER BY COUNT
				LIMIT 1
			)
	)
```

</details>

56. Удалить все перелеты, совершенные из Москвы (Moscow). [(сайт)](https://sql-academy.org/ru/trainer/tasks/56)

<details>
  <summary>Решение</summary>

```mysql
DELETE FROM Trip
WHERE town_from = 'Moscow'
```

</details>

57. Перенести расписание всех занятий на 30 мин. вперед. [(сайт)](https://sql-academy.org/ru/trainer/tasks/57)

<details>
  <summary>Решение</summary>

```mysql
UPDATE Timepair
SET start_pair = ADDTIME(start_pair, '00:30:00'),
	end_pair = ADDTIME(end_pair, '00:30:00')
```

</details>

58. Добавить отзыв с рейтингом 5 на жилье, находящиеся по адресу "11218, Friel Place, New York", от имени "George 
Clooney" [(сайт)](https://sql-academy.org/ru/trainer/tasks/58)

<details>
  <summary>Решение</summary>

```mysql
INSERT INTO Reviews
SELECT COUNT(*) + 1,
	(
		SELECT rv.id
		FROM Reservations rv
			JOIN Users ON Users.id = rv.user_id
			JOIN Rooms ON Rooms.id = rv.room_id
		WHERE address = '11218, Friel Place, New York'
			AND name = 'George Clooney'
	),
	5
FROM Reviews
```

</details>

59. Вывести пользователей,указавших Белорусский номер телефона ? Телефонный код Белоруссии +375. 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/59)

<details>
  <summary>Решение</summary>

```mysql
SELECT *
FROM Users
WHERE phone_number LIKE '+375%'
```

</details>

60. Выведите идентификаторы преподавателей, которые хотя бы один раз за всё время преподавали в каждом из одиннадцатых 
классов. [(сайт)](https://sql-academy.org/ru/trainer/tasks/60)

<details>
  <summary>Решение</summary>

```mysql
SELECT teacher
FROM Schedule
	JOIN Class ON Class.id = Schedule.class
WHERE name LIKE '11 %'
GROUP BY teacher
HAVING COUNT(DISTINCT name) = 2
```

</details>

61. Выведите список комнат, которые были зарезервированы хотя бы на одни сутки в 12-ую неделю 2020 года. В данной задаче
в качестве одной недели примите период из семи дней, первый из которых начинается 1 января 2020 года. Например, первая 
неделя года — 1–7 января, а третья — 15–21 января. [(сайт)](https://sql-academy.org/ru/trainer/tasks/61)

<details>
  <summary>Решение</summary>

```mysql
SELECT Rooms.*
FROM Rooms
	JOIN Reservations rv ON rv.room_id = Rooms.id
WHERE YEAR(start_date) = 2020
	AND WEEK(start_date, 1) = 12
```

</details>

62. Вывести в порядке убывания популярности доменные имена 2-го уровня, используемые пользователями для электронной 
почты. Полученный результат необходимо дополнительно отсортировать по возрастанию названий доменных имён. 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/62)

<details>
  <summary>Решение</summary>

```mysql
SELECT substring_index(email, '@', -1) AS domain,
	COUNT(substring_index(email, '@', -1)) AS COUNT
FROM Users
GROUP BY domain
ORDER BY COUNT DESC,
	domain
```

</details>

63. Выведите отсортированный список (по возрастанию) фамилий и имен студентов в виде Фамилия.И. 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/63)

<details>
  <summary>Решение</summary>

```mysql
SELECT CONCAT(last_name, '.', LEFT(first_name, 1), '.') AS name
FROM Student
ORDER BY name
```

</details>

64. Вывести количество бронирований по каждому месяцу каждого года, в которых было хотя бы 1 бронирование. Результат 
отсортируйте в порядке возрастания даты бронирования. [(сайт)](https://sql-academy.org/ru/trainer/tasks/64)

<details>
  <summary>Решение</summary>

```mysql
SELECT YEAR(start_date) AS year,
	MONTH(start_date) AS MONTH,
	COUNT(*) AS amount
FROM Reservations
GROUP BY YEAR(start_date),
	MONTH(start_date)
ORDER BY year,
	MONTH
```

</details>

65. Необходимо вывести рейтинг для комнат, которые хоть раз арендовали, как среднее значение рейтинга отзывов 
округленное до целого вниз. [(сайт)](https://sql-academy.org/ru/trainer/tasks/65)

<details>
  <summary>Решение</summary>

```mysql
SELECT room_id,
	FLOOR(avg(rating)) AS rating
FROM Reviews
	JOIN Reservations ON Reservations.id = Reviews.reservation_id
GROUP BY room_id
```

</details>

66. Вывести список комнат со всеми удобствами (наличие ТВ, интернета, кухни и кондиционера), а также общее количество 
дней и сумму за все дни аренды каждой из таких комнат. [(сайт)](https://sql-academy.org/ru/trainer/tasks/66)

<details>
  <summary>Решение</summary>

```mysql
SELECT home_type,
	address,
	IFNULL(SUM(total / rs.price), 0) AS days,
	IFNULL(SUM(total), 0) AS total_fee
FROM Rooms rm
	LEFT JOIN Reservations rs ON rs.room_id = rm.id
WHERE has_tv = 1
	AND has_internet = 1
	AND has_kitchen = 1
	AND has_air_con = 1
GROUP BY home_type,
	address;
```

</details>

67. Вывести время отлета и время прилета для каждого перелета в формате "ЧЧ:ММ, ДД.ММ - ЧЧ:ММ, ДД.ММ", где часы и минуты
с ведущим нулем, а день и месяц без. [(сайт)](https://sql-academy.org/ru/trainer/tasks/67)

<details>
  <summary>Решение</summary>

```mysql
SELECT CONCAT(
		DATE_FORMAT(time_out, "%H:%i, %e.%c"),
		' - ',
		DATE_FORMAT(time_in, "%H:%i, %e.%c")
	) AS flight_time
FROM Trip
```

</details>

68. Для каждой комнаты, которую снимали как минимум 1 раз, найдите имя человека, снимавшего ее последний раз, и дату, 
когда он выехал [(сайт)](https://sql-academy.org/ru/trainer/tasks/68)

<details>
  <summary>Решение</summary>

```mysql
SELECT nrv.room_id,
	name,
	date AS end_date
FROM (
		SELECT room_id,
			MAX(end_date) AS date
		FROM Reservations
		GROUP BY room_id
	) AS nrv
	JOIN Reservations rv ON rv.room_id = nrv.room_id
	AND nrv.date = rv.end_date
	JOIN Users ON Users.id = rv.user_id
```

</details>

69. Вывести идентификаторы всех владельцев комнат, что размещены на сервисе бронирования жилья и сумму, которую они 
заработали [(сайт)](https://sql-academy.org/ru/trainer/tasks/69)

<details>
  <summary>Решение</summary>

```mysql
SELECT owner_id,
	IFNULL(SUM(total), 0) AS total_earn
FROM Rooms
	LEFT JOIN Reservations rv ON rv.room_id = Rooms.id
GROUP BY owner_id
```

</details>

70. Необходимо категоризовать жилье на economy, comfort, premium по цене соответственно <= 100,
100 < цена < 200, >= 200. В качестве результата вывести таблицу с названием категории и количеством жилья, попадающего в
данную категорию [(сайт)](https://sql-academy.org/ru/trainer/tasks/70)

<details>
  <summary>Решение</summary>

```mysql
SELECT CASE
		WHEN price <= 100 THEN 'economy'
		WHEN 100 < price
		AND price < 200 THEN 'comfort'
		WHEN price >= 200 THEN 'premium'
	END AS category,
	COUNT(*) AS COUNT
FROM Rooms
GROUP BY category
```

</details>

71. Найдите какой процент пользователей, зарегистрированных на сервисе бронирования, хоть раз арендовали или сдавали в 
аренду жилье. Результат округлите до сотых. [(сайт)](https://sql-academy.org/ru/trainer/tasks/71)

<details>
  <summary>Решение</summary>

```mysql
SELECT ROUND(
		(
			SELECT COUNT(*)
			FROM (
					SELECT user_id
					FROM Reservations
					UNION
					SELECT owner_id
					FROM Rooms
						JOIN Reservations ON Reservations.room_id = Rooms.id
				) active_users
		) * 100 / COUNT(*),
		2
	) AS percent
FROM Users
```

</details>

72. Выведите среднюю стоимость бронирования для комнат, которых бронировали хотя бы один раз. Среднюю стоимость 
необходимо округлить до целого значения вверх. [(сайт)](https://sql-academy.org/ru/trainer/tasks/72)

<details>
  <summary>Решение</summary>

```mysql
SELECT room_id,
	CEIL(AVG(Reservations.price)) AS avg_price
FROM Reservations
GROUP BY room_id
```

</details>

73. Выведите id тех комнат, которые арендовали нечетное количество раз 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/73)

<details>
  <summary>Решение</summary>

```mysql
SELECT room_id,
	COUNT(*) AS COUNT
FROM Reservations
GROUP BY room_id
HAVING COUNT % 2 != 0
```

</details>

74. Выведите идентификатор и признак наличия интернета в помещении. Если интернет в сдаваемом жилье присутствует, то 
выведите «YES», иначе «NO». [(сайт)](https://sql-academy.org/ru/trainer/tasks/74)

<details>
  <summary>Решение</summary>

```mysql
SELECT id,
	IF(has_internet = 1, 'YES', 'NO') AS has_internet
FROM Rooms
```

</details>

75. Выведите фамилию, имя и дату рождения студентов, кто был рожден в мае. 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/75)

<details>
  <summary>Решение</summary>

```mysql
SELECT last_name,
	first_name,
	birthday
FROM Student
WHERE MONTH(birthday) = 5
```

</details>

76. Вывести имена всех пользователей сервиса бронирования жилья, а также два признака: является ли пользователь 
собственником какого-либо жилья (is_owner) и является ли пользователь арендатором (is_tenant). В случае наличия у 
пользователя признака необходимо вывести в соответствующее поле 1, иначе 0. 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/76)

<details>
  <summary>Решение</summary>

```mysql
SELECT name,
	IF(
		id IN (
			SELECT owner_id
			FROM Rooms
		),
		1,
		0
	) AS is_owner,
	IF(
		id IN (
			SELECT user_id
			FROM Reservations
		),
		1,
		0
	) AS is_tenant
FROM Users
```

</details>

77. Создайте представление с именем "People", которое будет содержать список имен (first_name) и фамилий (last_name) 
всех студентов (Student) и преподавателей(Teacher) [(сайт)](https://sql-academy.org/ru/trainer/tasks/77)

<details>
  <summary>Решение</summary>

```mysql
CREATE OR REPLACE VIEW People AS
SELECT first_name,
	last_name
FROM Student
UNION
SELECT first_name,
	last_name
FROM Teacher
```

</details>

78. Выведите всех пользователей с электронной почтой в «hotmail.com» 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/78)

<details>
  <summary>Решение</summary>

```mysql
SELECT *
FROM Users
WHERE email LIKE '%@hotmail.com'
```

</details>

79. Выведите поля id, home_type, price у всех комнат из таблицы Rooms. Если комната имеет телевизор и интернет 
одновременно, то в качестве цены в поле price выведите цену, применив скидку 10% 
[(сайт)](https://sql-academy.org/ru/trainer/tasks/79)

<details>
  <summary>Решение</summary>

```mysql
SELECT id,
	home_type,
	IF (
		has_internet
		AND has_tv,
		price * 0.9,
		price
	) AS price
FROM Rooms
```

</details>

80. Создайте представление «Verified_Users» с полями id, name и email, которое будет показывает только тех 
пользователей, у которых подтвержден адрес электронной почты. [(сайт)](https://sql-academy.org/ru/trainer/tasks/80)

<details>
  <summary>Решение</summary>

```mysql
CREATE OR REPLACE VIEW Verified_Users AS
SELECT id,
	name,
	email
FROM Users
WHERE email_verified_at IS NOT NULL
```

</details>

93. Какой средний возраст клиентов, купивших Smartwatch (использовать наименование товара product.name) в 2024 году?
[(сайт)](https://sql-academy.org/ru/trainer/tasks/93)

<details>
  <summary>Решение</summary>

```mysql
SELECT AVG(uniqueCustomers.age) AS average_age
FROM (
		SELECT DISTINCT Customer.customer_key,
			Customer.age
		FROM Customer
			JOIN Purchase ON Purchase.customer_key = Customer.customer_key
			JOIN Product ON Product.product_key = Purchase.product_key
		WHERE Product.name = 'Smartwatch'
			AND YEAR(Purchase.date) = '2024'
	) AS uniqueCustomers
```

</details>

94. Вывести имена покупателей, каждый из которых приобрёл Laptop и Monitor (использовать наименование товара product.name) в марте 2024 года?
[(сайт)](https://sql-academy.org/ru/trainer/tasks/94)

<details>
  <summary>Решение</summary>

```mysql
SELECT Customer.name AS name
FROM Customer
	JOIN Purchase ON Purchase.customer_key = Customer.customer_key
	JOIN Product ON Product.product_key = Purchase.product_key
WHERE Product.name IN ('Laptop', 'Monitor')
	AND YEAR(Purchase.date) = 2024
	AND MONTH(Purchase.date) = 3
GROUP BY Customer.name
HAVING COUNT(DISTINCT Product.name) = 2
```

</details>

97. Посчитать количество работающих складов на текущую дату по каждому городу. Вывести только те города, у которых количество складов более 80. Данные на выходе - город, количество складов.
[(сайт)](https://sql-academy.org/ru/trainer/tasks/97)

<details>
  <summary>Решение</summary>

```mysql
SELECT city,
	COUNT(*) AS warehouse_count
FROM Warehouses
WHERE date_close IS NULL
GROUP BY city
HAVING warehouse_count > 80
```

</details>

99. Посчитай доход с женской аудитории (доход = сумма(price * items)). Обратите внимание, что в таблице женская аудитория имеет поле user_gender «female» или «f».
[(сайт)](https://sql-academy.org/ru/trainer/tasks/99)

<details>
  <summary>Решение</summary>

```mysql
SELECT sum(items * price) AS income_from_female
FROM Purchases
WHERE user_gender LIKE 'f%'
```

</details>

101. Выведи для каждого пользователя первое наименование, которое он заказал (первое по времени транзакции).
[(сайт)](https://sql-academy.org/ru/trainer/tasks/101)

<details>
  <summary>Решение</summary>

```mysql
SELECT DISTINCT user_id,
	FIRST_VALUE(item) OVER(
		PARTITION BY user_id
		ORDER BY transaction_ts
	) AS item
FROM Transactions
```

</details>

103. Вывести список имён сотрудников, получающих большую заработную плату, чем у непосредственного руководителя.
[(сайт)](https://sql-academy.org/ru/trainer/tasks/103)

<details>
  <summary>Решение</summary>

```mysql
SELECT employees.name
FROM Employee AS employees,
	Employee AS chieves
WHERE chieves.id = employees.chief_id
	AND employees.salary > chieves.salary;
```

</details>

109. Выведите название страны, где находится город «Salzburg»
[(сайт)](https://sql-academy.org/ru/trainer/tasks/109)

<details>
  <summary>Решение</summary>

```mysql
SELECT Countries.name AS country_name
FROM Countries
	JOIN Regions ON Regions.countryid = Countries.id
	JOIN Cities ON Cities.regionid = Regions.id
WHERE Cities.name = 'Salzburg'
```

</details>

111. Посчитайте население каждого региона. В качестве результата выведите название региона и его численность населения.
[(сайт)](https://sql-academy.org/ru/trainer/tasks/111)

<details>
  <summary>Решение</summary>

```mysql
SELECT Regions.name AS region_name,
	SUM(population) AS total_population
FROM Regions
	JOIN Cities ON Cities.regionid = Regions.id
GROUP BY Regions.name
```

</details>