<!DOCTYPE html>
<html>
  <head>
    <title>User Form</title>
    <style>
      form {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 50px;
      }
      label {
        margin-bottom: 10px;
      }
      input {
        margin-bottom: 20px;
        padding: 10px;
        border-radius: 5px;
        border: none;
        box-shadow: 1px 1px 5px #ccc;
      }
      input:focus {
        outline: none;
        box-shadow: 1px 1px 5px #007bff;
      }
      button {
        padding: 10px 20px;
        border-radius: 5px;
        border: none;
        background-color: #007bff;
        color: #fff;
        cursor: pointer;
      }
      button:hover {
        background-color: #0062cc;
      }
    </style>
  </head>
  <body>
    <form>
      <label for="name">Name *</label>
      <input type="text" id="name" required>
      <label for="email">Email *</label>
      <input type="email" id="email" required>
      <label for="phone">Phone *</label>
      <input type="tel" id="phone" required>
      <button type="submit">Submit</button>
    </form>
    <script>
      const form = document.querySelector("form");
      const apiUrl = "https://example.com/api/users";

      form.addEventListener("submit", async (event) => {
        event.preventDefault();

        const name = document.getElementById("name").value;
        const email = document.getElementById("email").value;
        const phone = document.getElementById("phone").value;

        const userExists = await getUser(apiUrl, email);

        if (userExists) {
          alert("User found");
        } else {
          const userCreated = await createUser(apiUrl, name, email, phone);
          alert("User created successfully");
        }

        form.reset();
      });

      async function getUser(apiUrl, email) {
        const response = await fetch(`${apiUrl}/?email=${email}`);

        if (response.ok) {
          const data = await response.json();
          return data.length > 0;
        } else {
          throw new Error("Unable to get user");
        }
      }

      async function createUser(apiUrl, name, email, phone) {
        const response = await fetch(apiUrl, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            name,
            email,
            phone,
          }),
        });

        if (response.ok) {
          return true;
        } else {
          throw new Error("Unable to create user");
        }
      }
    </script>
  </body>
</html>
