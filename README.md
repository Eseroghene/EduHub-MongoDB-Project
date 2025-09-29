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



