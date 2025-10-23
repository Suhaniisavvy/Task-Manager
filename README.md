# Task-Manager
A simple task manager application to create, organize, and track daily tasks with feature like progress tracking. It is built using html,Css, javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TaskMaster - Productivity Tracker</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(135deg, #6e8efb, #a777e3);
            font-family: 'Segoe UI', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            background: white;
            padding: 25px;
            border-radius: 15px;
            width: 350px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.2);
            animation: fadeIn 1s ease-in-out;
        }

        h1 {
            text-align: center;
            color: #444;
            margin-bottom: 20px;
        }

        .task-input {
            display: flex;
            gap: 10px;
        }

        input {
            flex: 1;
            padding: 10px;
            border: 2px solid #a777e3;
            border-radius: 8px;
            outline: none;
            font-size: 14px;
        }

        button {
            padding: 10px 15px;
            background: #6e8efb;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
        }

        button:hover {
            background: #5a75e1;
        }

        ul {
            list-style: none;
            margin-top: 20px;
        }

        li {
            background: #f9f9f9;
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: transform 0.2s ease;
        }

        li:hover {
            transform: scale(1.02);
        }

        .completed {
            text-decoration: line-through;
            opacity: 0.6;
        }

        .action-btns button {
            margin-left: 5px;
            padding: 5px 8px;
            font-size: 12px;
            border-radius: 6px;
            border: none;
            cursor: pointer;
        }

        .edit {
            background: #ffc107;
            color: black;
        }

        .delete {
            background: #dc3545;
            color: white;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.9); }
            to { opacity: 1; transform: scale(1); }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📋 TaskMaster</h1>
        <div class="task-input">
            <input type="text" id="task" placeholder="Enter your task...">
            <button id="addTask">Add Task</button>
        </div>
        <ul id="taskList"></ul>
    </div>

    <script>
        const taskInput = document.getElementById("task");
        const addTaskBtn = document.getElementById("addTask");
        const taskList = document.getElementById("taskList");

        let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

        function renderTasks() {
            taskList.innerHTML = "";
            tasks.forEach((task, index) => {
                const li = document.createElement("li");
                li.innerHTML = `
                    <span class="${task.completed ? 'completed' : ''}" onclick="toggleComplete(${index})">${task.text}</span>
                    <div class="action-btns">
                        <button class="edit" onclick="editTask(${index})">Edit</button>
                        <button class="delete" onclick="deleteTask(${index})">Delete</button>
                    </div>
                `;
                taskList.appendChild(li);
            });
            localStorage.setItem("tasks", JSON.stringify(tasks));
        }

        function addTask() {
            const taskText = taskInput.value.trim();
            if (taskText) {
                tasks.push({ text: taskText, completed: false });
                taskInput.value = "";
                renderTasks();
            } else {
                alert("Please enter a task!");
            }
        }

        function toggleComplete(index) {
            tasks[index].completed = !tasks[index].completed;
            renderTasks();
        }

        function editTask(index) {
            const newText = prompt("Edit task:", tasks[index].text);
            if (newText) {
                tasks[index].text = newText;
                renderTasks();
            }
        }

        function deleteTask(index) {
            if (confirm("Are you sure you want to delete this task?")) {
                tasks.splice(index, 1);
                renderTasks();
            }
        }

        addTaskBtn.addEventListener("click", addTask);
        taskInput.addEventListener("keypress", (e) => {
            if (e.key === "Enter") addTask();
        });

        renderTasks();
    </script>
</body>
</html>
