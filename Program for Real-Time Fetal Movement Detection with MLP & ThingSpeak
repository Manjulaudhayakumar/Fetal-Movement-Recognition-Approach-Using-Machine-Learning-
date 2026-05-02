import time import requests
import numpy as np from joblib import load

# Function to retrieve data from ThingSpeak
def get_thingspeak_data(channel_id, read_api_key):
try:
url = f'https://api.thingspeak.com/channels/{channel_id}/feeds.json' params = {'api_key': read_api_key, 'results': 1} # Get the latest entry response = requests.get(url, params=params)
print("URL:", response.url) # Print the URL used for the request if response.status_code == 200:
data = response.json()
print("Received Data:", data) # Print the received data return data
else:
print("Error accessing ThingSpeak API. Status code:", response.status_code) print("Response content:", response.content)
return None
except Exception as e:
print("An error occurred during data retrieval:", str(e)) return None
# Function to load model from joblib file def load_model(file_path):
try:
model = load(file_path) return model
except Exception as e:
print("Error loading model:", str(e)) return None
# Function to classify data using the loaded model def classify_data(model,Ax,Ay,Az,Gx,Gy,Gz):
if Ax is not None and Ay is not None:
input_features = np.array([[Ax,Ay,Az,Gx,Gy,Gz]]) prediction = model.predict(input_features)
return int(prediction[0]) else:
print("Warning: Temperature or air quality values are None. Skipping classification.") return None
 

# Function to send classification result to ThingSpeak
# Function to send classification result to ThingSpeak field3
def send_classification_result(channel_id, write_api_key, classification_result): try:
url = f'https://api.thingspeak.com/update.json'
params = {'api_key': write_api_key, 'field7': classification_result} response = requests.post(url, params=params)
if response.status_code == 200:
print("Classification result sent successfully to ThingSpeak field5.") else:
print("Failed to send classification result to ThingSpeak field3. Status code:", response.status code)
print("Response content:", response.content) except Exception as e:
print("An error occurred during data submission:", str(e)) # Main function
def main():
channel_id = '2803234'
read_api_key = 'G767RW2BT7KYVXT5' write_api_key = 'N93FFS9WCVXM5C7U' model_file = 'mlp_classifier.joblib'
# Load the classifier model classifier_model = load_model(model_file) if classifier_model is None:
print("Failed to load classifier model. Exiting.") return

while True: # Run continuously # Get data from ThingSpeak
data = get_thingspeak_data(channel_id, read_api_key)

if data is not None:
# Extract temperature and air quality from the data dictionary if needed entry = data['feeds'][0]
Ax = entry.get('field1') Ay = entry.get('field2') Az = entry.get('field3') Gx = entry.get('field4') Gy = entry.get('field5') Gz = entry.get('field6')


# Convert to float if not None
Ax= float(Ax) if Ax is not None else None Ay= float(Ay) if Ay is not None else None Az= float(Az) if Az is not None else None Gx= float(Gx) if Gx is not None else None
 

Gy= float(Gy) if Gy is not None else None Gz= float(Gz) if Gz is not None else None print(" Ax:", Ax)
print(" Ay:", Ay)
print(" Az:", Az)
print(" Gx:", Gx)
print(" Gy:", Gy)
print(" Gy:", Gz)

# Classify the data if both temperature and air quality are valid classification_result = classify_data(classifier_model, Ax,Ay,Az,Gx,Gy,Gz) print("Classification result:", classification_result)
# Send classification result to ThingSpeak if classification result is not None if classification_result is not None:
send_classification_result(channel_id, write_api_key, classification_result) else:
print("Failed to retrieve valid data from ThingSpeak.")
# Wait for 9 seconds before fetching data again time.sleep(9)
if _name_ == "_main_": main()
 

