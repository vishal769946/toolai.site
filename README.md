<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Image Converter</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background: #f0f2f5;
        }

        .converter-box {
            background: white;
            padding: 2rem;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            text-align: center;
        }

        .upload-area {
            border: 2px dashed #ccc;
            padding: 2rem;
            margin: 1rem 0;
            border-radius: 8px;
            transition: all 0.3s;
        }

        .upload-area:hover {
            border-color: #007bff;
            background: #f8f9fa;
        }

        #fileInput {
            display: none;
        }

        .custom-file-upload {
            background: #007bff;
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            display: inline-block;
            transition: background 0.3s;
        }

        .custom-file-upload:hover {
            background: #0056b3;
        }

        .format-selector {
            margin: 1.5rem 0;
            display: flex;
            gap: 1rem;
            justify-content: center;
        }

        select {
            padding: 8px 12px;
            border-radius: 5px;
            border: 1px solid #ddd;
        }

        #convertBtn {
            background: #28a745;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background 0.3s;
        }

        #convertBtn:hover {
            background: #218838;
        }

        .result-section {
            margin-top: 2rem;
            display: none;
        }

        .image-preview {
            display: flex;
            gap: 1rem;
            justify-content: center;
            margin: 1rem 0;
        }

        .image-container {
            max-width: 300px;
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 5px;
        }

        .converted-image {
            max-width: 100%;
            height: auto;
            border-radius: 4px;
        }

        #downloadLink {
            display: inline-block;
            margin-top: 1rem;
            padding: 10px 20px;
            background: #17a2b8;
            color: white;
            text-decoration: none;
            border-radius: 5px;
            transition: background 0.3s;
        }

        #downloadLink:hover {
            background: #138496;
        }

        @media (max-width: 600px) {
            .image-preview {
                flex-direction: column;
                align-items: center;
            }
        }
    </style>
