1. Contare quanti iscritti ci sono stati ogni anno
SELECT YEAR(enrolment_date) AS year, COUNT(*) AS number_of_students
FROM students
GROUP BY YEAR(enrolment_date);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT office_address, COUNT(*) AS number_of_teachers
FROM teachers
GROUP BY office_address
HAVING COUNT(*) > 1;


3. Calcolare la media dei voti di ogni appello d'esame
SELECT exam_id, AVG(grade) AS average_grade
FROM exam_results
GROUP BY exam_id;

4. Contare quanti corsi di laurea ci sono per ogni dipartiment
SELECT department_id, COUNT(*) AS number_of_degrees
FROM degrees
GROUP BY department_id;

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
SELECT *
FROM students
WHERE degree_id IN (SELECT id FROM degrees WHERE name = 'Economia');

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze
SELECT *
FROM degrees
WHERE level = 'laurea magistrale' AND department_id = 
(SELECT id FROM departments WHERE name = 'Neuroscienze');

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
SELECT *
FROM courses
WHERE id IN (SELECT course_id FROM course_teacher WHERE teacher_id = 44);

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome
SELECT students.name, students.surname, degrees.name AS degree_name, departments.name AS department_name
FROM students
JOIN degrees ON students.degree_id = degrees.id
JOIN departments ON degrees.department_id = departments.id
ORDER BY students.surname, students.name;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
SELECT degrees.name AS degree_name, courses.name AS course_name, teachers.name AS teacher_name, teachers.surname AS teacher_surname
FROM degrees
JOIN courses ON degrees.id = courses.degree_id
JOIN course_teacher ON courses.id = course_teacher.course_id
JOIN teachers ON course_teacher.teacher_id = teachers.id;


6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)
SELECT teachers.*
FROM teachers
JOIN course_teacher ON teachers.id = course_teacher.teacher_id
JOIN courses ON course_teacher.course_id = courses.id
JOIN degrees ON courses.degree_id = degrees.id
WHERE degrees.department_id = 54;


7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18
SELECT students.name, students.surname, exams.name AS exam_name, COUNT(exam_student.exam_id) AS attempts, MAX(exam_student.grade) AS max_grade
FROM exam_student
JOIN students ON exam_student.student_id = students.id
JOIN exams ON exam_student.exam_id = exams.id
WHERE exam_student.grade >= 18
GROUP BY students.id, exams.id;


