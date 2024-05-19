# Query con SELECT

## 1. Selezionare tutti gli studenti nati nel 1990 (160)

```sql

SELECT *
FROM `students`
WHERE YEAR(`date_of_birth`) = 1990;

```

## 2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

```sql

SELECT *
FROM `courses`
WHERE `cfu` > 10;

```
## 3. Selezionare tutti gli studenti che hanno più di 30 anni

```sql
SELECT *
FROM `students`
WHERE `date_of_birth` < CURRENT_DATE() - INTERVAL 30 YEAR;
```

## 4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

```sql
SELECT *
FROM `courses`
WHERE `period` LIKE 'i %' AND `year` = 1;
```

## 5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)

```sql
SELECT *
FROM `exams`
WHERE `date` = "2020-01-19" AND HOUR(`hour`) >= '14';
```

## 6. Selezionare tutti i corsi di laurea magistrale (38)

```sql
SELECT * FROM `degrees`
WHERE `level` = 'magistrale';
```

## 7. Da quanti dipartimenti è composta l'università? (12)

```sql
SELECT COUNT(`name`) AS `numero_dipartimenti`
FROM `departments`;
```

## 8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

```sql
SELECT *
FROM `teachers`
WHERE `phone` IS NULL;
```

# Query con JOIN

## 1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```sql
SELECT *
FROM `students`
INNER JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';
```

## 2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

```sql
SELECT *
FROM `degrees`
INNER JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze' AND `degrees`.`level` = 'magistrale';
```

## 3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```sql
SELECT * FROM `courses` INNER JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id` INNER JOIN `teachers` ON `course_teacher`.`course_id` = `teachers`.`id` WHERE `teachers`.`name` = 'Fulvio' AND `teachers`.`surname` = 'Amato';
```

## 4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

```sql
SELECT *, CONCAT_WS(" ", `surname`, `name`) AS 'nome_completo'
FROM `students`
INNER JOIN `exam_student`
ON `students`.`id` = `exam_student`.`student_id`
INNER JOIN `exams`
ON `exam_student`.`exam_id` = `exams`.`id`
ORDER BY `nome_completo` ASC;
```

## 5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```sql
SELECT *
FROM `degrees`
INNER JOIN `courses`
ON `degrees`.`id` = `courses`.`degree_id`
INNER JOIN `course_teacher`
ON `courses`.`degree_id` = `course_teacher`.`course_id`
INNER JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`;
```

## 6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

```sql
SELECT DISTINCT `teachers`.`name`
FROM `departments`
INNER JOIN `degrees`
ON `departments`.`id` = `degrees`.`department_id`
INNER JOIN `courses`
ON `degrees`.`id` = `courses`.`degree_id`
INNER JOIN `course_teacher`
ON `courses`.`degree_id` = `course_teacher`.`course_id`
INNER JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';
```

## 7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.

```sql
SELECT *
FROM `teachers`
WHERE `phone` IS NULL;
```

# Query con GROUP BY

## 1. Contare quanti iscritti ci sono stati ogni anno

```sql
SELECT COUNT(`students`.`id`) AS 'iscritti', YEAR(`students`.`enrolment_date`) AS 'anno'
FROM `students`
GROUP BY `anno`;
```

## 2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

```sql
SELECT COUNT(*), `teachers`.`office_address`
FROM `teachers`
GROUP BY `teachers`.`office_address`;
```

## 3. Calcolare la media dei voti di ogni appello d'esame

```sql
SELECT *
FROM `teachers`
WHERE `phone` IS NULL;
```

## 4. Contare quanti corsi di laurea ci sono per ogni dipartimento

```sql
SELECT *
FROM `teachers`
WHERE `phone` IS NULL;
```
