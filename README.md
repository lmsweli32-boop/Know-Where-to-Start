# Know-Where-to-Start 


# Task Manager Exploration and Understanding

**Language:** Python
**Repo:** Starter code on GitLab


## **Part 1: Understanding Project Structure**

### Initial Exploration

* I looked at the directory structure and saw folders like `src/`, `tests/`, and `utils/`.
* Configuration file: `requirements.txt` lists dependencies like `Flask`, `SQLAlchemy`, and `pandas`.
* Main files:

  * `app.py` – seemed like the application entry point.
  * `models.py` – contains class definitions for `Task`, `TaskStatus`, `TaskPriority`.
  * `routes.py` – handles HTTP endpoints.
  * `utils/` – helper functions for file handling and data manipulation.

### Initial Understanding

* **Technologies/Frameworks:** Python, Flask (web framework), SQLAlchemy (ORM), pandas (for data processing).
* **Main Components:**

  * Models: Representing domain entities.
  * Routes/Controllers: Handling API requests.
  * Utilities: File operations, data formatting.
  * App entry: Bootstrapping the Flask server.

### AI Prompt Insights

* Confirmed that `app.py` is the main entry point.
* Identified architectural pattern: **MVC-like** with models, controllers/routes, and utilities.
* Clarified misconceptions: I initially thought all data transformations were in routes, but AI clarified some logic lives in `services/`.

---

## **Part 2: Finding Feature Implementation**

### Initial Search

* Searched for: `"export"`, `"csv"`, `"file"`, `"write"`, `"save"`.
* Found `utils/file_utils.py` with functions like `write_to_csv(data, filename)` and `serialize_task(task)`.
* Noted `routes/task_routes.py` had `/tasks/download` endpoint calling `file_utils.write_to_csv`.

### Hypothesis

* Task export should integrate with `utils/file_utils.py`.
* The `/tasks/download` route is the likely controller endpoint to extend.
* Existing components to modify: `routes/task_routes.py`, `utils/file_utils.py`.

### AI Prompt Insights

* AI helped map existing patterns for exporting tasks.
* Suggested checking `services/task_service.py` for data preparation logic before exporting.
* Confirmed correct layers to implement new export feature: controller → service → utility.

### Implementation Plan

1. Add a function `export_tasks_to_csv` in `task_service.py` to prepare tasks.
2. Reuse `write_to_csv` in `file_utils.py`.
3. Add a new route `/tasks/export` in `task_routes.py`.
4. Update tests in `tests/test_task_export.py`.

---

## Understanding Domain Model

## Initial Understanding

* Core entities:

  * **Task** – represents a task with attributes: `id`, `title`, `description`, `status`, `priority`, `due_date`.
  * **TaskStatus** – Enum: `Pending`, `In Progress`, `Completed`, `Abandoned`.
  * **TaskPriority** – Enum: `Low`, `Medium`, `High`.

* Initial diagram sketch:

```
Task
 ├── id
 ├── title
 ├── description
 ├── status -> TaskStatus
 ├── priority -> TaskPriority
 └── due_date
```

## AI Prompt Insights

* Clarified relationships: `Task` references `TaskStatus` and `TaskPriority` enums.
* Suggested testing understanding: AI asked, “What happens if a high-priority task is overdue?” → highlighted business rule exceptions.

## Initial Understanding 

* **Task** – the main unit of work.
* **TaskStatus** – current state of a task.
* **TaskPriority** – importance level of a task.
* **Overdue** – tasks past `due_date`.

##  Practical Application**

### New Business Rule

**Rule:** Tasks overdue > 7 days are marked `Abandoned` unless `High` priority.

### Implementation Plan

* **Files to modify:**

  * `services/task_service.py` – add a function to evaluate overdue tasks.
  * `models.py` – verify TaskStatus updates.
  * Optional: `scheduled_tasks.py` – for automated checks.

* **Steps:**

  1. Fetch tasks with `due_date < today - 7 days`.
  2. Filter out high-priority tasks.
  3. Update `status` to `Abandoned`.
  4. Persist updates to the database.
  5. Write tests to ensure the rule works correctly.

* **Questions for team:**

  * Should this run automatically as a cron job or manually?
  * Are there notifications when a task is marked abandoned?

## Reflection

* AI prompts helped identify entry points and patterns for implementation.
* Still unsure about existing automated processes for scheduled tasks.
* Next steps: examine `scheduled_tasks.py` and review database update flows.

## **Final Reflection**

## Insights Gained

* **Part 1:** Learned the project follows an MVC-like pattern.
* **Part 2:** Learned how to locate feature implementation by searching controllers → services → utilities.
* **Part 3:** Understanding the domain model clarified the business logic for task prioritization and statuses.
* **Part 4:** Planning a new feature becomes manageable once the architecture and domain rules are clear.

## Strategies for Future Unfamiliar Code

* Start with **directory overview and config files**.
* Search for keywords related to the feature.
* Map **controllers → services → utilities** for any new functionality.
* Sketch **entity diagrams** to understand the domain quickly.


