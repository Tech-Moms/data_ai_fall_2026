# 🤝 SQL JOINs Made Easy

**Time:** ~20 min | **Level:** Total beginner 🌱 | **Where to practice:** [sql-practice.com](https://www.sql-practice.com/) (hospital.db)

## 🎯 Today's goal
Learn how to combine **two tables** into one result. That's it!

---

## 🤔 The problem

Here are the first few rows of the real `patients` table on sql-practice.com:

**`patients`**

| patient_id | first_name | last_name | city | province_id |
|---|---|---|---|---|
| 1 | Donald | Waterfield | Barrie | ON |
| 2 | Mickey | Baasha | Dundas | ON |
| 3 | Jiji | Sharma | Hamilton | ON |
| 8 | Sonny | Beckett | Port Hawkesbury | NS |

**`province_names`**

| province_id | province_name |
|---|---|
| ON | Ontario |
| NS | Nova Scotia |

❓ **What is the full province name where Sonny lives?**

The `patients` table only says **NS**. The full name is in the other table. We need to **join** them! 🙌

---

## 🔑 What do they have in common?

Both tables have a **`province_id`** column. That's how SQL matches the rows up.

> 💡 **Think of it like this:** You have one list with your friends' names + zip codes, and another list with zip codes + city names. To find what city each friend lives in, you match them up **by zip code**. 📬

---

## ⬅️ Your first JOIN: LEFT JOIN

```sql
SELECT *
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id;
```

**Result:**

| patient_id | first_name | last_name | city | province_id | province_name |
|---|---|---|---|---|---|
| 1 | Donald | Waterfield | Barrie | ON | Ontario |
| 2 | Mickey | Baasha | Dundas | ON | Ontario |
| 3 | Jiji | Sharma | Hamilton | ON | Ontario |
| 8 | Sonny | Beckett | Port Hawkesbury | NS | Nova Scotia |

🎉 Now everything is in one place!

> 💜 **Why LEFT JOIN?** It's the JOIN I use every day. It keeps **every row** from your first table, so nothing disappears by accident. Start with your main table, then add the extra info.

### 🗣️ Read it like a sentence
| SQL | In plain English |
|---|---|
| `FROM patients` | Start with the patients table |
| `LEFT JOIN province_names` | Keep **every** patient, and bring in their province name |
| `ON patients.province_id = province_names.province_id` | Match rows where the province IDs are the same |

---

## ✏️ Pick just the columns you want

Instead of `*`, name the columns. Put the **table name + a dot** in front so SQL knows where each one lives:

```sql
SELECT patients.first_name, patients.last_name, province_names.province_name
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id;
```

---

## 📝 Remember this pattern

```sql
SELECT columns
FROM first_table
LEFT JOIN second_table
  ON first_table.shared_column = second_table.shared_column;
```

## 🍎 More Examples: Starting From `patients`
 
Every example below starts with **all the patients**, then adds info from another table. Run them on sql-practice.com and see what you get! 👀
 
### 1️⃣ Patients from Hamilton, with their province name
*Adding a `WHERE` filter works just like normal!*
 
```sql
SELECT patients.first_name, patients.last_name, patients.city, province_names.province_name
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id
WHERE patients.city = 'Hamilton';
```
 
### 2️⃣ Patients with allergies, sorted by province
*You can `ORDER BY` a column from either table.*
 
```sql
SELECT patients.first_name, patients.allergies, province_names.province_name
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id
WHERE patients.allergies IS NOT NULL
ORDER BY province_names.province_name;
```
 
### 3️⃣ Patients with their hospital visits
*This time we join to `admissions` using `patient_id`.*
 
```sql
SELECT patients.first_name, patients.last_name, admissions.admission_date, admissions.diagnosis
FROM patients
LEFT JOIN admissions
  ON patients.patient_id = admissions.patient_id;
```
 
👀 **Notice two things:**
- Some patients show up **more than once**. They visited the hospital more than once!
- Some patients have **`NULL`** for the date and diagnosis. They've never been admitted, but LEFT JOIN still keeps them. 💜
### 4️⃣ Patients who have NEVER been admitted
*This is a LEFT JOIN superpower! Look for the `NULL`s.* 🦸‍♀️
 
```sql
SELECT patients.first_name, patients.last_name
FROM patients
LEFT JOIN admissions
  ON patients.patient_id = admissions.patient_id
WHERE admissions.patient_id IS NULL;
```
 
### 5️⃣ How many times each patient was admitted
*LEFT JOIN + `GROUP BY` + `COUNT`*
 
```sql
SELECT patients.first_name, patients.last_name, COUNT(admissions.patient_id) AS total_visits
FROM patients
LEFT JOIN admissions
  ON patients.patient_id = admissions.patient_id
GROUP BY patients.patient_id, patients.first_name, patients.last_name
ORDER BY total_visits DESC;
```
 
💡 We count `admissions.patient_id` (not `*`) so patients who were never admitted get **0** instead of 1.
 
### 6️⃣ The 5 tallest patients and where they live
*LEFT JOIN + `ORDER BY` + `LIMIT`*
 
```sql
SELECT patients.first_name, patients.last_name, patients.height, province_names.province_name
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id
ORDER BY patients.height DESC
LIMIT 5;
```
 
### 7️⃣ Patients born in 2000 or later, with their province
*Filtering on a date works too!*
 
```sql
SELECT patients.first_name, patients.birth_date, province_names.province_name
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id
WHERE patients.birth_date >= '2000-01-01'
ORDER BY patients.birth_date;
```
 
### 8️⃣ How many patients live in each province
*`GROUP BY` the full province name instead of the code.*
 
```sql
SELECT province_names.province_name, COUNT(patients.patient_id) AS total_patients
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id
GROUP BY province_names.province_name
ORDER BY total_patients DESC;
```
 
### 9️⃣ Average height and weight by province
*Use `AVG` with a JOIN. `ROUND` keeps the numbers tidy.*
 
```sql
SELECT province_names.province_name,
       ROUND(AVG(patients.height), 1) AS avg_height,
       ROUND(AVG(patients.weight), 1) AS avg_weight
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id
GROUP BY province_names.province_name;
```
 
### 🔟 Each patient's most recent hospital visit
*`MAX` on a date gives you the latest one.*
 
```sql
SELECT patients.first_name, patients.last_name, MAX(admissions.admission_date) AS last_visit
FROM patients
LEFT JOIN admissions
  ON patients.patient_id = admissions.patient_id
GROUP BY patients.patient_id, patients.first_name, patients.last_name
ORDER BY last_visit DESC;
```
 
💡 Patients who were never admitted will have a `NULL` last visit.
 
### 🌟 Stretch: Patients, their visits, AND their doctor
*You can chain LEFT JOINs! Patients ➡️ admissions ➡️ doctors*
 
```sql
SELECT patients.first_name, admissions.diagnosis, doctors.last_name AS doctor
FROM patients
LEFT JOIN admissions
  ON patients.patient_id = admissions.patient_id
LEFT JOIN doctors
  ON admissions.attending_doctor_id = doctors.doctor_id;
```
 

## 🏋️ Your turn! (on sql-practice.com)

**1.** Show each patient's first name with their admission date.
*Hint: join `patients` and `admissions` on `patient_id`.*

<details><summary>Answer</summary>

```sql
SELECT patients.first_name, admissions.admission_date
FROM patients
LEFT JOIN admissions
  ON patients.patient_id = admissions.patient_id;
```
</details>

**2.** Show the first name, last name, and city of every patient who lives in **Nova Scotia**. Use the full province name, not `NS`!
*Hint: LEFT JOIN `province_names`, then filter with `WHERE`.*
 
<details><summary>Answer</summary>
  
```sql
SELECT patients.first_name, patients.last_name, patients.city
FROM patients
LEFT JOIN province_names
  ON patients.province_id = province_names.province_id
WHERE province_names.province_name = 'Nova Scotia';
```
</details>

---


