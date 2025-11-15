<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>To-Do List App</title>

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-family: Arial, sans-serif;
    }

    body {
        background: #f7f7f7;
        display: flex;
        justify-content: center;
        padding-top: 60px;
    }

    .container {
        background: white;
        width: 350px;
        padding: 20px;
        border-radius: 12px;
        box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    }

    h1 {
        text-align: center;
        margin-bottom: 20px;
    }

    .input-area {
        display: flex;
        gap: 10px;
    }

    input {
        flex: 1;
        height: 35px;
        padding: 0 10px;
        border: 1px solid #ccc;
        border-radius: 8px;
        outline: none;
    }

    button {
        padding: 0 15px;
        background: #4caf50;
        border: none;
        color: white;
        border-radius: 8px;
        cursor: pointer;
        transition: 0.3s;
    }

    button:hover {
        opacity: 0.85;
    }

    ul {
        margin-top: 20px;
        list-style: none;
    }

    li {
        display: flex;
        justify-content: space-between;
        background: #f0f0f0;
        padding: 10px;
        border-radius: 8px;
        margin-bottom: 10px;
        transition: 0.3s;
    }

    li.completed {
        text-decoration: line-through;
        background: #d4ffd4;
    }

    .delete {
        background: #ff4b4b;
        color: white;
        border: none;
        padding: 4px 8px;
        border-radius: 5px;
        cursor: pointer;
    }
</style>
</head>
<body>

<div class="container">
    <h1>To-Do List</h1>

    <div class="input-area">
        <input type="text" id="taskInput" placeholder="Add a new task..." />
        <button id="addBtn">Add</button>
    </div>

    <ul id="taskList"></ul>
</div>

<script>
// === Select Elements ===
const taskInput = document.getElementById("taskInput");
const addBtn = document.getElementById("addBtn");
const taskList = document.getElementById("taskList");

// Load tasks on startup
document.addEventListener("DOMContentLoaded", loadTasks);

// Add task on button click
addBtn.addEventListener("click", addTask);

// Add task on Enter key
taskInput.addEventListener("keypress", function (e) {
    if (e.key === "Enter") addTask();
});

function addTask() {
    const taskText = taskInput.value.trim();
    if (taskText === "") return;

    const task = {
        id: Date.now(),
        text: taskText,
        completed: false
    };

    addTaskToDOM(task);
    saveTask(task);

    taskInput.value = "";
}

// === Display Task in DOM ===
function addTaskToDOM(task) {
    const li = document.createElement("li");
    li.dataset.id = task.id;

    if (task.completed) li.classList.add("completed");

    li.innerHTML = `
        <span class="taskText">${task.text}</span>
        <button class="delete">X</button>
    `;

    // Toggle completed
    li.addEventListener("click", function (e) {
        if (e.target.classList.contains("delete")) return;
        li.classList.toggle("completed");
        toggleTask(task.id);
    });

    // Delete task
    li.querySelector(".delete").addEventListener("click", function (e) {
        e.stopPropagation();
        li.remove();
        deleteTask(task.id);
    });

    taskList.appendChild(li);
}

// === Local Storage Functions ===
function saveTask(task) {
    const tasks = JSON.parse(localStorage.getItem("tasks")) || [];
    tasks.push(task);
    localStorage.setItem("tasks", JSON.stringify(tasks));
}

function loadTasks() {
    const tasks = JSON.parse(localStorage.getItem("tasks")) || [];
    tasks.forEach(addTaskToDOM);
}

function toggleTask(id) {
    const tasks = JSON.parse(localStorage.getItem("tasks")) || [];
    const task = tasks.find(t => t.id === id);
    task.completed = !task.completed;
    localStorage.setItem("tasks", JSON.stringify(tasks));
}

function deleteTask(id) {
    let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
    tasks = tasks.filter(t => t.id !== id);
    localStorage.setItem("tasks", JSON.stringify(tasks));
}
</script>

</body>
</html># task-2
