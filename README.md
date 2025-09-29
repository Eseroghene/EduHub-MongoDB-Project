# EduHub-MongoDB-Project
## Table of Contents

- [Project Overview](#project-overview)  
- [Prerequisites](#prerequisites)  
- [Installation & Setup](#installation--setup)  
- [Usage](#usage)  
- [Database Schema](#database-schema)  
- [Key Features Implemented](#key-features-implemented)  
- [Performance Optimizations](#performance-optimizations)  
- [Project Structure](#project-structure)  
- [Challenges & Solutions](#challenges--solutions)  
- [Contact](#contact)  

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

## Database Schema  

### Collections  

**Users – Students and instructors**  
```javascript
{
  "userId": "STU001",
  "email": "student@email.com",
  "firstName": "John",
  "lastName": "Doe",
  "role": "student",
  "dateJoined": ISODate("2024-09-01"),
  "isActive": true
}
```
**Courses – Course catalog**  
```javascript
{
  "courseId": "CS101",
  "title": "Introduction to Python",
  "instructorId": "INST001",
  "category": "Programming",
  "level": "beginner",
  "price": 99.99,
  "isPublished": True
}
```
**Other Collections:** enrollments, lessons, assignments, submissions  

See the Jupyter Notebook (`notebooks/eduhub_mongodb_project.ipynb`) for complete schema details.

## Key Operations  

### CRUD Operations  

**Create a New User:**  
```python
new_user = {
    'userId': 'STU016',
    'email': 'newstudent@email.com',
    'firstName': 'Jane',
    'lastName': 'Doe',
    'role': 'student',
    'dateJoined': datetime.now(),
    'isActive': True
}
db.users.insert_one(new_user)
```
**Find Active Students:**

```python
students = db.users.find({'role': 'student', 'isActive': True})
### Update User Profile

```python
db.users.update_one(
    {'userId': 'STU001'},
    {'$set': {'profile.bio': 'Updated bio'}}
)
```
**Delete Enrollment (Soft Delete);**

```python
db.users.update_one(
    {'userId': 'STU001'},
    {'$set': {'isActive': False}}
)
```
### Advanced Queries  
**Find Courses in Price Range:**  

```python
courses = db.courses.find({
    'price': {'$gte': 50, '$lte': 200},
    'isPublished': True
})
```
**Search Courses by Title:**  

```python
courses = db.courses.find({
    'title': {'$regex': 'python', '$options': 'i'}
})
```
**Aggregation Examples – Enrollment Statistics by Category:**  

```python
pipeline = [
    {'$lookup': {
        'from': 'enrollments',
        'localField': 'courseId',
        'foreignField': 'courseId',
        'as': 'enrollments'
    }},
    {'$group': {
        '_id': '$category',
        'totalEnrollments': {'$sum': {'$size': '$enrollments'}},
        'avgPrice': {'$avg': '$price'}
    }}
]

results = db.courses.aggregate(pipeline)
```
**Student Performance Analysis:**  

```python
pipeline = [
    {'$lookup': {
        'from': 'submissions',
        'localField': 'userId',
        'foreignField': 'studentId',
        'as': 'submissions'
    }},
    {'$addFields': {
        'avgGrade': {'$avg': '$submissions.grade'}
    }},
    {'$sort': {'avgGrade': -1}}
]

top_students = db.users.aggregate(pipeline)
```
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
│   ├── eduhub_queries.py            
│   └── data_generator.py            
│
├── data/
│   ├── sample_data.json             
│   ├── users.json                   
│   ├── courses.json                 
│   └── schema_validation.json       
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

