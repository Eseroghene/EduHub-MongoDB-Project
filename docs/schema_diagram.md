```mermaid
erDiagram
    USERS {
      string userId PK
      string email
      string firstName
      string lastName
      string role
      date   dateJoined
      bool   isActive
    }

    COURSES {
      string courseId PK
      string title
      string description
      string category
      string level
      string instructorId FK
      date   updatedAt
      bool   isPublished
    }

    ENROLLMENTS {
      string enrollmentId PK
      string studentId FK
      string courseId FK
      date   enrolledAt
      string status
    }

    ASSIGNMENTS {
      string assignmentId PK
      string courseId FK
      string content
      int    order
      float  maxScore
    }

    LESSONS {
      string lessonId PK
      string courseId FK
      string title
      string content
      string status
    }

    SUBMISSIONS {
      string submissionId PK
      string assignmentId FK
      string studentId FK
      string content
      float  grade
      date   submittedAt
    }

    USERS ||--o{ COURSES : teaches
    USERS ||--o{ ENROLLMENTS : enrolls
    USERS ||--o{ SUBMISSIONS : submits
    COURSES ||--o{ ENROLLMENTS : contains
    COURSES ||--o{ ASSIGNMENTS : has
    COURSES ||--o{ LESSONS : includes
    ASSIGNMENTS ||--o{ SUBMISSIONS : receives