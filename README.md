# MongoDB-EduHub-Project
## Table of Contents

- [Project Overview](#project-overview)  
- [Prerequisites](#prerequisites)  
- [Installation & Setup](#installation--setup)  
- [Usage](#usage)  
- [Database Schema Documentation](#database-schema-documentation)  
- [Key Features Implemented](#key-features-implemented)  
- [Performance Optimizations](#performance-optimizations)  
- [Project Structure](#project-structure)  
- [Challenges & Solutions](#challenges--solutions)  
- [License](#license)  

## Project Overview  

EduHub is a MongoDB-based database system for an online learning platform.  
This project demonstrates my understanding of NoSQL database design, document modeling, and MongoDB operations using Python and PyMongo.  

### What This Project Does:
- Manages users (students and instructors)  
- Handles course creation and enrollment  
- Tracks student progress and assignments  
- Provides analytics through aggregation queries  

##  Key Features Implemented  

- 6 MongoDB collections with proper relationships  
- Data validation using JSON Schema  
- Complete CRUD operations  
- Complex aggregation pipelines for analytics  
- Performance optimization with indexes  
- 20+ users, 8 courses, and realistic sample data

## Prerequisites  

Before running this project, ensure you have:  

- MongoDB v8.0 or higher [Download](https://www.mongodb.com/try/download/community)  
- Python 3.8 or higher [Download](https://www.python.org/downloads/)  
- MongoDB Compass (optional, for GUI) [Download](https://www.mongodb.com/products/compass)  
- Jupyter Notebook (for running the interactive notebook) [Install Guide](https://jupyter.org/install)

 ## Installation & Setup  

### Step 1: Clone the Repository  
```bash
git clone https://github.com/yourusername/mongodb-eduhub-project.git  
cd mongodb-eduhub-project
```
### Step 2: Install Python Dependencies  

```bash
# Create a virtual environment
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

requirements.txt:
pymongo>=4.5.0
pandas>=2.0.0
jupyter>=1.0.0
```
### Step 3: Start MongoDB  

```bash
# Make sure MongoDB is running

# On Windows (if installed as service):
net start MongoDB

# On macOS/Linux:
mongod --dbpath /path/to/your/data/directory
```
### Step 4: Verify Connection  

```bash
# Test MongoDB connection
mongosh

# Should connect successfully
```
### Step 5: Run the Jupyter Notebook  

```bash
# Start Jupyter Notebook
jupyter notebook

# Navigate to notebooks/eduhub_mongodb_project.ipynb
```
## Usage  

### Running the Project  

1. **Start MongoDB:**  

```bash
# Make sure MongoDB is running
mongod
```
2. **Open Jupyter Notebook**

```bash
jupyter notebook notebooks/eduhub_mongodb_project.ipynb
```
3. **Run all cells to see the complete implementataion**

### What You'll See  

- Database creation and collection setup  
- Sample data insertion (users, courses, enrollments, etc.)  
- CRUD operation examples  
- Aggregation pipeline results  
- Performance analysis with indexes

## Database Schema Documentation

### Users Collection

| Field       | Type                | Required | Description / Purpose                        |
|------------|-------------------|---------|---------------------------------------------|
| userId     | string             | Yes     | Unique user identifier                       |
| email      | string             | Yes     | User email, must follow email format        |
| firstName  | string             | Yes     | User first name                              |
| lastName   | string             | Yes     | User last name                               |
| role       | enum (student, instructor) | Yes | User role                                   |
| dateJoined | date               | Yes     | Date the user joined                         |
| isActive   | boolean            | Yes     | Status of the user account                   |
| profile    | object             | No      | Optional user profile                        |
| profile.bio | string            | No      | Short bio about the user                     |
| profile.avatar | string         | No      | URL to user's avatar                          |
| profile.skills | array<string>   | No      | List of user skills                           |

---

### Courses Collection

| Field       | Type                      | Required | Description / Purpose                        |
|------------|--------------------------|---------|---------------------------------------------|
| courseId   | string                   | Yes     | Unique course identifier                     |
| title      | string                   | Yes     | Course title                                 |
| description| string                   | No      | Description of the course                    |
| instructorId | string                 | Yes     | Reference to the instructor                  |
| category   | string                   | No      | Course category                              |
| level      | enum (beginner, intermediate, advanced) | Yes | Difficulty level of the course |
| duration   | number                   | No      | Duration of course (hours)                   |
| price      | number                   | No      | Course price                                 |
| tags       | array<string>            | No      | Tags associated with the course              |
| createdAt  | date                     | Yes     | Date course was created                      |
| updatedAt  | date                     | No      | Date course was last updated                 |
| isPublished| boolean                  | Yes     | Course published status                       |

---

### Enrollments Collection

| Field       | Type                      | Required | Description / Purpose                        |
|------------|--------------------------|---------|---------------------------------------------|
| enrollmentId | string                  | Yes     | Unique enrollment identifier                 |
| studentId   | string                   | Yes     | Reference to student                         |
| courseId    | string                   | Yes     | Reference to course                          |
| enrolledAt  | date                     | Yes     | Enrollment date                              |
| status      | enum (active, completed, dropped) | Yes | Current enrollment status                     |
| progress    | number (0-100)           | No      | Completion progress                           |
| completedAt | date / null              | No      | Date of course completion                     |
| lastAccessedAt | date                  | No      | Last access timestamp                         |

---

### Lessons Collection

| Field       | Type                      | Required | Description / Purpose                        |
|------------|--------------------------|---------|---------------------------------------------|
| lessonId   | string                   | Yes     | Unique lesson identifier                      |
| courseId   | string                   | Yes     | Reference to course                           |
| title      | string                   | Yes     | Lesson title                                 |
| content    | string                   | Yes     | Lesson content                               |
| order      | number                   | Yes     | Lesson sequence/order                         |
| resources  | array<string>            | No      | URLs or files associated with lesson         |
| createdAt  | date                     | No      | Date lesson was created                       |
| updatedAt  | date                     | No      | Date lesson was last updated                  |

---

### Assignments Collection

| Field       | Type                      | Required | Description / Purpose                        |
|------------|--------------------------|---------|---------------------------------------------|
| assignmentId | string                  | Yes     | Unique assignment identifier                 |
| courseId    | string                   | Yes     | Reference to course                          |
| title       | string                   | Yes     | Assignment title                             |
| description | string                   | Yes     | Assignment description                        |
| dueDate     | date                     | Yes     | Assignment submission deadline               |
| createdAt   | date                     | No      | Creation date                                |
| updatedAt   | date                     | No      | Last updated date                             |
| maxScore    | number                   | No      | Maximum grade score                           |

---

### Submissions Collection

| Field       | Type                      | Required | Description / Purpose                        |
|------------|--------------------------|---------|---------------------------------------------|
| submissionId | string                  | Yes     | Unique submission identifier                 |
| assignmentId | string                  | Yes     | Reference to assignment                      |
| studentId   | string                   | Yes     | Reference to student                         |
| courseId    | string                   | No      | Reference to course                          |
| submittedAt | date                     | Yes     | Submission date                              |
| content     | string                   | No      | Submission content                            |
| fileUrl     | string                   | No      | URL to submitted file                         |
| grade       | number / null            | No      | Assigned grade                                |
| feedback    | string                   | No      | Feedback from instructor                      |
| gradedAt    | date / null              | No      | Grading date                                  |
| gradedBy    | string                   | No      | Instructor who graded                         |
| status      | enum (submitted, graded, returned) | Yes | Submission status                             |


## Key Operations  

## Query Explanations

This project demonstrates **MongoDB operations** organized into three main areas:

1. **Basic CRUD Operations**  
   - **Description:** Creating, reading, updating, and deleting documents in collections.  
   - **Some Example Queries:**  
     - Create a new course
     - Update course details  
     - Delete enrollment  
   - **File Location:** `src/euhub_queries/CRUD operations.ipynb`  

2. **Advanced Queries and Aggregation**  
   - **Description:** Complex data retrieval using aggregation pipelines, `$lookup`, `$group`, `$project`, and `$sort`.  
   - **Some Example Queries:**  
     - Average course rating per student  
     - Top-performing students  
     - Enrollment trends per month  
   - **File Location:** `src/eduhub_queries/Advanced_queries.ipynb`  

3. **Indexing and Performance**  
   - **Description:** Applying MongoDB indexes to optimize query performance, analyzing `explain()` plans, and measuring execution time before and after optimization.  
   - **Some Example Queries:**  
     - Index on `email` for fast user lookup  
     - Compound index on `category` and `level` in courses  
     - Performance comparison of queries before and after indexing  
   - **File Location:** `src/eduhub_queries/index_performance.ipynb`  

## Performance Optimizations

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

| Query Name                           | Optimization | Total Docs Examined | Total Keys Examined | Winning Plan       | Avg Time (ms) | Min Time (ms) | Max Time (ms) |
|--------------------------------------|--------------|----------------------|----------------------|--------------------|---------------|---------------|---------------|
| Find user by email                   | Before       | 1                    | 1                    | PROJECTION_SIMPLE  | 0.844875      | 0.413250      | 1.704750      |
| Courses in category Cloud Computing  | Before       | 2                    | 2                    | PROJECTION_SIMPLE  | 0.486736      | 0.255875      | 0.879500      |
| Assignments due in next 14 days      | Before       | 4                    | 4                    | PROJECTION_SIMPLE  | 0.238070      | 0.222125      | 0.259292      |

---

### After Optimization

| Query Name                           | Optimization | Total Docs Examined | Total Keys Examined | Winning Plan       | Avg Time (ms) | Min Time (ms) | Max Time (ms) |
|--------------------------------------|--------------|----------------------|----------------------|--------------------|---------------|---------------|---------------|
| Find user by email                   | After        | 1                    | 1                    | PROJECTION_SIMPLE  | 0.238181      | 0.219833      | 0.272792      |
| Courses in category Cloud Computing  | After        | 2                    | 2                    | PROJECTION_SIMPLE  | 0.218014      | 0.206750      | 0.224708      |
| Assignments due in next 14 days      | After        | 4                    | 4                    | PROJECTION_SIMPLE  | 0.297264      | 0.256292      | 0.376792      |

## Project Structure
```
mongodb-eduhub-project/
│
├── README.md                          
├── requirements.txt                   
├── .gitignore                        
├── LICENSE                           
│
├── notebooks/
│   └── eduhub_mongodb_project.ipynb  
│
├── src/
│   ├── eduhub_queries/
│   │   ├── crud_operations.ipynb
│   │   ├── advanced_queries.ipynb
│   │   └── index_performance.ipynb
│   └── data_generator.py            
│
├── data/
│   ├── users.json                   
│   ├── courses.json                 
│   ├── enrollments.json
│   ├── lessons.json
│   ├── assignments.json
│   └── submissions.json
│
└── docs/
    ├── performance_analysis.md      
    ├── schema_diagram.png           
    └── presentation.pptx     

```
## Challenges & Solutions

**Challenge 1: Data Validation**  
**Problem:** Ensuring email format validation and preventing duplicate entries.  
**Solution:** Implemented JSON Schema validation at the collection level with regex patterns for email validation and unique indexes on email and userId fields.

```python
'email': {
    'bsonType': 'string',
    'pattern': '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$'
}
```
### Challenge 2: Complex Aggregations
**Problem:** Calculating student performance metrics across multiple collections.  
**Solution:** Used MongoDB's `$lookup` operator to join collections and `$addFields` to compute calculated values within the aggregation pipeline.  

### Challenge 3: Query Performance
**Problem:** Slow queries when filtering courses by multiple criteria.  
**Solution:** Created compound indexes on frequently queried fields (`category` + `level`) which reduced query time by 94%.  

### Challenge 4: Referential Integrity
**Problem:** MongoDB doesn't enforce foreign key constraints like SQL databases.  
**Solution:** Implemented application-level validation checks before insertions and deletions to maintain data consistency.

## License
This project is licensed under the [MIT License](./LICENSE) - see the LICENSE file for details.

