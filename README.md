<!DOCTYPE html>
<html lang="ru">
  <head>
    <meta charset="UTF-8" />
    <title>Регистрация команды</title>

    <style>
      body {
        font-family: Arial, sans-serif;
        background: #f0f4f8;
        margin: 0;
        padding: 0;
      }

      .container {
        max-width: 720px;
        margin: 40px auto;
        background: #fff;
        padding: 30px;
        border-radius: 12px;
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
      }

      h2 {
        text-align: center;
        color: #0388e8;
      }

      label {
        display: block;
        margin-top: 16px;
        font-weight: bold;
      }

      input {
        width: 100%;
        padding: 10px;
        margin-top: 6px;
        border: 1px solid #ccc;
        border-radius: 6px;
        font-size: 14px;
      }

      input:focus {
        border-color: #0388e8;
        outline: none;
      }

      input.error {
        border-color: #e53935;
        background: #fff5f5;
      }

      small {
        display: block;
        font-size: 12px;
        color: #666;
        margin-top: 3px;
      }

      .error-message {
        display: none;
        color: #e53935;
        font-size: 13px;
        margin-top: 4px;
      }

      button {
        margin-top: 20px;
        width: 100%;
        padding: 12px;
        background: #0388e8;
        color: #fff;
        border: none;
        border-radius: 6px;
        font-size: 16px;
        cursor: pointer;
      }

      button:hover {
        background: #026fc2;
      }

      .add-btn {
        margin-top: 10px;
        background: #0388e8;
        width: auto;
        padding: 8px 14px;
        font-size: 14px;
      }
    </style>
  </head>

  <body>
    <div class="container">
      <h2>Регистрация команды</h2>

      <form
        id="registrationForm"
        method="POST"
        action="https://script.google.com/macros/s/AKfycbyXlAg5TQK2pBGcZZzTXQ6LdvbSNKIDzO2eILz1RjMzu9qK6vp-Jd_ttNn4_nJPmZLh/exec"
        target="hidden_iframe"
        onsubmit="submitted = true; return validateForm();"
      >
        <label>ФИО сопровождающего</label>
        <input type="text" name="supervisorName" required />

        <label>Телефон сопровождающего</label>
        <input
          type="tel"
          name="supervisorPhone"
          placeholder="+7 (999) 123-45-67"
          required
        />

        <label>Email сопровождающего</label>
        <input type="email" name="supervisorEmail" required />

        <label>Город / населённый пункт</label>
        <input
          type="text"
          name="city"
          placeholder="г. Москва, с. Ивановка"
          required
        />
        <small>Используйте префиксы: г., с., п. и т.д.</small>

        <label>Школа (полное официальное название)</label>
        <input type="text" name="school" placeholder='МАОУ СОШ "4"' required />

        <label>Класс</label>
        <input type="text" name="class" required />

        <label>Название команды</label>
        <input type="text" name="teamName" id="teamName" required />
        <div id="teamError" class="error-message"></div>

        <label>Участники (Фамилия Имя)</label>
        <input type="text" name="participant1" required />
        <input type="text" name="participant2" required />
        <input type="text" name="participant3" />
        <input type="text" name="participant4" />
        <input type="text" name="participant5" />
        <input type="text" name="participant6" />
        <small
          >Можно оставить некоторые поля пустыми, если участников меньше
          6</small
        >

        <label>Учителя, подготовившие команду</label>
        <div id="teachers">
          <input type="text" name="teacher1" required />
        </div>
        <button type="button" class="add-btn" onclick="addTeacher()">
          + Добавить учителя
        </button>

        <button type="submit">Отправить</button>
      </form>

      <iframe
        name="hidden_iframe"
        style="display: none"
        onload="handleResponse()"
      ></iframe>
    </div>

    <script>
      let submitted = false;

      function validateForm() {
        clearTeamError();
        return true;
      }

      function handleResponse() {
        if (!submitted) return;

        const iframe = document.getElementsByName("hidden_iframe")[0];
        const text = iframe.contentDocument.body.innerText.trim();
        submitted = false;

        if (text === "Success") {
          alert("Регистрация прошла успешно!");
          document.getElementById("registrationForm").reset();
          clearTeamError();
          return;
        }

        if (text.includes("название команды")) {
          showTeamError(text);
          return;
        }

        alert(text);
      }

      function showTeamError(message) {
        const input = document.getElementById("teamName");
        const error = document.getElementById("teamError");

        input.classList.add("error");
        error.textContent = message;
        error.style.display = "block";

        input.scrollIntoView({ behavior: "smooth", block: "center" });
        input.focus();
      }

      function clearTeamError() {
        const input = document.getElementById("teamName");
        const error = document.getElementById("teamError");

        input.classList.remove("error");
        error.style.display = "none";
        error.textContent = "";
      }

      document
        .getElementById("teamName")
        .addEventListener("input", clearTeamError);

      function addTeacher() {
        const teachers = document.getElementById("teachers");
        const count = teachers.children.length + 1;
        const input = document.createElement("input");
        input.type = "text";
        input.name = "teacher" + count;
        input.required = true;
        teachers.appendChild(input);
      }
    </script>
  </body>
</html>
