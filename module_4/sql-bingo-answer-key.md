# 🏥 SQL Bingo Answer Key (Instructor Only)

**Where to play:** [sql-practice.com](https://www.sql-practice.com/). No setup needed! Keep the database set to **hospital.db**.

> [!NOTE]
> Run each query on the site to see the answer. Some "top" squares could have ties, so peek at the top few rows to check.

| Square | Query |
|---|---|
| How many patients are there? | `SELECT COUNT(*) FROM patients;` |
| How many patients are male? | `SELECT COUNT(*) FROM patients WHERE gender = 'M';` |
| Tallest patient | `SELECT first_name, last_name, height FROM patients ORDER BY height DESC LIMIT 1;` |
| Heaviest patient | `SELECT first_name, last_name, weight FROM patients ORDER BY weight DESC LIMIT 1;` |
| How many patients have no allergies? | `SELECT COUNT(*) FROM patients WHERE allergies IS NULL;` |
| How many patients live in Hamilton? | `SELECT COUNT(*) FROM patients WHERE city = 'Hamilton';` |
| List every city, no repeats | `SELECT DISTINCT city FROM patients;` |
| Patients whose first name starts with C | `SELECT first_name, last_name FROM patients WHERE first_name LIKE 'C%';` |
| Average patient height | `SELECT AVG(height) FROM patients;` |
| How many patients weigh 100 to 120? | `SELECT COUNT(*) FROM patients WHERE weight BETWEEN 100 AND 120;` |
| Oldest patient | `SELECT first_name, last_name, birth_date FROM patients ORDER BY birth_date LIMIT 1;` |
| Youngest patient | `SELECT first_name, last_name, birth_date FROM patients ORDER BY birth_date DESC LIMIT 1;` |
| Count patients by gender | `SELECT gender, COUNT(*) FROM patients GROUP BY gender;` |
| City with the most patients | `SELECT city, COUNT(*) FROM patients GROUP BY city ORDER BY COUNT(*) DESC LIMIT 1;` |
| Most common allergy | `SELECT allergies, COUNT(*) FROM patients WHERE allergies IS NOT NULL GROUP BY allergies ORDER BY COUNT(*) DESC LIMIT 1;` |
| How many doctors are there? | `SELECT COUNT(*) FROM doctors;` |
| List every doctor specialty, no repeats | `SELECT DISTINCT specialty FROM doctors;` |
| How many admissions are there? | `SELECT COUNT(*) FROM admissions;` |
| Most common diagnosis | `SELECT diagnosis, COUNT(*) FROM admissions GROUP BY diagnosis ORDER BY COUNT(*) DESC LIMIT 1;` |
| How many admissions were discharged the same day? | `SELECT COUNT(*) FROM admissions WHERE admission_date = discharge_date;` |
| How many patients were born in the 1990s? | `SELECT COUNT(*) FROM patients WHERE birth_date BETWEEN '1990-01-01' AND '1999-12-31';` |
| How many provinces are in the table? | `SELECT COUNT(*) FROM province_names;` |
| Each patient with their full province name | `SELECT patients.first_name, patients.last_name, province_names.province_name FROM patients LEFT JOIN province_names ON patients.province_id = province_names.province_id;` |
| Each admission with the doctor's last name | `SELECT admissions.patient_id, admissions.diagnosis, doctors.last_name FROM admissions LEFT JOIN doctors ON admissions.attending_doctor_id = doctors.doctor_id;` |

## 💡 Teachable moments
- **Most common allergy:** without `WHERE allergies IS NOT NULL`, the winner might be "no allergy" (`NULL`)!
- **The two JOIN squares** tie right into today's JOINs lecture. 🤝
