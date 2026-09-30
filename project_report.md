# 🎓 Student Grade Management System (CLI)
## Comprehensive Technical Project Report

- **Author**: Devyanshu Negi
- **GitHub Repository**: [https://github.com/DevyanshuNegi/student-grade-system](https://github.com/DevyanshuNegi/student-grade-system)
- **Technology Stack**: Pure Python 3.7+ (Standard Library: `csv`, `os`, `sys`, `typing`)
- **Persistence**: Atomic CSV File Storage (`students_data.csv`)

---

## 1. Executive Summary

The **Student Grade Management System** is a standalone, lightweight, and high-reliability Command-Line Interface (CLI) application engineered in standard Python. It is designed to modernize academic record tracking for educators, tutors, and academic administrators without requiring complex web frameworks, external database servers, or third-party dependencies.

The application automates letter grade evaluations, delivers aggregate cohort analytics (such as class averages, passing percentage, extreme performers, and grade frequency distribution), enforces strict multi-stage input validation, and guarantees safe local file persistence through an atomic write-and-replace mechanism.

---

## 2. Problem Statement & Motivation

In many educational environments, small classrooms, tutoring centers, and academic departments, student performance data is still managed through manual paper records, disconnected spreadsheets, or heavyweight software suites. These traditional approaches present recurring challenges:

- **Calculation Inefficiencies & Human Error**: Calculating individual letter grades, class averages, passing percentages, and identifying top/bottom performers manually is labor-intensive and prone to computational mistakes.
- **Data Fragmentation & Corruption**: Decentralized files often lack schema validation, leading to duplicate records, invalid values, or lost entries.
- **Overhead of Heavyweight Software**: Enterprise Learning Management Systems (LMS) require continuous internet access, high memory overhead, database servers, and complicated installation procedures.
- **Need for a Reliable Offline CLI Solution**: There is a clear operational requirement for a lightweight, dependency-free terminal utility built on standard Python principles.

---

## 3. Project Objectives & Scope

### In-Scope:
- **Terminal User Interface**: Interactive CLI navigation loop with clear menus, formatted ASCII tables, and keyboard interrupt resilience (`Ctrl+C`).
- **Automated Academic Grading**: Instant mapping of raw numerical marks (0.00 – 100.00) to standardized letter grades (`A+`, `A`, `B`, `C`, `D`, `F`).
- **Input Validation & Sanitization**: Strict validation against blank strings, invalid numerical inputs, out-of-bounds scores, and duplicate Student IDs.
- **Cohort Analytics**: Real-time aggregation of class averages, passing percentage rates, highest/lowest academic achievers, and frequency distribution across all grade tiers.
- **Atomic Local Persistence**: Safe, synchronous read/write operations against `students_data.csv` using atomic file replacement to prevent data corruption during sudden shutdowns.
- **Pure Standard Python**: Zero third-party library dependencies; runs natively on Python 3.7+ across Windows, macOS, and Linux.

---

## 4. System Architecture & Modular Design

The application is structured into six modular subsystems to ensure maintainability, testability, and high fault tolerance:

| Subsystem / Layer | Component / Functions | Primary Responsibility |
| :--- | :--- | :--- |
| **Configuration** | `BASE_DIR`, `DATA_FILE`, `GRADE_THRESHOLDS` | Centralizes constants, CSV paths, schema headers, and grade boundaries. |
| **Business Logic** | `calculate_grade()`, `compute_statistics()` | Calculates grade classifications, class averages, pass rates, and extremes. |
| **Persistence Layer** | `load_students()`, `save_students()`, `initialize_storage()` | Handles resilient CSV I/O, atomic file swap via `.tmp`, and row recovery. |
| **Input Validation** | `get_non_empty_string()`, `get_valid_marks()`, `get_unique_student_id()` | Sanitizes strings, enforces numeric intervals [0.00, 100.00], and rejects duplicate IDs. |
| **Presentation Layer** | `print_header()`, `view_all_students_view()`, `search_student_view()` | Renders formatted ASCII tables, summary stat dashboards, and record profiles. |
| **Controller Loop** | `render_main_menu()`, `main()` | Coordinates user loop, menu routing, and graceful exit handling. |

---

## 5. Grading Scale & Business Rules

Letter grades are assigned deterministically according to standard academic brackets:

| Marks Range (%) | Letter Grade | Academic Standing | Classification |
| :--- | :---: | :--- | :---: |
| **90.00% – 100.00%** | `A+` | Outstanding / Distinction | Passing |
| **80.00% – 89.99%** | `A` | Excellent | Passing |
| **70.00% – 79.99%** | `B` | Good / Commendable | Passing |
| **60.00% – 69.99%** | `C` | Satisfactory / Average | Passing |
| **50.00% – 59.99%** | `D` | Minimum Passing Grade | Passing |
| **0.00% – 49.99%** | `F` | Fail / Unsatisfactory | Non-Passing |

---

## 6. Fault-Tolerance & Atomic Persistence

- **Atomic File Writing**: When saving records, the system writes first to `students_data.csv.tmp` and performs an atomic replace operation (`os.replace`). This guarantees that if power is lost or the process is killed during a write, the original database is never left in a truncated or corrupted state.
- **Self-Healing CSV Parsing**: If invalid rows are introduced via external modification, the loader skips corrupted entries and loads all recoverable records safely without raising fatal exceptions.

---

## 7. Sample CLI Execution

```text
========================================================================
             STUDENT ACADEMIC REGISTRY & PERFORMANCE SUMMARY             
========================================================================
No.   | Student ID     | Student Name               | Marks      | Grade 
------------------------------------------------------------------------
1     | 234            | Devyanshu Negi             | 99.00      | A+    
2     | STU101         | Jane Doe                   | 94.50      | A+    
3     | STU102         | John Smith                 | 78.20      | B     
4     | STU103         | Alex Johnson               | 88.00      | A     
========================================================================
                       PERFORMANCE ANALYTICS                      
------------------------------------------------------------------------
 Total Enrolled Students : 4
 Class Average Marks     : 89.92 / 100.00 (Grade: A)
 Passing Rate            : 100.0%
 Highest Achiever        : Devyanshu Negi (ID: 234) - 99.00 marks [A+]
 Lowest Achiever         : John Smith (ID: STU102) - 78.20 marks [B]
------------------------------------------------------------------------
 Grade Distribution Breakdown:
  ->  A+: 2 | A: 1 | B: 1 | C: 0 | D: 0 | F: 0
========================================================================
```

---

## 8. Conclusion

The **Student Grade Management System** fulfills all requirements for a dependable, portable, and zero-dependency terminal application. Its modular structure, comprehensive input validation, and atomic storage ensure robust operation across any standard Python environment.
