import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.neural_network import MLPClassifier
from sklearn.metrics import confusion_matrix, ConfusionMatrixDisplay, accuracy_score
from joblib import dump

# Load dataset
dataset = pd.read_csv("sensor_data1.csv")

# Display info
print(dataset.info())

# Drop missing values
dataset = dataset.dropna()

# Show dataset
print(dataset)

# Label counts
print(dataset['label'].value_counts())

# Label encoding
label_mapping = {label: idx for idx, label in enumerate(dataset['label'].unique())}
dataset['label'] = dataset['label'].map(label_mapping)

print(dataset.info())

# Features & labels
X = dataset[['Ax', 'Ay', 'Az', 'Gx', 'Gy', 'Gz']].values
y = dataset['label'].values

# Split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Model
mlp_classifier = MLPClassifier(hidden_layer_sizes=(100,), max_iter=500, random_state=42)
mlp_classifier.fit(X_train, y_train)

# Predictions
y_train_pred = mlp_classifier.predict(X_train)
y_test_pred = mlp_classifier.predict(X_test)

# Loss curve
train_loss = mlp_classifier.loss_curve_

plt.figure(figsize=(8, 4))
plt.plot(train_loss, label='Training Loss')
plt.title('Training Loss')
plt.xlabel('Iterations')
plt.ylabel('Loss')
plt.legend()
plt.show()

# Confusion matrix
cm = confusion_matrix(y_test, y_test_pred)
disp = ConfusionMatrixDisplay(confusion_matrix=cm, display_labels=list(label_mapping.keys()))
disp.plot(cmap=plt.cm.Blues)
plt.title('Confusion Matrix')
plt.show()

# Accuracy
print("Training Accuracy:", accuracy_score(y_train, y_train_pred))
print("Testing Accuracy:", accuracy_score(y_test, y_test_pred))

# Save model
dump(mlp_classifier, 'mlp_classifier.joblib')
