import hashlib
import tensorflow as tf
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences
from flask import Flask, request, render_template

app = Flask(__name__)


# Route for collecting user input
@app.route('/', methods=['GET', 'POST'])
def collect_user_input():
    if request.method == 'POST':
        user_data = {
            'age': request.form['age'],
            'symptoms': request.form['symptoms'],
            # ... other input fields ...
        }

        # Anonymization and data protection
        anonymized_data = anonymize_data(user_data)

        # Proceed with diagnosis
        diagnosis_result = use_ai_model(loaded_model, anonymized_data)

        return render_template('result.html', diagnosis_result=diagnosis_result)

    return render_template('input_form.html')

def anonymize_data(data):
    anonymized_data = data.copy()

    # Anonymize age using hash function
    anonymized_data['age'] = hash_age(data['age'])

    # Anonymize symptoms by removing specific details
    anonymized_data['symptoms'] = anonymize_symptoms(data['symptoms'])

    return anonymized_data

def hash_age(age):
    # Use a secure hash function to hash the age
    hashed_age = hashlib.sha256(str(age).encode()).hexdigest()
    return hashed_age

def anonymize_symptoms(symptoms):
    # Replace specific symptoms with general categories
    # ... implement your anonymization logic ...
    anonymized_symptoms = "Anonymized Symptoms"
    return anonymized_symptoms

def load_diagnosis_model(model_path):
    model = tf.keras.models.load_model(model_path)
    return model

def use_ai_model(model, user_data):
    text = user_data['symptoms']
    tokenized_text = tokenizer.texts_to_sequences([text])
    padded_text = pad_sequences(tokenized_text, maxlen=max_sequence_length, padding='post')

    # Make predictions using the AI model
    diagnosis_result = model.predict(padded_text)

    return diagnosis_result

# Load the tokenizer and model
model_path = 'diagnosis_model.h5'
tokenizer = Tokenizer()  # Create a new Tokenizer instance
max_sequence_length = 100  # Max sequence length used during training

loaded_model = load_diagnosis_model(model_path)

if __name__ == '__main__':
    app.run(debug=True)