</head>
<body>
    <div class="converter-box">
        <h1>🖼️ Image Converter</h1>
        
        <div class="upload-area">
            <label class="custom-file-upload">
                📤 Upload Image
                <input type="file" id="fileInput" accept="image/*">
            </label>
            <p>Supported formats: JPEG, PNG, WEBP, BMP, GIF</p>
        </div>

        <div class="format-selector">
            <label>Convert to:</label>
            <select id="formatSelect">
                <option value="jpg">JPG</option>
                <option value="png">PNG</option>
                <option value="webp">WEBP</option>
                <option value="bmp">BMP</option>
            </select>
        </div>

        <button id="convertBtn" onclick="convertImage()">✨ Convert Now</button>

        <div class="result-section" id="resultSection">
            <div class="image-preview">
                <div class="image-container">
                    <h3>Original</h3>
                    <img id="originalImage" class="converted-image">
                </div>
                <div class="image-container">
                    <h3>Converted</h3>
                    <img id="convertedImage" class="converted-image">
                </div>
            </div>
            <a id="downloadLink" download>💾 Download Converted Image</a>
        </div>
    </div>

    <script>
        let originalImage = null;
        let convertedImageBlob = null;

        document.getElementById('fileInput').addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (!file) return;

            if (!file.type.startsWith('image/')) {
                alert('Please select an image file');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                originalImage = new Image();
                originalImage.src = e.target.result;
                document.getElementById('originalImage').src = e.target.result;
                document.getElementById('resultSection').style.display = 'block';
            }
            reader.readAsDataURL(file);
        });

        function convertImage() {
            if (!originalImage) {
                alert('Please select an image first');
                return;
            }

            const format = document.getElementById('formatSelect').value;
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');

            canvas.width = originalImage.width;
            canvas.height = originalImage.height;
            ctx.drawImage(originalImage, 0, 0);

            let mimeType = 'image/jpeg';
            if (format === 'png') mimeType = 'image/png';
            if (format === 'webp') mimeType = 'image/webp';
            if (format === 'bmp') mimeType = 'image/bmp';

            canvas.toBlob(function(blob) {
                convertedImageBlob = blob;
                const url = URL.createObjectURL(blob);
                document.getElementById('convertedImage').src = url;
                
                const downloadLink = document.getElementById('downloadLink');
                downloadLink.href = url;
                downloadLink.download = `converted_image.${format}`;
            }, mimeType, format === 'jpg' ? 0.9 : 1);
        }
    </script>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BMI Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(135deg, #83a4d4, #b6fbff);
            padding: 20px;
        }

        .container {
            background: rgba(255, 255, 255, 0.95);
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 500px;
        }

        h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 1.5rem;
        }

        .unit-switch {
            text-align: center;
            margin-bottom: 1rem;
        }

        .unit-btn {
            padding: 0.5rem 1rem;
            margin: 0 0.5rem;
            border: none;
            border-radius: 20px;
            background: #ecf0f1;
            cursor: pointer;
            transition: all 0.3s;
        }

        .unit-btn.active {
            background: #3498db;
            color: white;
        }

        .input-group {
            margin-bottom: 1.5rem;
        }

        label {
            display: block;
            margin-bottom: 0.5rem;
            color: #34495e;
        }

        input {
            width: 100%;
            padding: 0.8rem;
            border: 2px solid #bdc3c7;
            border-radius: 8px;
            font-size: 1rem;
            transition: border-color 0.3s;
        }

        input:focus {
            outline: none;
            border-color: #3498db;
        }

        .result-container {
            text-align: center;
            padding: 1.5rem;
            border-radius: 10px;
            margin: 1.5rem 0;
            transition: all 0.5s ease;
        }

        .bmi-value {
            font-size: 2.5rem;
            font-weight: bold;
            margin-bottom: 0.5rem;
        }

        .category {
            font-size: 1.2rem;
            margin-bottom: 1rem;
        }

        .recommendations {
            text-align: left;
            padding: 1rem;
            background: #f8f9fa;
            border-radius: 8px;
            margin-top: 1rem;
        }

        button {
            width: 100%;
            padding: 1rem;
            background: #3498db;
            border: none;
            border-radius: 8px;
            color: white;
            font-size: 1.1rem;
            cursor: pointer;
            transition: background 0.3s;
        }

        button:hover {
            background: #2980b9;
        }

        .scale {
            display: flex;
            justify-content: space-between;
            margin: 1rem 0;
            position: relative;
            height: 10px;
            border-radius: 5px;
            overflow: hidden;
        }

        .scale-segment {
            flex: 1;
            height: 100%;
            transition: all 0.3s;
        }

        .pointer {
            position: absolute;
            top: -15px;
            width: 0;
            height: 0;
            border-left: 10px solid transparent;
            border-right: 10px solid transparent;
            border-top: 15px solid #e74c3c;
            transition: left 0.5s ease;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>BMI Calculator</h1>
        
        <div class="unit-switch">
            <button class="unit-btn active" id="metricBtn" onclick="switchUnits('metric')">Metric</button>
            <button class="unit-btn" id="imperialBtn" onclick="switchUnits('imperial')">Imperial</button>
        </div>

        <div id="metricInputs">
            <div class="input-group">
                <label>Height (cm)</label>
                <input type="number" id="heightCm" step="0.1">
            </div>
            <div class="input-group">
                <label>Weight (kg)</label>
                <input type="number" id="weightKg" step="0.1">
            </div>
        </div>

        <div id="imperialInputs" style="display: none;">
            <div class="input-group">
                <label>Height (feet)</label>
                <input type="number" id="heightFeet" step="1">
            </div>
            <div class="input-group">
                <label>Height (inches)</label>
                <input type="number" id="heightInches" step="1">
            </div>
            <div class="input-group">
                <label>Weight (pounds)</label>
                <input type="number" id="weightLbs" step="0.1">
            </div>
        </div>

        <div class="scale">
            <div class="scale-segment" style="background: #3498db"></div>
            <div class="scale-segment" style="background: #2ecc71"></div>
            <div class="scale-segment" style="background: #f1c40f"></div>
            <div class="scale-segment" style="background: #e74c3c"></div>
            <div class="pointer" id="bmiPointer"></div>
        </div>

        <div class="result-container" id="resultContainer">
            <div class="bmi-value" id="bmiValue">-</div>
            <div class="category" id="bmiCategory">Enter your measurements</div>
            <div class="recommendations" id="recommendations"></div>
        </div>

        <button onclick="calculateBMI()">Calculate BMI</button>
    </div>

    <script>
        let currentUnit = 'metric';

        function switchUnits(unit) {
            currentUnit = unit;
            document.getElementById('metricBtn').classList.toggle('active', unit === 'metric');
            document.getElementById('imperialBtn').classList.toggle('active', unit === 'imperial');
            document.getElementById('metricInputs').style.display = unit === 'metric' ? 'block' : 'none';
            document.getElementById('imperialInputs').style.display = unit === 'imperial' ? 'block' : 'none';
        }

        function calculateBMI() {
            let height, weight;
            
            if(currentUnit === 'metric') {
                height = document.getElementById('heightCm').value / 100;
                weight = document.getElementById('weightKg').value;
            } else {
                const feet = document.getElementById('heightFeet').value;
                const inches = document.getElementById('heightInches').value;
                height = (feet * 12 + inches) * 0.0254;
                weight = document.getElementById('weightLbs').value * 0.453592;
            }

            if(!height || !weight || height <= 0 || weight <= 0) {
                alert('Please enter valid measurements');
                return;
            }

            const bmi = weight / (height * height);
            displayResult(bmi);
        }

        function displayResult(bmi) {
            const resultContainer = document.getElementById('resultContainer');
            const bmiValue = document.getElementById('bmiValue');
            const bmiCategory = document.getElementById('bmiCategory');
            const recommendations = document.getElementById('recommendations');
            const pointer = document.getElementById('bmiPointer');

            bmiValue.textContent = bmi.toFixed(1);
            
            let category, color, advice;
            if(bmi < 18.5) {
                category = 'Underweight';
                color = '#3498db';
                advice = 'Consider consulting a nutritionist to develop a healthy weight gain plan.';
            } else if(bmi < 25) {
                category = 'Normal Weight';
                color = '#2ecc71';
                advice = 'Maintain your healthy lifestyle with balanced diet and regular exercise.';
            } else if(bmi < 30) {
                category = 'Overweight';
                color = '#f1c40f';
                advice = 'Consider moderate weight loss through diet and exercise. Consult a healthcare provider.';
            } else {
                category = 'Obese';
                color = '#e74c3c';
                advice = 'Consult a healthcare professional for weight management strategies.';
            }

            // Update display
            resultContainer.style.backgroundColor = `${color}15`;
            bmiCategory.textContent = category;
            bmiCategory.style.color = color;
            recommendations.innerHTML = `
                <strong>Recommendations:</strong>
                <ul style="margin-top: 0.5rem; padding-left: 1rem;">
                    <li>${advice}</li>
                    <li>Regular physical activity (150 minutes/week)</li>
                    <li>Balanced diet with plenty of fruits and vegetables</li>
                </ul>
            `;

            // Update pointer position
            const scaleWidth = document.querySelector('.scale').offsetWidth;
            const position = Math.min(Math.max((bmi - 15) / (40 - 15) * scaleWidth, 0), scaleWidth);
            pointer.style.left = `${position - 10}px`;
        }

        // Initial setup
        switchUnits('metric');
    </script>
</body>
</html>
