<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Employee Commute Survey</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen">
  <div class="bg-white p-8 rounded-lg shadow-lg w-full max-w-lg">
    <h1 class="text-2xl font-bold mb-6">Employee Commute Survey</h1>
    
    <div class="space-y-6">
      <!-- Question 1: Commute Method -->
      <div>
        <p class="font-semibold">1. What is your commute method to the office?</p>
        <div class="space-y-2 mt-2">
          <label class="flex items-center">
            <input type="checkbox" name="commute_method" value="London Underground" class="mr-2">
            London Underground
          </label>
          <label class="flex items-center">
            <input type="checkbox" name="commute_method" value="Cycle/scoot" class="mr-2">
            Cycle/scoot
          </label>
          <label class="flex items-center">
            <input type="checkbox" name="commute_method" value="Walk" class="mr-2">
            Walk
          </label>
          <label class="flex items-center">
            <input type="checkbox" name="commute_method" value="National Rail" class="mr-2">
            National Rail
          </label>
          <label class="flex items-center">
            <input type="checkbox" name="commute_method" value="Local Buses" class="mr-2">
            Local Buses
          </label>
          <label class="flex items-center">
            <input type="checkbox" name="commute_method" value="Car/taxi" class="mr-2">
            Car/taxi
          </label>
          <label class="flex items-center">
            <input type="checkbox" name="commute_method" value="Motorbike" class="mr-2">
            Motorbike
          </label>
        </div>
      </div>

      <!-- Question 2: Commute Distance -->
      <div>
        <p class="font-semibold">2. How far are you commuting from?</p>
        <div class="mt-2">
          <input type="range" id="commute_distance" min="0" max="50" value="0" class="w-full">
          <div class="flex justify-between text-sm text-gray-600">
            <span>0 miles</span>
            <span>25 miles</span>
            <span>50 miles</span>
          </div>
          <p class="text-sm text-gray-600 mt-1">Distance: <span id="distance_value">0</span> miles</p>
        </div>
      </div>

      <!-- Question 3: Heat and Power Source -->
      <div>
        <p class="font-semibold">3. How do you heat and power your home?</p>
        <div class="space-y-2 mt-2">
          <label class="flex items-center">
            <input type="radio" name="heat_power" value="Gas" class="mr-2">
            Gas
          </label>
          <label class="flex items-center">
            <input type="radio" name="heat_power" value="Electricity" class="mr-2">
            Electricity
          </label>
          <label class="flex items-center">
            <input type="radio" name="heat_power" value="Solar" class="mr-2">
            Solar
          </label>
        </div>
      </div>

      <!-- Question 4: Heating Usage -->
      <div>
        <p class="font-semibold">4. Percentage of time during the working day that you have the heating on (throughout the year)</p>
        <div class="mt-2">
          <input type="range" id="heating_usage" min="0" max="100" value="0" class="w-full">
          <div class="flex justify-between text-sm text-gray-600">
            <span>0%</span>
            <span>50%</span>
            <span>100%</span>
          </div>
          <p class="text-sm text-gray-600 mt-1">Usage: <span id="heating_value">0</span>%</p>
        </div>
      </div>

      <!-- Question 5: Air Conditioning Usage -->
      <div>
        <p class="font-semibold">5. Percentage of time during the working day that you have the air conditioning on (throughout the year)</p>
        <div class="mt-2">
          <input type="range" id="ac_usage" min="0" max="100" value="0" class="w-full">
          <div class="flex justify-between text-sm text-gray-600">
            <span>0%</span>
            <span>50%</span>
            <span>100%</span>
          </div>
          <p class="text-sm text-gray-600 mt-1">Usage: <span id="ac_value">0</span>%</p>
        </div>
      </div>

      <!-- Buttons -->
      <div class="flex space-x-4">
        <button id="submit_btn" class="w-full bg-blue-500 text-white py-2 rounded-lg hover:bg-blue-600">Submit</button>
        <button id="view_results_btn" class="w-full bg-green-500 text-white py-2 rounded-lg hover:bg-green-600">View Consolidated Results</button>
      </div>
    </div>
  </div>

  <script>
    // Clear existing responses on page load
    localStorage.setItem('surveyResponses', JSON.stringify([]));

    // Update distance value display
    const distanceSlider = document.getElementById('commute_distance');
    const distanceValue = document.getElementById('distance_value');
    distanceSlider.addEventListener('input', () => {
      distanceValue.textContent = distanceSlider.value;
    });

    // Update heating usage value display
    const heatingSlider = document.getElementById('heating_usage');
    const heatingValue = document.getElementById('heating_value');
    heatingSlider.addEventListener('input', () => {
      heatingValue.textContent = heatingSlider.value;
    });

    // Update AC usage value display
    const acSlider = document.getElementById('ac_usage');
    const acValue = document.getElementById('ac_value');
    acSlider.addEventListener('input', () => {
      acValue.textContent = acSlider.value;
    });

    // Handle form submission
    const submitButton = document.getElementById('submit_btn');
    submitButton.addEventListener('click', () => {
      const commuteMethods = Array.from(document.querySelectorAll('input[name="commute_method"]:checked'))
        .map(input => input.value);
      const commuteDistance = parseInt(document.getElementById('commute_distance').value);
      const heatPower = document.querySelector('input[name="heat_power"]:checked')?.value || 'Not selected';
      const heatingUsage = parseInt(document.getElementById('heating_usage').value);
      const acUsage = parseInt(document.getElementById('ac_usage').value);

      // Store the response in localStorage
      const responses = JSON.parse(localStorage.getItem('surveyResponses'));
      responses.push({
        commuteMethods,
        commuteDistance,
        heatPower,
        heatingUsage,
        acUsage
      });
      localStorage.setItem('surveyResponses', JSON.stringify(responses));

      // Disable the submit button after submission
      submitButton.disabled = true;
      submitButton.classList.remove('hover:bg-blue-600');
      submitButton.classList.add('bg-gray-400', 'cursor-not-allowed');

      alert('Response submitted successfully! You can view consolidated results using the button below. Refresh the page to submit another response.');
    });

    // Handle view consolidated results with password protection
    const viewResultsButton = document.getElementById('view_results_btn');
    viewResultsButton.addEventListener('click', () => {
      const password = prompt('Please enter the password to view results:');
      const correctPassword = 'yourpassword123'; // Change this to your desired password

      if (password === correctPassword) {
        const responses = JSON.parse(localStorage.getItem('surveyResponses'));
        if (responses.length === 0) {
          alert('No responses submitted yet.');
          return;
        }

        // Format individual responses
        let result = `Individual Survey Responses (${responses.length} total):\n\n`;
        responses.forEach((response, index) => {
          result += `Response ${index + 1}:\n`;
          result += `1. Commute Methods: ${response.commuteMethods.length > 0 ? response.commuteMethods.join(', ') : 'None selected'}\n`;
          result += `2. Commute Distance: ${response.commuteDistance} miles\n`;
          result += `3. Heat/Power Source: ${response.heatPower}\n`;
          result += `4. Heating Usage: ${response.heatingUsage}%\n`;
          result += `5. Air Conditioning Usage: ${response.acUsage}%\n\n`;
        });

        result += `Please copy this summary and email it to daniel.stoyanov@veretec.co.uk.`;
        alert(result);
      } else {
        alert('Incorrect password. Only authorized personnel can view the results.');
      }
    });
  </script>
</body>
</html>
