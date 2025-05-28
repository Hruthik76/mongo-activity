# mongo-activity
# ToDo List Management

This project utilizes a MongoDB collection named `todo_list` to manage a list of tasks with various attributes such as priority, due dates, and completion status.

## Collection: `todo_list`

Each document in the `todo_list` collection represents a task with the following schema:

```json
{
  "task": "string",
  "description": "string",
  "due_date": ISODate,
  "status": "string",          // Example: "pending", "in-progress", "completed"
  "priority": "string",        // Example: "high", "medium", "low"
  "completed": Boolean,        // true if the task is done
  "created_at": ISODate,
  "updated_at": ISODate
}

db.todo_list.insertMany([
  {
    "task": "Prepare project documentation",
    "description": "Write detailed API documentation for the backend",
    "due_date": ISODate("2025-06-12T00:00:00Z"),
    "status": "in-progress",
    "priority": "high",
    "created_at": new Date(),
    "updated_at": new Date()
  },
  {
    "task": "Conduct security audit",
    "description": "Review authentication and authorization mechanisms",
    "due_date": ISODate("2025-06-15T00:00:00Z"),
    "status": "pending",
    "priority": "high",
    "created_at": new Date(),
    "updated_at": new Date()
  }
])

db.todo_list.find({}) // List all tasks

db.todo_list.find({ completed: true }) // List completed tasks

db.todo_list.find({ completed: false }) // List incomplete tasks

// Mark a task as completed
db.todo_list.updateOne(
  { _id: ObjectId("...") },
  { $set: { completed: true } }
)

// Set `completed` to false for high-priority tasks
db.todo_list.updateMany(
  { priority: "high" },
  { $set: { completed: false, updated_at: new Date() } }
)

// Update all tasks' `updated_at` field
db.todo_list.updateMany(
  {},
  { $set: { updated_at: new Date() } }
)

db.todo_list.countDocuments() //count documents

const twentyMinutesAgo = new Date(Date.now() - 20 * 60 * 1000);
db.todo_list.find({ updated_at: { $gte: twentyMinutesAgo } })     //recently updated tasks
