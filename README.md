<h1 align="center"> Insurance Premium Prediction </h1>

<p align="center">
  <b>FastAPI + Machine Learning + Scikit-learn</b><br>
  Predict insurance premium categories with confidence scores.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-blue?logo=python">
  <img src="https://img.shields.io/badge/FastAPI-0.129-green?logo=fastapi">
  <img src="https://img.shields.io/badge/ML-Scikit--Learn-orange">
  <img src="https://img.shields.io/badge/Status-Production Ready-success">
</p>

<hr>

<h2>⚡ Overview</h2>

<p>
A real-time <b>Insurance Premium Prediction API</b> built using FastAPI and Machine Learning.
It takes user details and predicts the insurance premium category along with confidence scores.
</p>

<p><i>Validate → Transform → Predict → Respond</i></p>

<hr>

<h2>📁 Repository Structure</h2>

<ul>
  <li><b>app.py</b> – FastAPI application and endpoints.</li>
  <li><b>models/predict.py</b> – Loads the pickled model (<code>models/rc_model.pkl</code>), defines <code>predict_output()</code> and <code>MODEL_VERSION</code>.</li>
  <li><b>config/city_tier.py</b> – Lists of tier 1 and tier 2 cities used to compute <code>city_tier</code>.</li>
  <li><b>schema/user_input.py</b> – Pydantic model (<code>UserInput</code>) for request validation and computed properties.</li>
  <li><b>schema/response.py</b> – Pydantic response model (<code>PredictionResponse</code>).</li>
  <li><b>requirements.txt</b> – Python dependencies for the project.</li>
</ul>

<h2>📁 Project Structure</h2>

<pre>
insurance-premium-prediction/
├── app.py
├── README.md
├── requirements.txt
├── config/
│   └── city_tier.py
├── models/
│   ├── predict.py
│   └── rc_model.pkl   # trained model (binary, may be large)
├── schema/
│   ├── response.py
│   └── user_input.py
├── myenv/              # optional: local virtualenv (do not commit to VCS)
│   ├── Include/
│   ├── Lib/
│   └── Scripts/
└── (other files)
	├── response.py
	└── user_input.py
</pre>

<hr>

<h2>✅ Features</h2>

<ul>
  <li>✔ FastAPI REST API</li>
  <li>✔ Input validation using Pydantic</li>
  <li>✔ Automatic feature engineering (BMI, risk, age group)</li>
  <li>✔ ML model prediction with probability scores</li>
  <li>✔ Clean JSON responses</li>
  <li>✔ Health check endpoint</li>
</ul>

<hr>

<p>
<b>Note:</b> <code>myenv/</code> is a local virtual environment and should generally be added to <code>.gitignore</code>. Replace <code>models/rc_model.pkl</code> with your own trained model when updating the application.
</p>

<hr>

<h2>🚀 Getting Started</h2>

<h3>1. Prerequisites</h3>

<ul>
  <li>Python 3.10+</li>
  <li>pip installed</li>
</ul>

<h3>2. Create & Activate Virtual Environment</h3>

<pre>
python -m venv myenv
.\myenv\Scripts\Activate.ps1
</pre>

<p>If the included environment already exists:</p>

<pre>
(Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned) ; (& .\myenv\Scripts\Activate.ps1)
</pre>

<h3>3. Install Dependencies</h3>

<pre>
pip install --upgrade pip
pip install -r requirements.txt
</pre>

<hr>

<h2>⚙️ Run the Application</h2>

<pre>
uvicorn app:app --reload --host 0.0.0.0 --port 8000
</pre>

<h3>Available Endpoints</h3>

<table>
  <tr>
    <th>Method</th>
    <th>Endpoint</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>GET</td>
    <td>/</td>
    <td>Welcome message</td>
  </tr>
  <tr>
    <td>GET</td>
    <td>/health</td>
    <td>Service status and model information</td>
  </tr>
  <tr>
    <td>POST</td>
    <td>/predict</td>
    <td>Insurance premium prediction</td>
  </tr>
