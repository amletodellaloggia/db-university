Esercizio GROUP BY
===
1. Contare quanti iscritti ci sono stati ogni anno

  SELECT COUNT(*) as `students_enroled`, `enrolment_date`
  FROM `students`
  GROUP BY YEAR(`enrolment_date`);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

  SELECT COUNT(*) as `sharing_teachers`, `office_address`
  FROM `teachers`
  GROUP BY `office_address`;

3. Calcolare la media dei voti di ogni appello d'esame

  SELECT AVG(`vote`) as `average_vote`, `exam_id`
  FROM `exam_student`
  GROUP BY `exam_id`

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

  SELECT COUNT(*) as `degrees_amount`, `name` 
  FROM `degrees`
  GROUP BY `department_id`