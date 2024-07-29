import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.application import MIMEApplication
import datetime
import os

# Email configuration
SMTP_SERVER = 'smtp.example.com'  # Replace with your SMTP server
SMTP_PORT = 587  # Port for SMTP (587 for TLS, 465 for SSL)
USERNAME = 'your_email@example.com'  # Your email address
PASSWORD = 'your_password'  # Your email password
SENDER_EMAIL = 'your_email@example.com'
RECEIVER_EMAIL = 'recipient@example.com'
SUBJECT = 'Daily Report'

# File to attach (ensure this file exists or generate it)
REPORT_FILE = 'daily_report.pdf'

def create_report():
    # Generate your report here
    with open(REPORT_FILE, 'w') as file:
        file.write('This is a sample daily report.')

def send_email():
    # Create a MIME object
    msg = MIMEMultipart()
    msg['From'] = SENDER_EMAIL
    msg['To'] = RECEIVER_EMAIL
    msg['Subject'] = SUBJECT
    
    # Body of the email
    body = 'Please find the daily report attached.'
    msg.attach(MIMEText(body, 'plain'))
    
    # Attach the report file
    with open(REPORT_FILE, 'rb') as file:
        part = MIMEApplication(file.read(), Name=os.path.basename(REPORT_FILE))
    part['Content-Disposition'] = f'attachment; filename="{os.path.basename(REPORT_FILE)}"'
    msg.attach(part)
    
    # Send the email
    try:
        with smtplib.SMTP(SMTP_SERVER, SMTP_PORT) as server:
            server.starttls()
            server.login(USERNAME, PASSWORD)
            server.send_message(msg)
            print('Email sent successfully!')
    except Exception as e:
        print(f'Failed to send email: {e}')

if __name__ == "__main__":
    create_report()
    send_email()
