<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sleep Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 50px;
            background-color: #f4f4f4;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            display: inline-block;
        }
        select, button {
            font-size: 16px;
            padding: 10px;
            margin: 10px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }
        button {
            background-color: #28a745;
            color: white;
            cursor: pointer;
        }
        button:hover {
            background-color: #218838;
        }
        h1, h2 {
            color: #333;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: center;
        }
        th {
            background-color: #28a745;
            color: white;
        }
        .good { background-color: #d4edda; color: green; font-weight: bold; }
        .average { background-color: #fff3cd; color: orange; font-weight: bold; }
        .bad { background-color: #f8d7da; color: red; font-weight: bold; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Sleep Calculator</h1>
        <p>Select your wake-up time:</p>
        <select id="hour"></select> : <select id="minute"></select> 
        <select id="ampm">
            <option value="AM">AM</option>
            <option value="PM">PM</option>
        </select>
        <p>Select your lifestyle:</p>
        <select id="lifestyle">
            <option value="student">Student 📚</option>
            <option value="gym">Gym-Goer 💪</option>
            <option value="office">Office Worker 🏢</option>
            <option value="general">General Wellness 🌿</option>
            <option value="athlete">Athlete 🏅</option>
            <option value="night_shift">Night Shift Worker 🌙</option>
            <option value="parent">Parent 👶</option>
            <option value="freelancer">Freelancer 💻</option>
            <option value="elderly">Elderly 🧓</option>
            <option value="traveler">Traveler ✈️</option>
            <option value="entrepreneur">Entrepreneur 🚀</option>
            <option value="healthcare">Healthcare Worker 🏥</option>
            <option value="baby">Baby 👶</option>
        </select>
        <button onclick="calculateSleepTimes()">Calculate</button>
        <h2>Recommended Sleep Times:</h2>
        <table>
            <thead>
                <tr>
                    <th>Sleep Time</th>
                    <th>Duration</th>
                    <th>Recommendation</th>
                </tr>
            </thead>
            <tbody id="result"></tbody>
        </table>
    </div>

    <script>
        function populateTimeOptions() {
            let hourSelect = document.getElementById('hour');
            let minuteSelect = document.getElementById('minute');
            for (let i = 1; i <= 12; i++) {
                let option = document.createElement('option');
                option.value = i;
                option.textContent = i;
                hourSelect.appendChild(option);
            }
            for (let i = 0; i < 60; i++) {
                let option = document.createElement('option');
                option.value = i;
                option.textContent = i.toString().padStart(2, '0');
                minuteSelect.appendChild(option);
            }
        }

        function calculateSleepTimes() {
            let hour = parseInt(document.getElementById('hour').value);
            let minute = parseInt(document.getElementById('minute').value);
            let period = document.getElementById('ampm').value;
            let lifestyle = document.getElementById('lifestyle').value;
            if (period === "PM" && hour !== 12) hour += 12;
            if (period === "AM" && hour === 12) hour = 0;
            
            let sleepCycles = [9, 7.5, 6, 4.5];
            let recommendations = {
                "baby": ["👶 Essential for growth and brain development.", "🍼 Helps in better mood and immune system.", "😐 May cause irritability and fussiness.", "😴 Leads to developmental delays."],
                "student": ["📖 Boosts memory and learning.", "🧠 Enhances focus and cognitive function.", "😐 May lead to decreased retention.", "📉 Can cause poor academic performance."],
                "gym": ["💪 Helps muscle recovery and strength.", "⚡ Boosts energy levels.", "😐 Slower muscle growth.", "🏋️‍♂️ Can lead to fatigue and injury."],
                "office": ["🖥️ Improves productivity and reduces stress.", "💼 Helps mental clarity.", "😐 May lead to work fatigue.", "😴 Increases risk of burnout."],
                "night_shift": ["🌙 Adjusts circadian rhythm.", "🛌 Supports better sleep quality.", "😐 Can cause mild fatigue.", "⚠️ Increased long-term health risks."],
                "parent": ["👶 Increases patience and energy levels.", "💙 Helps manage stress and fatigue.", "😐 May cause exhaustion.", "💤 Sleep deprivation affects parenting."],
                "athlete": ["🏅 Boosts muscle recovery & energy.", "⚡ Enhances stamina and endurance.", "😐 Delays muscle recovery.", "🏋️‍♂️ Leads to weak performance."],
               "freelancer": ["💻 Boosts creativity and innovation.", "🧠 Improves problem-solving skills.", "😐 Can lead to mental blocks.", "😴 Long work hours may cause burnout."],
    "elderly": ["🧓 Supports heart health and circulation.", "🧠 Improves memory retention and cognitive function.", "😐 May cause occasional fatigue.", "🦴 Lack of activity can weaken joints."],
    "traveler": ["✈️ Reduces jet lag and fatigue.", "⚡ Improves energy levels and stamina.", "😐 Disrupted sleep cycle.", "🕒 Helps balance body clock."],
    "entrepreneur": ["🚀 Enhances decision-making and strategic thinking.", "💡 Improves mental stamina and resilience.", "😐 Stress from high responsibility.", "📉 Poor work-life balance may reduce productivity."],
    "healthcare_worker": ["🏥 Strengthens immunity and endurance.", "🧠 Improves mental clarity under stress.", "😐 Can lead to emotional exhaustion.", "💤 Long shifts may cause burnout."]
};
            
            let resultTable = document.getElementById('result');
            resultTable.innerHTML = "";
            
            sleepCycles.forEach((cycle, index) => {
                let date = new Date();
                date.setHours(hour, minute, 0, 0);
                date.setTime(date.getTime() - cycle * 60 * 60 * 1000);
                let formattedHour = date.getHours() % 12 || 12;
                let newPeriod = date.getHours() >= 12 ? 'PM' : 'AM';
                let recommendation = recommendations[lifestyle] ? recommendations[lifestyle][index] : "💤 Standard sleep duration.";
                let sleepClass = index === 0 ? 'good' : index === 1 ? 'average' : 'bad';
                let row = `<tr>
                    <td>${formattedHour}:${date.getMinutes().toString().padStart(2, '0')} ${newPeriod}</td>
                    <td>${cycle} hours</td>
                    <td class='${sleepClass}'>${recommendation}</td>
                </tr>`;
                resultTable.innerHTML += row;
            });
        }

        populateTimeOptions();
    </script>
</body>
</html>
