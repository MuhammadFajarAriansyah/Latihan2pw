<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Form Submission</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" 
    rel="stylesheet" 
    integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" 
    crossorigin="anonymous">

    <style>
      body {
        background-color: #f8f9fa;
        color: #333;
    }

    .container {
        margin-top: 50px;
        background-color: #fff;
        border-radius: 10px;
        padding: 30px;
        box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.1);
    }

    .title {
        text-align: center;
        margin-bottom: 30px;
        color: #007bff;
    }

    .form-label {
        font-weight: bold;
    }

    .form-check-label {
        font-weight: normal;
    }

    #successAlert {
        margin-top: 20px;
    }
  </style>
</head>
<body>
    <div class="container">
      <h1 class="title">Form Submission</h1>
      <form id="userForm">
        <div id="errorAlerts"></div>
        <div id="successAlert" class="alert alert-success d-none" role="alert">
            <i class="fas fa-check-circle"></i> Data submitted successfully!
        </div>
      <div class="mt-3">
    </div>
        <form id="userForm">
            <div class="mb-3">
                <label for="name" class="form-label">Name</label>
                <input type="text" class="form-control" id="name" aria-describedby="nameHelp">
            </div>
            <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input type="email" class="form-control" id="email">
            </div>
            <div class="mb-3">
                <label class="form-label">Gender</label>
                <div class="form-check">
                    <input class="form-check-input" type="radio" name="gender" id="male" value="male">
                    <label class="form-check-label" for="male">
                        Male
                    </label>
                </div>
                <div class="form-check">
                    <input class="form-check-input" type="radio" name="gender" id="female" value="female" checked>
                    <label class="form-check-label" for="female">
                        Female
                    </label>
                </div>
            </div>
            <div class="mb-3">
                <label for="status" class="form-label">Status</label>
                <select class="form-control" id="status">
                    <option selected>Status info</option>
                    <option value="active">Active</option>
                    <option value="inactive">Inactive</option>
                </select>
            </div>
            <button type="submit" class="btn btn-primary" >Submit

            </button>
        </form>

    </div>

    <script>
      document.getElementById('userForm').addEventListener('submit', function(event) {
        event.preventDefault(); // Prevent default form submission

        const name = document.getElementById('name').value;
        const email = document.getElementById('email').value;
        const gender = document.querySelector('input[name="gender"]:checked').value;
        const status = document.getElementById('status').value;

        clearErrorAlerts();
        if (!name || !email) {
          showErrorAlert('Please fill in both name and email fields.');
          return;
        }



        const data = {
            name: name,
            email: email,
            gender: gender,
            status: status
        };

        sendData(data);
    });

    const nameInput = document.getElementById('name');
    const emailInput = document.getElementById('email');
    let selectedGender = document.querySelector('input[name="gender"]:checked').value;
    let status = document.getElementById('status').value;

    document.querySelectorAll('input[name="gender"]').forEach(function (radio) {
        radio.addEventListener('change', function () {
            selectedGender = document.querySelector('input[name="gender"]:checked').value;
            console.log('Gender yang dipilih: ' + selectedGender);
        });
    });

    document.getElementById('status').addEventListener('change', function () {
        status = document.getElementById('status').value;
        console.log('Nilai Status:', status);
    });

    function sendData(data) {
      const url = 'https://gorest.co.in/public/v2/users';

      fetch(url, {
          method: 'POST',
          headers: {
              'Content-Type': 'application/json',
              'Authorization': 'Bearer 75220362603200cc24d985e6af1bc55632675cc69e14f8fec70f5f474b0a2b6f'
          },
          body: JSON.stringify(data)
      })
      .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(responseData => {
        console.log(responseData); // Log response data to console
        showSuccessAlert('Data submitted successfully!');
    })
    .catch(error => {
        console.error('Error:', error); // Log any errors to console
        showErrorAlert('An error occurred while submitting data.');
    });
}

function showSuccessAlert(message) {
    const successAlert = document.getElementById('successAlert');
    successAlert.innerHTML = `<i class="fas fa-check-circle"></i> ${message}`;
    successAlert.classList.remove('d-none');
}

function showErrorAlert(message) {
    const errorAlerts = document.getElementById('errorAlerts');
    const alert = document.createElement('div');
    alert.classList.add('alert', 'alert-danger');
    alert.setAttribute('role', 'alert');
    alert.innerHTML = `<i class="fas fa-exclamation-circle"></i> ${message}`;
    errorAlerts.appendChild(alert);
}

function clearErrorAlerts() {
    const errorAlerts = document.getElementById('errorAlerts');
    errorAlerts.innerHTML = ''; // Clear all existing alerts
}
    </script>
</body>
</html>
