Esercizio JOIN
===
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

  SELECT `students`.`name`, `students`.`surname`
  FROM `students`
  JOIN `degrees` ON `degrees`.`id` = `degree_id`
  WHERE `degrees`.`name` = "Corso di Laurea in Economia";

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze

  SELECT `courses`.`name` AS "Nome Corso", `degrees`.`name` AS "Nome Corso di Laurea", `departments`.`name` AS "Dipartimento"
  FROM `courses`
  JOIN `degrees` ON `degrees`.`id` = `degree_id`
  JOIN `departments` ON `department_id` = `departments`.`id`
  WHERE `degrees`.`level` = "magistrale" 
  AND `departments`.`name` = "Dipartimento di Neuroscienze";

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

  SELECT `courses`.`name` AS "Nome Corso", `teachers`.`name` AS "Nome", `teachers`.`surname` AS "Cognome"
  FROM `courses`
  JOIN `course_teacher` ON `course_id` = `courses`.`id`
  JOIN `teachers` ON `teacher_id` = `teachers`.`id`
  WHERE `teachers`.`id` = 44

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome

  SELECT `students`.`surname` AS "Cognome", `students`.`name` AS "Nome", `degrees`.`name` AS "Nome Corso di Laurea", `degrees`.`level` AS "Livello", `departments`.`name` AS "Dipartimento"
  FROM `students`
  JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
  JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
  ORDER BY `students`.`surname`, `students`.`name` ASC

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

  SELECT `degrees`.`name` AS "Nome Corso Laurea", `courses`.`name` AS "Nome Corso", `teachers`.`name` AS "Nome Insegnante", `teachers`.`surname` AS "Cognome Insegnante"
  FROM `degrees`
  JOIN `courses` ON `courses`.`degree_id` = `degrees`.`id`
  JOIN `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
  JOIN `teachers` ON `teacher_id` = `teachers`.`id`

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)

  SELECT `teachers`.`name` AS "Nome Insegnante", `teachers`.`surname` AS "Cognome Insegnante", `departments`.`name` AS "Dipartimento in comune"
  FROM `teachers`
  JOIN `course_teacher` ON `course_teacher`.`teacher_id` = `teachers`.`id`
  JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
  JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
  JOIN `departments` ON `department_id` = `departments`.`id`
  WHERE `departments`.`name` = "Dipartimento di Matematica"

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,
filtrare i tentativi con voto minimo 18.

<!-- Numero Tentativi per ogni esame per studente, con voto massimo  -->

  SELECT `students`.`name` AS "Nome", `students`.`surname` AS "Cognome", `courses`.`name` AS "Nome Esame", COUNT(`exam_student`.`vote`) AS "Numero Tentativi", MAX(`exam_student`.`vote`) AS "Voto Massimo"
  FROM `students`
  JOIN `exam_student` ON `exam_student`.`student_id` = `students`.`id`
  JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id`
  JOIN `courses` ON `exams`.`course_id` = `courses`.`id`
  GROUP BY `students`.`id`, `courses`.`id`
  ORDER BY `students`.`id`, `courses`.`id`;

  <!-- Numero tentativi con voto minimo 18 -->

  SELECT `students`.`name` AS "Nome", `students`.`surname` AS "Cognome", `courses`.`name` AS "Nome Esame", COUNT(`exam_student`.`vote`) AS "Numero Tentativi", MAX(`exam_student`.`vote`) AS "Voto Massimo"
  FROM `students`
  JOIN `exam_student` ON `exam_student`.`student_id` = `students`.`id`
  JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id`
  JOIN `courses` ON `exams`.`course_id` = `courses`.`id`
  WHERE `exam_student`.`vote` >= 18
  GROUP BY `students`.`id`, `courses`.`id`
  ORDER BY `students`.`id`, `courses`.`id`;