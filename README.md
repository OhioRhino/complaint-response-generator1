<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Complaint Response & Formal Warning Generator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .container {
            margin-bottom: 20px;
        }
        textarea {
            width: 100%;
            height: 100px;
            margin-top: 10px;
        }
        button {
            margin-top: 10px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
        .output {
            margin-top: 20px;
            padding: 10px;
            background-color: #f1f1f1;
            border: 1px solid #ccc;
        }
    </style>
</head>
<body>

    <h1>Complaint Response & Formal Warning Generator</h1>

    <!-- Complaint Response Section -->
    <div class="container">
        <h2>Generate a Response to a Complaint</h2>
        <label for="complaint">Paste the Complaint Text Here:</label>
        <textarea id="complaint" placeholder="Enter complaint text here..."></textarea>
        <br>
        <button onclick="generateComplaintResponse()">Click for Response</button>
        <button onclick="clearComplaint()">Clear Information</button>
        <div class="output" id="complaintResponse"></div>
    </div>

    <!-- Formal Written Warning Section -->
    <div class="container">
        <h2>Generate a Formal Written Warning</h2>
        <label for="infraction">Describe the Infraction:</label>
        <textarea id="infraction" placeholder="Enter the employee infraction here..."></textarea>
        <br>
        <button onclick="generateFormalWarning()">Write me a formal written warning</button>
        <button onclick="clearInfraction()">Clear Information</button>
        <div class="output" id="formalWarning"></div>
    </div>

    <script>
        // Use this function to send a complaint to the OpenAI API and get a response
        async function generateComplaintResponse() {
            let complaint = document.getElementById('complaint').value;

            // Validate input
            if (!complaint) {
                alert('Please enter complaint text');
                return;
            }

            // Call OpenAI API
            try {
                const response = await callOpenAI([{ role: 'user', content: complaint }]);
                document.getElementById('complaintResponse').innerText = response;
            } catch (error) {
                document.getElementById('complaintResponse').innerText = 'Error generating response: ' + error.message;
            }
        }

        // Use this function to send an infraction description to the OpenAI API and get a formal warning
        async function generateFormalWarning() {
            let infraction = document.getElementById('infraction').value;

            // Validate input
            if (!infraction) {
                alert('Please describe the infraction');
                return;
            }

            // Call OpenAI API
            try {
                const response = await callOpenAI([{ role: 'user', content: `Write a formal written warning for the following infraction: "${infraction}"` }]);
                document.getElementById('formalWarning').innerText = response;
            } catch (error) {
                document.getElementById('formalWarning').innerText = 'Error generating formal warning: ' + error.message;
            }
        }

        // Function to call OpenAI API
        async function callOpenAI(messages) {
            const apiKey = 'YOUR_BACKEND_API_ENDPOINT'; // Move this key to a backend server

            try {
                const response = await fetch(apiKey, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer YOUR_SERVER_GENERATED_TOKEN`
                    },
                    body: JSON.stringify({
                        model: 'gpt-3.5-turbo', // or 'gpt-4' depending on your subscription
                        messages: messages,
                        max_tokens: 150,  // Adjust the token limit as needed
                        temperature: 0.7  // Adjust temperature for creative/factual responses
                    })
                });

                const data = await response.json();

                if (data.choices && data.choices.length > 0) {
                    return data.choices[0].message.content.trim();
                } else {
                    throw new Error('No response from OpenAI');
                }
            } catch (error) {
                throw new Error('Failed to fetch OpenAI response: ' + error.message);
            }
        }

        // Functions to clear the text areas and outputs
        function clearComplaint() {
            document.getElementById('complaint').value = "";
            document.getElementById('complaintResponse').innerText = "";
        }

        function clearInfraction() {
            document.getElementById('infraction').value = "";
            document.getElementById('formalWarning').innerText = "";
        }
    </script>

</body>
</html>
