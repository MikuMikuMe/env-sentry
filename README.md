# env-sentry

Creating a comprehensive Python program for "env-sentry," a real-time environmental hazard monitoring and alert system, involves several steps. This program will hypothetically use environmental data and provide alerts when conditions are predicted to result in hazards. We'll simulate this using random data generation since real data input mechanisms are not specifiable without further specifics.

Here's an outline and a simple version of the `env-sentry` tool:

```python
import random
import time
import logging
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

# Configure logging for the application.
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

# Email configuration for alerts
EMAIL_ADDRESS = "your_email@example.com"
EMAIL_PASSWORD = "your_password"
ALERT_EMAIL = "alert_recipient@example.com"
SMTP_SERVER = "smtp.example.com"
SMTP_PORT = 587

def send_email_alert(subject, message):
    """Send an email alert with the specified subject and message."""
    try:
        msg = MIMEMultipart()
        msg['From'] = EMAIL_ADDRESS
        msg['To'] = ALERT_EMAIL
        msg['Subject'] = subject
        msg.attach(MIMEText(message, 'plain'))

        server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
        server.starttls()
        server.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
        text = msg.as_string()
        server.sendmail(EMAIL_ADDRESS, ALERT_EMAIL, text)
        server.quit()

        logging.info("Successfully sent the alert email.")
    except Exception as e:
        logging.error("Failed to send email alert: %s", e)

def get_environment_data():
    """Simulate getting real-time environmental data."""
    # Simulated data for temperature, humidity, and air quality index (AQI)
    data = {
        'temperature': random.uniform(-10, 50),  # Celsius
        'humidity': random.uniform(0, 100),      # Percentage
        'AQI': random.randint(0, 500)            # Air Quality Index
    }
    
    # Log the generated data.
    logging.info("Generated environment data: %s", data)
    
    return data

def evaluate_hazards(data):
    """Evaluate environmental hazards based on the data provided."""
    hazards = []
    
    # Check temperature hazards
    if data['temperature'] < -5 or data['temperature'] > 40:
        hazards.append(f"Temperature hazard: {data['temperature']}°C")
    
    # Check humidity hazards
    if data['humidity'] < 20 or data['humidity'] > 80:
        hazards.append(f"Humidity hazard: {data['humidity']}%")
    
    # Check air quality hazards
    if data['AQI'] > 150:  # 151 is unhealthy
        hazards.append(f"Air Quality Index hazard: {data['AQI']}")
    
    return hazards

def monitor_environment():
    """Continuously monitor the environment for hazards."""
    while True:
        try:
            env_data = get_environment_data()  # Get simulated environmental data
            hazards = evaluate_hazards(env_data)  # Evaluate the hazards
            
            if hazards:
                alert_message = "\n".join(hazards)
                logging.warning("Hazards detected: %s", alert_message)
                send_email_alert("Environmental Hazard Alert", alert_message)
            else:
                logging.info("Environment conditions are safe.")
            
            # Monitor at regular intervals (e.g., every 10 seconds)
            time.sleep(10)
        except Exception as e:
            logging.error("An error occurred during monitoring: %s", e)

if __name__ == "__main__":
    logging.info("Starting env-sentry monitoring tool.")
    monitor_environment()
```

### Key Components
- **Data Simulation:** Randomly generates temperature, humidity, and AQI data.
- **Hazard Evaluation:** Checks the data against predefined safe ranges to identify potential hazards.
- **Alert System:** Sends an email alert if hazards are detected using SMTP.
- **Logging:** Provides logs for monitoring and debugging purposes.
- **Error Handling:** Catches exceptions to prevent the application from crashing unexpectedly.

### Note
- Replace placeholders with your actual email and SMTP server details.
- This is a basic simulation. Integration with real sensors and robust data handling would be necessary for a production-ready application.
- Ensure environment variables or secure key management for sensitive information like email credentials in a real-world application.