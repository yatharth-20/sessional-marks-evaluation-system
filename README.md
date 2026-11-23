# Sessional Marks Evaluation System

A lightweight, role-based web application for calculating, managing, and evaluating student sessional marks, grade upgrade bonuses, and class performance reports. Built with HTML5, CSS3, and Vanilla JavaScript using browser `localStorage` for client-side persistence.

---

## Features by Role

### 1. Student
- **View Sessional Marks**: Look up subject marks and detailed sessional breakdowns (Attendance, Assignments, Upgrade Bonus, Total Sessional, Final Total, and Final Grade) by **Student ID** (e.g., `S101`) or **Student Name**.

### 2. Teacher
- **Submit & Delete Marks**: Record or remove subject marks (`0–100`) for individual students.
- **Sessional Marks Calculator**: Compute sessional marks and automatic grade upgrade bonuses from:
  - Attendance percentage (`0–100%`)
  - Assignments submitted (`0–5`)
  - Current marks (`0–75`)
- **Search & Export Processed Grades**: Filter batch-processed student records by Student ID and export the evaluated grades to `student_grades_processed.csv`.

### 3. Admin
- **Batch CSV Upload & Processing**: Upload a CSV file of student records to automatically evaluate sessional marks and grades across the entire batch.
- **Top Performing Students**: Automatically highlights the **Top 5 Students** sorted by final total marks.
- **Update Student Marks**: Modify subject marks for existing student records.
- **Class Performance Report**: Generate summary metrics including total students, class average final marks, and grade distribution (`10`, `9`, `8`, `7`, `6`, `5`, `F`).

---

## Evaluation & Grading Formula

Total marks (`100`) are composed of **Current Marks** (out of `75`) and **Sessional Marks** (out of `25`):

1. **Attendance Marks** (out of `10`):
   $$\text{Attendance Marks} = \left(\frac{\text{Attendance \%}}{100}\right) \times 10$$

2. **Assignment Marks** (out of `10`):
   $$\text{Assignment Marks} = \left(\frac{\text{Assignments Submitted}}{5}\right) \times 10$$

3. **Initial Sessional & Base Total**:
   $$\text{Initial Sessional} = \text{Attendance Marks} + \text{Assignment Marks}$$
   $$\text{Base Total} = \text{Current Marks} + \text{Initial Sessional}$$

4. **Grade Thresholds**:
   | Total Marks | Grade |
   | :--- | :--- |
   | `>= 87` | **10** |
   | `>= 75` | **9** |
   | `>= 65` | **8** |
   | `>= 55` | **7** |
   | `>= 45` | **6** |
   | `>= 35` | **5** |
   | `< 35` | **F** |

5. **Grade Upgrade Bonus** (out of `5`):
   - If a student's Base Grade is not `F` and their Base Total is within **5 marks** of a higher grade threshold (`87`, `75`, `65`, `55`, `45`, `35`), they receive the exact difference (`0 < diff <= 5`) as an **Upgrade Bonus** to reach the higher grade.
   - **Final Sessional Marks** = $\text{Initial Sessional} + \text{Upgrade Bonus}$ (out of `25`).
   - **Final Total Marks** = $\text{Current Marks} + \text{Final Sessional Marks}$ (out of `100`).

---

## Project Structure

```text
sessional-marks-evaluation-system/
├── select-role.html          # Role selection entry page (Student / Teacher / Admin)
├── login.html                # Role-aware login screen
├── dashboard.html            # Main role-based dashboard and evaluation engine
├── styles.css                # Application styles and responsive card layout
├── students_datafinal.csv    # Sample dataset (55 student records) for batch testing
├── LICENSE                   # Project license
└── README.md                 # Project documentation
```

---

## CSV File Format

When uploading student data in the **Admin Dashboard**, the CSV file must include the following headers:

```csv
StudentID,Attendance,AssignmentsSubmitted,CurrentMarksOutOf75
S101,85,5,65
S102,92,4,70
S103,76,3,55
```

- `StudentID`: Unique student identifier (e.g., `S101`, `S001`)
- `Attendance`: Attendance percentage (`0` to `100`)
- `AssignmentsSubmitted`: Number of assignments completed (`0` to `5`)
- `CurrentMarksOutOf75`: Marks scored out of `75` (`0` to `75`)

A ready-to-use sample file ([`students_datafinal.csv`](./students_datafinal.csv)) is included in the repository.

---

## Getting Started

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yatharth-20/sessional-marks-evaluation-system.git
   cd sessional-marks-evaluation-system
   ```

2. **Launch the application**:
   - Open `select-role.html` directly in any modern web browser, **or**
   - Serve the folder with a local static server (e.g., VS Code Live Server, `npx serve .`, or `python -m http.server 8000`) and navigate to `http://localhost:8000/select-role.html`.

3. **Recommended Testing Workflow**:
   - Select **Admin** -> Login -> **Manage Students Data** -> Upload `students_datafinal.csv` and click **Process Data**, then click **Generate Report**.
   - Select **Teacher** -> Login -> **Process Student Grades** to search or export the evaluated dataset, or use **Sessional Marks Calculator** to evaluate an individual student.
   - Select **Student** -> Login -> **View Sessional Marks** -> Enter a Student ID (such as `S101` or `S008`) to view their sessional breakdown.

---

## License

Distributed under the terms of the [LICENSE](./LICENSE) file.
