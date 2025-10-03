# Performance Analysis  

This document provides a detailed breakdown of query performance **before and after optimization** in the **MongoDB-EduHub-Project**.  


## Optimization Goals
- Reduce query execution time  
- Minimize scanned documents/keys  
- Ensure efficient index usage  



**Indexes Created**

| Collection   | Index                   | Type     | Purpose                  |
|-------------|------------------------|---------|--------------------------|
| users       | email                  | Unique  | Fast user lookup         |
| users       | userId                 | Unique  | Primary identifier       |
| courses     | title, description     | Text    | Full-text search         |
| courses     | category, level        | Compound| Filtered queries         |
| enrollments | studentId, courseId    | Compound| Join operations          |
| assignments | dueDate                | Single  | Date range queries       |



## Query Performance Analysis

### Before Optimization  

| Query Name                          | Optimization | Total Docs | Total Keys | Avg Time (ms) | Min Time (ms) | Max Time (ms) |
|-------------------------------------|--------------|------------|------------|---------------|---------------|---------------|
| Find user by email                  | Before       | 1          | 1          | 0.844875      | 0.413250      | 1.704750      |
| Courses in category Cloud Computing | Before       | 2          | 2          | 0.486736      | 0.255875      | 0.879500      |
| Assignments due in next 14 days     | Before       | 4          | 4          | 0.238070      | 0.222125      | 0.259292      |


### After Optimization  

| Query Name                          | Optimization | Total Docs | Total Keys | Avg Time (ms) | Min Time (ms) | Max Time (ms) |
|-------------------------------------|--------------|------------|------------|---------------|---------------|---------------|
| Find user by email                  | After        | 1          | 1          | 0.238181      | 0.219833      | 0.272792      |
| Courses in category Cloud Computing | After        | 2          | 2          | 0.218014      | 0.206750      | 0.224708      |
| Assignments due in next 14 days     | After        | 4          | 4          | 0.297264      | 0.256292      | 0.376792      |


## Optimization Plan Explanation  

In this project, we tested three key queries **before and after optimization**. The main goal was to reduce execution time, minimize documents/keys scanned, and ensure efficient use of indexes.  

### 1. Find User by Email  
- **Before:** Docs examined = `1`, Keys examined = `1`, Avg time = `0.84 ms`  
- **After:** Docs examined = `1`, Keys examined = `1`, Avg time = `0.23 ms`  
 **Improvement:** Ensured a unique index on `email`, resulting in faster lookups.  

---

### 2. Courses in Category: *Cloud Computing*  
- **Before:** Docs examined = `2`, Keys examined = `2`, Avg time = `0.49 ms`  
- **After:** Docs examined = `2`, Keys examined = `2`, Avg time = `0.21 ms`  
**Improvement:** Added a compound index on `{ category, level }`, making category filtering more efficient.  

---

### 3. Assignments Due in Next 14 Days  
- **Before:** Docs examined = `4`, Keys examined = `4`, Avg time = `0.23 ms`  
- **After:** Docs examined = `4`, Keys examined = `4`, Avg time = `0.29 ms`  
**Observation:** Slightly slower on small data due to index overhead. However, the `dueDate` index ensures scalability for larger datasets by avoiding full collection scans.  

---

### Summary of Optimization  
1. **Unique index on `email`** → Faster user lookups.  
2. **Compound index on `{ category, level }`** → Optimized course filtering.  
3. **Single index on `dueDate`** → Efficient range queries for assignments.  

**Key Takeaway:** Even if improvements look small on a toy dataset, indexing prepares the database for **scalability and high-performance querying** in real-world, large-scale scenarios.  