</table>

<hr>

<h2>📥 Example Request</h2>

### Request
```bash
curl -X POST "http://127.0.0.1:8000/predict" \
-H "Content-Type: application/json" \
-d @request.json
```
```json
{
  "age": 34,
  "weight": 78.0,
  "height": 1.75,
  "income_lpa": 6.5,
  "smoker": false,
  "city": "Pune",
  "occupation": "private_job"
}
```

<hr>

<h2>📤 Example Response</h2>

```json
{
  "response": {
    "predicted_category": "Medium",
    "confidence": 0.82,
    "class_probabilities": {
      "Low": 0.12,
      "Medium": 0.82,
      "High": 0.06
    }
  }
}
```

<hr>

<h2>🧠 Schema & Validation Details</h2>

<h3>Input Fields</h3>

<ul>
  <li><b>age</b> – Integer (1–119)</li>
  <li><b>weight</b> – Float (&gt; 0 kg)</li>
  <li><b>height</b> – Float (&gt; 0 and &lt; 2.5 meters)</li>
  <li><b>income_lpa</b> – Annual income in LPA</li>
  <li><b>smoker</b> – Boolean</li>
  <li><b>city</b> – Normalized city name</li>
  <li><b>occupation</b> – Valid occupation category</li>
</ul>

<h3>Computed Features</h3>

<ul>
  <li><b>BMI</b> = weight / height²</li>
  <li><b>Lifestyle Risk</b> = High / Medium / Low</li>
  <li><b>Age Group</b> = Young / Adult / Middle Age / Senior</li>
  <li><b>City Tier</b> = 1 / 2 / 3</li>
</ul>

<hr>

<h2>🤖 Model Behavior</h2>

<p>Model file:</p>

<pre>
models/rc_model.pkl
</pre>

<p><b>models/predict.py</b> exposes:</p>

<ul>
  <li><code>model</code></li>
  <li><code>MODEL_VERSION</code></li>
  <li><code>predict_output(user_input)</code></li>
</ul>

<p>The API automatically prepares:</p>

<ul>
  <li>BMI</li>
  <li>Age Group</li>
  <li>Lifestyle Risk</li>
  <li>City Tier</li>
  <li>Income</li>
  <li>Occupation</li>
</ul>

<p>These features are converted into a Pandas DataFrame and passed to the ML model for prediction.</p>

<hr>

<h2>🔄 Updating the Model</h2>

<ol>
  <li>Train a compatible Scikit-learn model.</li>
  <li>Export it as <code>models/rc_model.pkl</code>.</li>
  <li>Update <code>MODEL_VERSION</code> in <code>models/predict.py</code>.</li>
  <li>Restart the FastAPI server.</li>
</ol>

<hr>

<h2>🛠 Troubleshooting</h2>

<ul>
  <li>Ensure <code>models/rc_model.pkl</code> exists if prediction fails.</li>
  <li>Review Pydantic validation errors for invalid inputs.</li>
  <li>Ensure <code>requirements.txt</code> is UTF-8 encoded.</li>
</ul>

<hr>

<h2>👨‍💻 Developer Notes</h2>

<ul>
  <li>Swagger UI → <code>http://127.0.0.1:8000/docs</code></li>
  <li>ReDoc → <code>http://127.0.0.1:8000/redoc</code></li>
  <li>Health Check → <code>http://127.0.0.1:8000/health</code></li>
</ul>

<hr>

<h2 align="center">💡 Suggested Improvements</h2>

<ul>
  <li>Add unit tests</li>
  <li>Add GitHub Actions CI/CD</li>
  <li>Add Docker support</li>
  <li>Add MLflow monitoring</li>
  <li>Add React or Streamlit frontend</li>
</ul>

<hr>

<p align="center">
  ⭐ Star this repository if you found it useful!
</p>
