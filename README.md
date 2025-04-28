# 🚢 **Titanic Survival Predictor: Containerized Streamlit App**

**View the source code on GitHub:** [https://github.com/PrakritiRawat/Titanic-Surviva-.git](https://github.com/PrakritiRawat/Titanic-Surviva-.git)

---

## 📌 **Overview**
The **Titanic Survival Prediction Model** is a machine learning application that predicts whether a passenger would have survived the Titanic disaster based on various input features. Built with **Python**, **scikit-learn**, **pandas**, and **Streamlit**, this containerized app ensures seamless deployment and portability via **Docker**.

**LIVE DEMO:** (https://vu5kcnybxrtkvzgxqfyhw4.streamlit.app/)

---

## 📂 **Project Structure**
```bash
Titanic-Prediction-Model/
│── Dockerfile
│── requirements.txt
│── main.py
│── titanic_model.py
│── titanic_model.pkl
```

### **File Descriptions**
- **`main.py`** – Streamlit web interface for user inputs and predictions.
- **`titanic_model.py`** – Script to train and serialize the Random Forest model.
- **`titanic_model.pkl`** – Serialized machine learning model.
- **`requirements.txt`** – Application dependencies.
- **`Dockerfile`** – Container specification for the Streamlit app.

---

## 🤖 **Model Training (`titanic_model.py`)**
1. Load and preprocess the Titanic dataset.
2. Train a **Random Forest Classifier**.
3. Serialize the trained model to `titanic_model.pkl` using `joblib`.

---

## 🎨 **Streamlit Application (`main.py`)**
Interactive UI features:
- Enhanced CSS styling.
- Live prediction from the `.pkl` model.
- Input controls: sliders, dropdowns, text fields.

---

## 🐳 **Docker Setup**
**`Dockerfile`**:
```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY main.py titanic_model.pkl ./
EXPOSE 8501
CMD ["streamlit", "run", "main.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

---

## 🚀 **Running with Docker**
1️⃣ **Build the image**:
```bash
docker build -t titanic-prediction .
```

2️⃣ **Run the container**:
```bash
docker run -d -p 8501:8501 --name titanic_app titanic-prediction
```

3️⃣ **Access the app**:
```
http://localhost:8501
```

---

## 🎯 **Conclusion**
- Containerized ML app with **Streamlit** and **Docker**.
- Predicts Titanic survival based on user inputs.
- Easily deployable via Docker CLI or orchestration.

**Next Steps:**
- Deploy on AWS/GCP
- Enhance UI
- Improve model with additional features

---

**Happy sailing through data!** 🐳🚢

