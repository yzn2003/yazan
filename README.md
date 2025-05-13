To fulfill your request, I will create a single HTML file that calculates the solar elevation angle for Mafraq. Mafraq is located approximately at latitude 32.34° N, longitude 36.22° E.

The solar elevation angle depends on date, time, latitude, longitude, and solar declination. I will include an input for date and time in the user's local timezone to calculate the solar elevation at Mafraq for that specific date/time. The calculation will use standard solar position formulas.

I will provide a nice modern UI with inline CSS and JavaScript, making it mobile responsive and visually appealing. The result will update dynamically when the user inputs date and time.

Plan:
- Create a single HTML file with:
  - Inputs: Date and time
  - Static location of Mafraq with latitude (32.34) and longitude (36.22)
  - "Calculate" button or auto-calculation on input change
  - Display solar elevation angle in degrees with a nice interface
  - Use JavaScript for calculation of solar elevation angle using known formulas
  - Style the page with modern CSS with responsive design fitting under 600px height and 350px width without scrolling
  - Ensure usability on mobile browsers

I will now proceed to create this HTML file.


`solar_elevation_mafraq.html`
```html
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
  <title>حساب زاوية ارتفاع الشمس في المفرق</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Cairo&display=swap');
    body {
      font-family: 'Cairo', sans-serif;
      background: linear-gradient(135deg, #fbc7a4, #fa709a);
      margin: 0;
      padding: 20px;
      color: #333;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      min-height: 600px;
      max-width: 350px;
      margin-left: auto;
      margin-right: auto;
      box-sizing: border-box;
    }
    h1 {
      margin-bottom: 10px;
      font-size: 1.6rem;
      color: #fff;
      text-shadow: 0 0 4px rgba(0,0,0,0.3);
      text-align: center;
    }
    label {
      display: block;
      margin: 15px 0 5px;
      color: #fff;
      font-weight: bold;
      font-size: 1.1rem;
    }
    input[type="date"],
    input[type="time"] {
      width: 100%;
      padding: 10px;
      border-radius: 8px;
      border: none;
      font-size: 1rem;
      box-sizing: border-box;
      outline: none;
      font-family: 'Cairo', sans-serif;
    }
    button {
      margin-top: 20px;
      width: 100%;
      padding: 12px;
      border: none;
      border-radius: 12px;
      background-color: #fff;
      color: #fa709a;
      font-weight: 900;
      font-size: 1.2rem;
      cursor: pointer;
      box-shadow: 0 4px 10px rgba(250, 112, 154, 0.6);
      transition: background-color 0.3s ease, color 0.3s ease;
    }
    button:hover {
      background-color: #fa709a;
      color: white;
    }
    .result-container {
      margin-top: 30px;
      background: rgba(255 255 255 / 0.85);
      border-radius: 15px;
      width: 100%;
      padding: 20px;
      text-align: center;
      box-sizing: border-box;
      box-shadow: 0 4px 10px rgba(0,0,0,0.12);
    }
    .result-container span {
      font-size: 2rem;
      font-weight: 900;
      color: #fa709a;
    }
    .credit {
      margin-top: auto;
      font-size: 0.8rem;
      color: rgba(255 255 255 / 0.7);
      text-align: center;
    }
  </style>
</head>
<body>
  <h1>حاسبة زاوية ارتفاع الشمس في المفرق</h1>
  <form id="solarForm" onsubmit="event.preventDefault(); calculateSolarElevation();">
    <label for="dateInput">التاريخ</label>
    <input type="date" id="dateInput" required />
    <label for="timeInput">الوقت (التوقيت المحلي)</label>
    <input type="time" id="timeInput" step="60" required />
    <button type="submit">احسب زاوية ارتفاع الشمس</button>
  </form>
  <div class="result-container" id="result" aria-live="polite" aria-atomic="true" role="region" style="display:none;">
    زاوية ارتفاع الشمس: <span id="elevationAngle">0.0</span> درجة
  </div>
  <div class="credit">الموقع: المفرق، الأردن (خط العرض: 32.34°, خط الطول: 36.22°)</div>

  <script>
    // Constants for Mafraq
    const latitude = 32.34 * Math.PI / 180;   // radians
    const longitude = 36.22;

    function calculateSolarElevation() {
      const dateInput = document.getElementById('dateInput').value;
      const timeInput = document.getElementById('timeInput').value;

      if (!dateInput || !timeInput) {
        alert('يرجى إدخال التاريخ والوقت');
        return;
      }

      // Combine date and time into a JS Date object in local time
      const datetime = new Date(dateInput + 'T' + timeInput);

      // Calculate Julian Day
      const JD = getJulianDay(datetime);
      // Calculate Julian Century
      const JC = (JD - 2451545.0) / 36525;

      // Calculate geometric mean longitude of the sun (deg)
      const geomMeanLongSun = (280.46646 + JC * (36000.76983 + JC*0.0003032)) % 360;

      // Calculate geometric mean anomaly of the sun (deg)
      const geomMeanAnomSun = 357.52911 + JC * (35999.05029 - 0.0001537 * JC);

      // Calculate eccentricity of Earth's
