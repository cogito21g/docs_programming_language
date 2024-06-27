### 11. 실전 예제와 프로젝트

#### 11.2. API 호출 및 데이터 처리

API(Application Programming Interface)는 웹 애플리케이션이 서버와 통신할 수 있게 해주는 인터페이스입니다. API를 통해 데이터를 요청하고 처리하여 웹 애플리케이션에 동적으로 표시할 수 있습니다. 이번 예제에서는 Fetch API를 사용하여 외부 API에서 데이터를 가져와 화면에 표시하는 방법을 학습하겠습니다.

##### 1. 프로젝트 구조

```
weather-app/
├── index.html
├── style.css
└── app.js
```

##### 2. HTML 작성

`index.html` 파일을 작성하여 기본적인 구조를 만듭니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather App</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>Weather App</h1>
    <form id="weather-form">
      <input type="text" id="city-input" placeholder="Enter city" required>
      <button type="submit">Get Weather</button>
    </form>
    <div id="weather-result"></div>
  </div>
  <script src="app.js"></script>
</body>
</html>
```

##### 3. CSS 작성

`style.css` 파일을 작성하여 기본적인 스타일을 적용합니다.

```css
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

.container {
  background: white;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
  width: 300px;
}

h1 {
  text-align: center;
}

form {
  display: flex;
  justify-content: space-between;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

button {
  padding: 10px;
  border: none;
  background: #28a745;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background: #218838;
}

#weather-result {
  margin-top: 20px;
  text-align: center;
}
```

##### 4. JavaScript 작성

`app.js` 파일을 작성하여 OpenWeatherMap API를 사용하여 날씨 정보를 가져오고, 화면에 표시하는 기능을 구현합니다.

```javascript
document.addEventListener('DOMContentLoaded', () => {
  const weatherForm = document.getElementById('weather-form');
  const cityInput = document.getElementById('city-input');
  const weatherResult = document.getElementById('weather-result');

  weatherForm.addEventListener('submit', (event) => {
    event.preventDefault();
    const city = cityInput.value;
    getWeather(city);
    cityInput.value = '';
  });

  async function getWeather(city) {
    const apiKey = 'YOUR_API_KEY'; // OpenWeatherMap API 키를 여기에 입력하세요.
    const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

    try {
      const response = await fetch(apiUrl);
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      const data = await response.json();
      displayWeather(data);
    } catch (error) {
      console.error('Fetch error:', error);
      weatherResult.textContent = 'Error fetching weather data';
    }
  }

  function displayWeather(data) {
    const { name, main, weather } = data;
    weatherResult.innerHTML = `
      <h2>${name}</h2>
      <p>Temperature: ${main.temp}°C</p>
      <p>Weather: ${weather[0].description}</p>
    `;
  }
});
```

##### 연습문제와 해답

1. **날씨 API 호출 및 결과를 화면에 표시하세요.**

   ```javascript
   async function getWeather(city) {
     const apiKey = 'YOUR_API_KEY';
     const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

     try {
       const response = await fetch(apiUrl);
       if (!response.ok) {
         throw new Error('Network response was not ok');
       }
       const data = await response.json();
       displayWeather(data);
     } catch (error) {
       console.error('Fetch error:', error);
       weatherResult.textContent = 'Error fetching weather data';
     }
   }

   function displayWeather(data) {
     const { name, main, weather } = data;
     weatherResult.innerHTML = `
       <h2>${name}</h2>
       <p>Temperature: ${main.temp}°C</p>
       <p>Weather: ${weather[0].description}</p>
     `;
   }
   ```

2. **날씨 정보가 없는 경우 오류 메시지를 표시하세요.**

   ```javascript
   async function getWeather(city) {
     const apiKey = 'YOUR_API_KEY';
     const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

     try {
       const response = await fetch(apiUrl);
       if (!response.ok) {
         throw new Error('Network response was not ok');
       }
       const data = await response.json();
       if (data.cod === '404') {
         weatherResult.textContent = 'City not found';
         return;
       }
       displayWeather(data);
     } catch (error) {
       console.error('Fetch error:', error);
       weatherResult.textContent = 'Error fetching weather data';
     }
   }
   ```

3. **날씨 데이터를 더 다양한 정보로 확장하여 표시하세요.**

   ```javascript
   function displayWeather(data) {
     const { name, main, weather, wind, sys } = data;
     weatherResult.innerHTML = `
       <h2>${name}, ${sys.country}</h2>
       <p>Temperature: ${main.temp}°C</p>
       <p>Weather: ${weather[0].description}</p>
       <p>Humidity: ${main.humidity}%</p>
       <p>Wind Speed: ${wind.speed} m/s</p>
     `;
   }
   ```
