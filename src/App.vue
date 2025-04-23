<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mission Log</title>
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      background-color: #111;
      color: #eee;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }

    .container {
      display: flex;
      flex-direction: row;
      width: 100%;
      max-width: 1000px;
      padding: 2rem;
      box-sizing: border-box;
    }

    .left-box, .right-box {
      flex: 1;
      padding: 1rem;
    }

    .right-box {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
    }

    .mission-log {
      background-color: #222;
      padding: 1rem;
      border: 1px solid #444;
      border-radius: 8px;
      width: 100%;
      max-width: 500px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    }

    h1, h2 {
      color: #f39c12;
      margin: 0 0 1rem;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="left-box">
      <!-- Placeholder for other content -->
    </div>
    <div class="right-box">
      <div class="mission-log">
        <h2>Mission Log</h2>
        <p>Entry 001: The crew has landed on Nova Yara and is preparing for the next phase...</p>
        <!-- Additional log entries -->
      </div>
    </div>
  </div>
</body>
</html>
