##Email Classification with PII Masking API

This project uses a machine learning model to classify emails into categories such as **Change**, **Incident**, **Problem**, or **Request**, while also masking Personally Identifiable Information (PII) from the email content. The final solution is deployed as a FastAPI web service accessible via ngrok.

---

###Features

* Email classification using **TF-IDF + SVM**
* PII masking from email body (e.g., phone numbers, emails)
* REST API built with **FastAPI**
* Hosted on **ngrok** for public testing

---

###Setup Instructions

> You can run this project directly in **Google Colab**.

1. **Clone or open the notebook** in Google Colab.

2. **Install required packages** (already handled in the notebook):

   ```python
   !pip install fastapi nest_asyncio pyngrok uvicorn scikit-learn joblib
   ```

3. **Add your ngrok auth token** in the cell:

   ```python
   # Replace YOUR_AUTH_TOKEN with your actual token
   !ngrok config add-authtoken YOUR_AUTH_TOKEN
   ```

4. **Run the notebook** cells in order. Once the FastAPI app is launched, you’ll see a **public URL** from ngrok.

---

###How to Test the API

Send a `POST` request to the `/classify` endpoint at the ngrok URL:

```bash
curl -X POST <NGROK_URL>/classify \
  -H "Content-Type: application/json" \
  -d '{"email_body": "Please reset my password, my email is john.doe@example.com"}'
```

**Response:**

```json
{
  "input_email_body": "Please reset my password, my email is john.doe@example.com",
  "list_of_masked_entities": {
    "EMAIL": ["john.doe@example.com"]
  },
  "masked_email": "Please reset my password, my email is <EMAIL>",
  "category_of_the_email": "Request"
}
```

---

### Model Details

* **Vectorizer**: TF-IDF (`TfidfVectorizer`)
* **Classifier**: SVM (`SVC(kernel='linear')`)
* **Accuracy**: \~77% on test set

For comparison, a Naive Bayes model was also trained and saved.

---

###Project Files

* `email_classifier_notebook.ipynb` – Main notebook
* `svm_model.pkl` – Trained model
* `tfidf_vectorizer.pkl` – Trained vectorizer

---

### Note

* Do **not** commit your `ngrok` auth token to public repositories.
* The public URL changes every time you run the notebook.
* The original dataset used in this project is not included in the repository due to confidentiality. To test or run the project, please use your own dataset with a similar structure, or create a sample dataset with columns like "email" and "type" representing the email content and its category respectively.

---
