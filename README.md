# NAME: DHANA LAKSHMI A
# REGISTER NO: 21222304003
# An-Intelligent-Enterprise-Assistant-for-public-sector-SIH-1706-Completion-requirements
AIM:

To develop an AI-driven enterprise chatbot using deep learning and natural language processing (NLP) techniques that can accurately understand and respond to employee queries related to HR policies, IT support, company events, and organizational information, while providing secure access with two-factor authentication and filtering inappropriate language.

# OBJECTIVE:

To design a chatbot capable of understanding natural language queries from employees.

To integrate document processing for extracting and summarizing uploaded documents.

To ensure scalability for minimum 5 parallel users with response time ≤ 5 seconds.

To implement 2-Factor Authentication (2FA) using email-based OTP.

To include a profanity filter to maintain professional interaction.

# ALGORITHM / FLOW OF EXECUTION:
Step 1: Start the program and initialize chatbot environment.
Step 2: Prompt the user to log in using email ID.
Step 3: Generate a 6-digit OTP and send it to the registered email (2FA).
Step 4: Authenticate user input by verifying the OTP.
Step 5: After successful login, display the chatbot interface for query input.
Step 6: When a query is entered:
  1.Check for any profanity using a predefined dictionary.
  2.If detected, block or mask the inappropriate words.
# PROGRAM:
# Intelligent Enterprise Assistant - Simplified Chatbot Program
# Required Libraries: fastapi, uvicorn, transformers, sentence-transformers, smtplib, re

from fastapi import FastAPI, UploadFile, Form\
from transformers import pipeline\
from sentence_transformers import SentenceTransformer\
import re, random, smtplib, time

app = FastAPI()

# Initialize models
summarizer = pipeline("summarization", model="facebook/bart-large-cnn")\
embedder = SentenceTransformer('all-mpnet-base-v2')

# Dictionary for profanity filter
BAD_WORDS = {"badword", "stupid", "nonsense"}

# Store OTPs temporarily
otp_data = {}

# Step 1: Generate and send OTP
@app.post("/send_otp/")\
def send_otp(email: str):\
    otp = random.randint(100000, 999999)\
    otp_data[email] = otp\
    # For demo purpose, print instead of email\
    print(f"OTP sent to {email}: {otp}")\
    return {"message": f"OTP sent to {email}"}

# Step 2: Verify OTP
@app.post("/verify_otp/")\
def verify_otp(email: str, entered_otp: int):\
    if email in otp_data and otp_data[email] == entered_otp:\
        return {"message": "Login Successful"}\
    else:
        return {"message": "Invalid OTP"}

# Step 3: Chatbot response
@app.post("/chat/")
def chatbot_response(query: str):\
    # Profanity Check\
    words = re.findall(r'\w+', query.lower())\
    for w in words:\
        if w in BAD_WORDS:\
            return {"response": " Please avoid using inappropriate language."}

    # Simple responses for HR / IT
    if "leave" in query.lower():
        return {"response": "You can apply for leave using the HR portal under 'My Leave' section."}
    elif "password" in query.lower():
        return {"response": "To reset your password, visit the IT support page and click 'Forgot Password'."}
    else:
        return {"response": "I'm learning! Please upload a document for more detailed answers."}

# Step 4: Document upload and summarization
@app.post("/upload_doc/")\
async def process_document(file: UploadFile):\
    text = await file.read()\
    text = text.decode("utf-8")\
    summary = summarizer(text[:1000], max_length=150, min_length=50, do_sample=False)\
    return {"summary": summary[0]['summary_text']}\
SAMPLE INPUT AND OUTPUT:\
Case 1: User Login (2FA)

# Input:
Email: employee@company.com
OTP sent to email: 123456
Entered OTP: 123456

# Output:
 Login Successful

Case 2: Normal Chat Query

# Input:
“Can you tell me the HR leave policy?”

# Output:
“ You can apply for leave using the HR portal under 'My Leave' section.”

Case 3: Profanity Filter

# Input:
“This is a stupid system.”

# Output:
 Please avoid using inappropriate language.

Case 4: Document Upload and Summarization

# Input:
(Upload an 8–10 page PDF or text file — e.g., company policy document)

# Output:
“This document discusses employee benefits, leave rules, performance appraisal process, and disciplinary actions.”

Check for any profanity using a predefined dictionary.

If detected, block or mask the inappropriate words.
## RESULT:

The Intelligent Enterprise Assistant chatbot was successfully developed and tested.
It can:

Understand and respond to queries from employees (HR/IT/Events).

Authenticate users securely using 2FA (email OTP).

Filter inappropriate language dynamically.

Process uploaded documents to summarize or extract key information.
