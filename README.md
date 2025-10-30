# SpringBoot_Lesson1_Sample6

## Propmt for the Code Agent (Codex, Gemini Code Assistant or Copilot)

**Context**:

You are an AI assistant that helps professionalize Spring Boot applications.

We are upgrading our Task API from Lesson 5. We will add custom queries, introduce a DTO pattern for all API responses, and implement proper 404 error handling.

**Task**:

Refactor the API to use DTOs and add a new query endpoint.

**Constraints**:

Spring Boot version: 3.3.0, Java 17, Maven.

Use a Java record for the DTO.

Use a derived query method.

Implement a global exception handler.

**Steps**:

Create a dto package (com.example.demo.dto). Inside, create a Java record named TaskDto(Long id, String description, boolean completed).

In the TaskRepository interface, add a new derived query method: List<Task> findByCompleted(boolean completed);.

Create a custom exception class ResourceNotFoundException.java that extends RuntimeException.

**Refactor the TaskService**:

a. All public methods that previously returned Task or List<Task> should now return TaskDto or List<TaskDto>.

b. Create private helper methods to map a Task entity to a TaskDto and a List<Task> to a List<TaskDto>.

c. In the getTaskById service method, use .orElseThrow() on the result of repository.findById() to throw a new ResourceNotFoundException("Task not found with id: " + id) if the task does not exist.

d. Implement a new public method findTasksByStatus(boolean completed) that calls the new repository method and maps the result to a List<TaskDto>.

e. Refactor the createTask method. It should still accept the data needed to create a task, save the Task entity, and then return the TaskDto of the newly created task.

**Refactor the TaskController**:

a. Update all method signatures to expect TaskDto in the response.

b. Add a new controller method mapped to GET /tasks with a request parameter completed (e.g., /tasks?completed=true). This method should call the new findTasksByStatus service method. Make the parameter optional. If it's not present, call the findAllTasks method.

**Create a new class GlobalExceptionHandler.java in a new exception package**.

a. Annotate the class with @ControllerAdvice.

b. Create a method that handles ResourceNotFoundException. Annotate it with @ExceptionHandler(ResourceNotFoundException.class).

c. This method should accept the exception as an argument and return a ResponseEntity<String> with the exception's message and an HttpStatus.NOT_FOUND status.

**Deliverables**:

The new TaskDto.java record.

The updated TaskRepository.java interface.

The new ResourceNotFoundException.java class.

The new GlobalExceptionHandler.java class.

The refactored TaskService.java and TaskController.java.

curl commands to test the new search endpoint and the 404 error handling.

**Acceptance Criteria**:

The application compiles and runs.

GET /tasks, POST /tasks, and GET /tasks/{id} now return a JSON object that matches the TaskDto structure (no extra fields from the entity).

A request to GET /tasks/999 (a non-existent ID) now returns an HTTP 404 Not Found status with an error message in the body.

A request to GET /tasks?completed=false returns a JSON array of TaskDto objects for all incomplete tasks.

A request to GET /tasks (without any parameter) returns all tasks.
