import hashlib
import datetime
import uuid
import json
import re
from num2words import num2words

# Helper function to hash passwords
def hash_password(password):
    return hashlib.sha256(password.encode()).hexdigest()

# Helper function to get current timestamp
def current_time():
    return datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")

# Function to send email notifications (dummy implementation)
def send_email(email, subject, message):
    print(f"Email sent to {email} with subject: '{subject}' and message: '{message}'")

# Function to validate password strength
def validate_password(password):
    if (len(password) > 10 and
        re.search(r"[A-Z]", password) and
        re.search(r"[a-z]", password) and
        re.search(r"\d", password) and
        re.search(r"[!@#$%^&*(),.?\":{}|<>]", password)):
        return True
    else:
        print("Password must be more than 10 characters long, "
              "contain at least one uppercase letter, one lowercase letter, "
              "one digit, and one special character.")
        return False

# Function to validate mobile number
def validate_mobile(mobile):
    if re.match(r"^\d{10}$", mobile):
        return True
    else:
        print("Mobile number must be exactly 10 digits.")
        return False

# Function to validate Aadhar number
def validate_aadhar(aadhar):
    if re.match(r"^\d{12}$", aadhar):
        return True
    else:
        print("Aadhar number must be exactly 12 digits.")
        return False

# Function to convert amount to words
def amount_in_words(amount):
    return num2words(amount, lang='en_IN').capitalize() + " rupees only"
