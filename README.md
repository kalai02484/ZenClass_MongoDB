# Zen Class Programme – MongoDB Database Design

This project demonstrates the design of a MongoDB database for managing a Zen Class programme. It includes collections for users, mentors, topics, tasks, attendance, CodeKata progress, and company placement drives, along with sample queries to retrieve useful insights.

---

## 📌 Project Objective

To design a MongoDB schema that can:

* Track students and mentors
* Record topics taught and tasks assigned
* Monitor attendance and task submission
* Store CodeKata problem-solving progress
* Manage company placement drives

---

## 🗂️ Database Collections

### 1. Users

Stores student details.

```js
{
  user_id: "U001",
  name: "Arun",
  email: "arun@mail.com",
  batch: "B43",
  mentor_id: "M001"
}
```

### 2. Mentors

Stores mentor details and assigned mentees.

```js
{
  mentor_id: "M001",
  name: "Rahul",
  mentees: ["U001", "U002", "U005"]
}
```

### 3. CodeKata

Tracks problems solved by each user.

```js
{
  user_id: "U001",
  problems_solved: 50,
  date: ISODate("2020-10-10")
}
```

### 4. Attendance

Tracks daily attendance of students.

```js
{
  user_id: "U002",
  date: ISODate("2020-10-16"),
  status: "absent"
}
```

### 5. Topics

Stores topics taught on specific dates.

```js
{
  topic: "MongoDB Aggregation",
  date: ISODate("2020-10-20")
}
```

### 6. Tasks

Stores tasks assigned for each topic.

```js
{
  task: "Aggregation Pipeline",
  topic: "MongoDB Aggregation",
  date: ISODate("2020-10-20"),
  submitted_users: ["U001", "U004"]
}
```

### 7. Company Drives

Stores placement drive details and participating students.

```js
{
  company: "Zoho",
  drive_date: ISODate("2020-10-18"),
  students_appeared: ["U001", "U002"]
}
```

---

## 🧪 Mock Data Insertion

Use `insertMany()` to populate collections.

### Users

```js
db.users.insertMany([
  { user_id: "U001", name: "Arun", email: "arun@mail.com", batch: "B43", mentor_id: "M001" },
  { user_id: "U002", name: "Divya", email: "divya@mail.com", batch: "B43", mentor_id: "M001" },
  { user_id: "U003", name: "Karthik", email: "karthik@mail.com", batch: "B44", mentor_id: "M002" },
  { user_id: "U004", name: "Meena", email: "meena@mail.com", batch: "B44", mentor_id: "M002" },
  { user_id: "U005", name: "Suresh", email: "suresh@mail.com", batch: "B43", mentor_id: "M001" }
])
```

### Mentors

```js
db.mentors.insertMany([
  { mentor_id: "M001", name: "Rahul", mentees: ["U001","U002","U005"] },
  { mentor_id: "M002", name: "Priya", mentees: ["U003","U004"] }
])
```

### CodeKata

```js
db.codekata.insertMany([
  { user_id: "U001", problems_solved: 50, date: ISODate("2020-10-10") },
  { user_id: "U001", problems_solved: 20, date: ISODate("2020-10-20") },
  { user_id: "U002", problems_solved: 40, date: ISODate("2020-10-15") },
  { user_id: "U003", problems_solved: 25, date: ISODate("2020-10-18") },
  { user_id: "U004", problems_solved: 60, date: ISODate("2020-10-25") }
])
```

### Attendance

```js
db.attendance.insertMany([
  { user_id: "U001", date: ISODate("2020-10-16"), status: "present" },
  { user_id: "U002", date: ISODate("2020-10-16"), status: "absent" },
  { user_id: "U003", date: ISODate("2020-10-20"), status: "absent" },
  { user_id: "U004", date: ISODate("2020-10-21"), status: "present" },
  { user_id: "U005", date: ISODate("2020-10-22"), status: "absent" }
])
```

### Topics

```js
db.topics.insertMany([
  { topic: "HTML Basics", date: ISODate("2020-10-05") },
  { topic: "CSS Flexbox", date: ISODate("2020-10-10") },
  { topic: "JavaScript DOM", date: ISODate("2020-10-15") },
  { topic: "MongoDB Aggregation", date: ISODate("2020-10-20") },
  { topic: "Node.js Intro", date: ISODate("2020-11-02") }
])
```

### Tasks

```js
db.tasks.insertMany([
  { task: "HTML Task", topic: "HTML Basics", date: ISODate("2020-10-05"), submitted_users: ["U001","U002"] },
  { task: "Flexbox Layout", topic: "CSS Flexbox", date: ISODate("2020-10-10"), submitted_users: ["U001"] },
  { task: "DOM Manipulation", topic: "JavaScript DOM", date: ISODate("2020-10-15"), submitted_users: ["U002","U003"] },
  { task: "Aggregation Pipeline", topic: "MongoDB Aggregation", date: ISODate("2020-10-20"), submitted_users: ["U001","U004"] }
])
```

### Company Drives

```js
db.company_drives.insertMany([
  { company: "Zoho", drive_date: ISODate("2020-10-18"), students_appeared: ["U001","U002"] },
  { company: "TCS", drive_date: ISODate("2020-10-25"), students_appeared: ["U003","U004","U005"] },
  { company: "Infosys", drive_date: ISODate("2020-11-05"), students_appeared: ["U001","U003"] }
])
```

---

## 🔍 Queries

### 1. Topics and Tasks in October

```js
db.topics.find({
  date: { $gte: ISODate("2020-10-01"), $lte: ISODate("2020-10-31") }
})

db.tasks.find({
  date: { $gte: ISODate("2020-10-01"), $lte: ISODate("2020-10-31") }
})
```

### 2. Company Drives between 15-Oct-2020 and 31-Oct-2020

```js
db.company_drives.find({
  drive_date: { $gte: ISODate("2020-10-15"), $lte: ISODate("2020-10-31") }
})
```

### 3. Company Drives and Students Appeared

```js
db.company_drives.find({}, { company: 1, drive_date: 1, students_appeared: 1, _id: 0 })
```

### 4. Number of Problems Solved by Each User

```js
db.codekata.aggregate([
  { $group: { _id: "$user_id", total_problems_solved: { $sum: "$problems_solved" } } }
])
```

### 5. Mentors with Mentee Count > 15

```js
db.mentors.aggregate([
  { $project: { name: 1, mentee_count: { $size: "$mentees" } } },
  { $match: { mentee_count: { $gt: 15 } } }
])
```

### 6. Users Absent and Task Not Submitted (15-Oct to 31-Oct)

```js
db.attendance.find({
  status: "absent",
  date: { $gte: ISODate("2020-10-15"), $lte: ISODate("2020-10-31") }
})
```

(Compare these users manually with `submitted_users` in tasks.)

---

## 🚀 How to Run

1. Install MongoDB and Mongo Shell.
2. Create a database:

```bash
use zen_class_db
```

3. Create collections and insert mock data using the scripts above.
4. Run the queries to verify outputs.

---

## 📖 Summary

This MongoDB project demonstrates:

* Schema design for an EdTech learning platform
* Relationship handling using references
* Querying data with `find()` and `aggregate()`
* Real-world analytics like attendance, tasks, and placements

This serves as a beginner-friendly MongoDB case study for full-stack development practice.
