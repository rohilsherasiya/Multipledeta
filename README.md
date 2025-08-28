
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>To Do List Form with LocalStorage</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body {
      background: linear-gradient(to right, #70f0d5, #6dbae6);
      margin: 0;
      padding: 15px;
    }

    .container {
      max-width: 900px;
      margin: auto;
      background: linear-gradient(to right, #f4de5e, #c5e34f);
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    }

    h2 {
      text-align: center;
      margin-bottom: 20px;
      font-size: 24px;
    }

    input[type="text"],
    input[type="email"],
    input[type="password"] {
      width: 100%;
      padding: 10px 35px 10px 10px;
      margin-bottom: 12px;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 15px;
    }

    .works {
      background: linear-gradient(to right, #ea4a22, #66fe07, #f46c05);
      height: 40px;
      width: 200px;
      border-radius: 20px;
      margin: auto;
      text-align: center;
      color: #fff;
      line-height: 40px;
      font-weight: bold;
    }

    .password-box {
      position: relative;
    }

    .toggle-btn {
      position: absolute;
      right: 10px;
      top: 50%;
      transform: translateY(-50%);
      background: none;
      border: none;
      cursor: pointer;
      font-size: 18px;
    }

    button[type="submit"] {
      width: 100%;
      padding: 12px;
      background: linear-gradient(to right, #fa4383, #e76efa, #ef31a0);
      color: #fff;
      font-size: 16px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-weight: bold;
    }

    button[type="submit"]:hover {
      background: linear-gradient(to right, #fa8343, #86a1eb, #59a133);
    }

    table {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
      overflow-x: auto;
      display: block;
    }

    table,
    th,
    td {
      border: 1px solid #ccc;
    }

    th,
    td {
      padding: 10px;
      text-align: center;
      font-size: 14px;
      word-break: break-word;
    }

    .action-btn {
      margin: 2px;
      padding: 6px 12px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      color: #fff;
      font-size: 13px;
    }

    .edit-btn {
      background: #007bff;
    }

    .delete-btn {
      background: #dc3545;
    }

    /* 📱 Responsive Design */
    @media (max-width: 768px) {
      .container {
        padding: 15px;
      }

      h2 {
        font-size: 20px;
      }

      input,
      button {
        font-size: 14px;
      }

      table {
        font-size: 12px;
      }

      th,
      td {
        padding: 6px;
      }

      .action-btn {
        padding: 4px 8px;
        font-size: 12px;
      }
    }

    @media (max-width: 500px) {
      .works {
        width: 150px;
        font-size: 14px;
        height: 35px;
        line-height: 35px;
      }

      button[type="submit"] {
        font-size: 14px;
        padding: 10px;
      }

      th,
      td {
        font-size: 11px;
      }
    }
  </style>
</head>

<body id="errohil">
  <div class="container" id="ress">
    <h2 class="works">To Do List</h2>
    <form id="todoForm">
      <label>Name:</label>
      <input type="text" id="name" required>

      <label>Email:</label>
      <input type="email" id="email" required>

      <label>Password:</label>
      <div class="password-box">
        <input type="password" id="password" required>
        <button type="button" class="toggle-btn" onclick="togglePassword()">👁️</button>
      </div>

      <div class="gender">
        <label>Gender:</label><br>
        <input type="radio" name="gender" value="Male"> Male
        <input type="radio" name="gender" value="Female"> Female
      </div>

      <div class="hobby">
        <label>Hobby:</label><br>
        <input type="checkbox" value="Study"> Study
        <input type="checkbox" value="Traveling"> Traveling
        <input type="checkbox" value="Sports"> Sports
      </div>

      <button type="submit">Add</button>
    </form>

    <table id="todoTable">
      <thead>
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th>Password</th>
          <th>Gender</th>
          <th>Hobbies</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>

  <script>
    // Background changer
    var words = document.querySelector("#ress");
    var Events = [
      "linear-gradient(to right, red, yellow)",
      "linear-gradient(to right, blue, green)",
      "linear-gradient(to right, purple, pink)",
      "radial-gradient(circle, yellow, green, red)",
      "linear-gradient(to bottom, orange, black, white)",
      "radial-gradient(circle, red, yellow, green)"
    ];
    var index = 0;

    setInterval(() => {
      words.style.background = Events[index];
      index = (index + 1) % Events.length;
    }, 2000); // 2 sec
  </script>

  <script>
    let editIndex = null;

    function togglePassword() {
      const passwordInput = document.getElementById("password");
      const btn = document.querySelector(".toggle-btn");
      if (passwordInput.type === "password") {
        passwordInput.type = "text";
        btn.textContent = "🙈";
      } else {
        passwordInput.type = "password";
        btn.textContent = "👁️";
      }
    }

    const form = document.getElementById("todoForm");
    const tableBody = document.querySelector("#todoTable tbody");

    function getTodos() {
      return JSON.parse(localStorage.getItem("todos")) || [];
    }

    function saveTodos(todos) {
      localStorage.setItem("todos", JSON.stringify(todos));
    }

    function errohil() {
      const todos = getTodos();
      tableBody.innerHTML = "";
      todos.forEach((todo, index) => {
        const row = document.createElement("tr");
        row.innerHTML = `
          <td>${todo.name}</td>
          <td>${todo.email}</td>
          <td>${"*".repeat(todo.password.length)}</td>
          <td>${todo.gender}</td>
          <td>${todo.hobbies.join(", ")}</td>
          <td>
            <button class="action-btn edit-btn" onclick="editRow(${index})">Edit</button>
            <button class="action-btn delete-btn" onclick="deleteRow(${index})">Delete</button>
          </td>
        `;
        tableBody.appendChild(row);
      });
    }

    form.addEventListener("submit", function (e) {
      e.preventDefault();

      const name = document.getElementById("name").value;
      const email = document.getElementById("email").value;
      const password = document.getElementById("password").value;
      const gender = document.querySelector("input[name='gender']:checked")?.value || "N/A";

      const hobbies = [];
      document.querySelectorAll(".hobby input[type='checkbox']:checked").forEach(cb => {
        hobbies.push(cb.value);
      });

      const todos = getTodos();

      if (editIndex !== null) {
        todos[editIndex] = { name, email, password, gender, hobbies };
        editIndex = null;
        form.querySelector("button[type='submit']").textContent = "Add";
      } else {
        todos.unshift({ name, email, password, gender, hobbies });
      }

      saveTodos(todos);
      errohil();
      form.reset();
    });

    function editRow(index) {
      const todos = getTodos();
      const todo = todos[index];

      document.getElementById("name").value = todo.name;
      document.getElementById("email").value = todo.email;
      document.getElementById("password").value = todo.password;

      document.querySelectorAll("input[name='gender']").forEach(radio => {
        radio.checked = radio.value === todo.gender;
      });

      document.querySelectorAll(".hobby input[type='checkbox']").forEach(cb => {
        cb.checked = todo.hobbies.includes(cb.value);
      });

      editIndex = index;
      form.querySelector("button[type='submit']").textContent = "Update";
    }

    function deleteRow(index) {
      const todos = getTodos();
      todos.splice(index, 1);
      saveTodos(todos);
      errohil();
    }

    errohil();
  </script>
</body>
</html>
