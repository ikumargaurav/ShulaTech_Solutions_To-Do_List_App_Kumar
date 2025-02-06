# ShulaTech_Solutions_To-Do_List_App_Kumar
A feature-rich and stylish To-Do List App built with HTML, CSS, and JavaScript. 🚀 Manage your tasks effortlessly 

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced To-Do List</title>
    <link rel="stylesheet" href="style.css">
    <style>
        import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap');

        body {
            font-family: 'Poppins', sans-serif;
            background: #1e1e1e;
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            flex-direction: column;
        }

        .container {
            background: #292929;
            padding: 20px;
            border-radius: 10px;
            width: 400px;
            text-align: center;
            box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.3);
        }

        h1 {
            font-size: 24px;
        }

        .input-section {
            display: flex;
            gap: 10px;
        }

        input,
        select {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 5px;
            outline: none;
        }

        button {
            background: #27ae60;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
        }

        button:hover {
            background: #2ecc71;
        }

        ul {
            list-style: none;
            padding: 0;
            margin-top: 20px;
        }

        li {
            background: #333;
            margin: 5px 0;
            padding: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-radius: 5px;
            cursor: pointer;
        }

        li.completed {
            text-decoration: line-through;
            opacity: 0.6;
        }

        .delete-btn {
            background: red;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 5px;
            cursor: pointer;
        }

        .edit-btn {
            background: orange;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>

<body>
    <div class="container">
        <h1>📝 To-Do List</h1>
        <div class="input-section">
            <input type="text" id="taskInput" placeholder="Add a new task...">
            <input type="date" id="dueDate">
            <select id="priority">
                <option value="Low">Low</option>
                <option value="Medium">Medium</option>
                <option value="High">High</option>
            </select>
            <button onclick="addTask()">Add</button>
        </div>
        <ul id="taskList"></ul>
    </div>
    <script>
        document.addEventListener("DOMContentLoaded", loadTasks);

        function addTask() {
            let taskInput = document.getElementById("taskInput").value.trim();
            let dueDate = document.getElementById("dueDate").value;
            let priority = document.getElementById("priority").value;

            if (taskInput === "") {
                alert("Please enter a task!");
                return;
            }

            let taskList = document.getElementById("taskList");
            let li = document.createElement("li");
            li.innerHTML = `<span>${taskInput} - ${dueDate} (${priority})</span> 
                            <button class='edit-btn' onclick='editTask(this)'>Edit</button>
                            <button class='delete-btn' onclick='deleteTask(this)'>X</button>`;

            li.addEventListener("click", function () {
                this.classList.toggle("completed");
                saveTasks();
            });

            taskList.appendChild(li);
            document.getElementById("taskInput").value = "";
            saveTasks();
        }

        function editTask(button) {
            let taskItem = button.parentElement;
            let taskText = taskItem.querySelector("span").innerText;
            let newTask = prompt("Edit Task:", taskText);
            if (newTask) {
                taskItem.querySelector("span").innerText = newTask;
                saveTasks();
            }
        }

        function deleteTask(button) {
            let taskItem = button.parentElement;
            taskItem.remove();
            saveTasks();
        }

        function saveTasks() {
            let tasks = [];
            document.querySelectorAll("#taskList li").forEach(li => {
                tasks.push({ text: li.querySelector("span").innerText, completed: li.classList.contains("completed") });
            });
            localStorage.setItem("tasks", JSON.stringify(tasks));
        }

        function loadTasks() {
            let storedTasks = localStorage.getItem("tasks");
            if (storedTasks) {
                let tasks = JSON.parse(storedTasks);
                let taskList = document.getElementById("taskList");
                tasks.forEach(task => {
                    let li = document.createElement("li");
                    li.innerHTML = `<span>${task.text}</span> 
                                    <button class='edit-btn' onclick='editTask(this)'>Edit</button>
                                    <button class='delete-btn' onclick='deleteTask(this)'>X</button>`;
                    if (task.completed) li.classList.add("completed");
                    li.addEventListener("click", function () {
                        this.classList.toggle("completed");
                        saveTasks();
                    });
                    taskList.appendChild(li);
                });
            }
        }
    </script>
</body>

</html>

