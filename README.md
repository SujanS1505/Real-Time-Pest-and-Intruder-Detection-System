# Real-Time-Pest-and-Intruder-Detection-System
LIBRARIES USED: Here are brief descriptions of each library:

TensorFlow

An open-source machine learning framework primarily used for training and deploying deep learning models. It supports neural networks, image classification, object detection, and more.
Pillow (PIL)

A Python Imaging Library used for image processing tasks such as opening, manipulating, and saving image files in various formats.
wikipedia-api

A Python wrapper for Wikipedia's API, allowing you to fetch and parse Wikipedia content, including page summaries, sections, and metadata.
folium

A library for creating interactive geographical maps using Python. It's commonly used for data visualization with map markers, layers, and geographic data overlays.
geotext

A Python library that extracts country and city names from text, making it useful for location-based data processing.
Gradio

A user-friendly library for creating interactive web interfaces for machine learning models and Python functions. It supports image, text, and audio inputs/outputs.
#Output-1: Multiple images detecting:
![image](https://github.com/user-attachments/assets/74c87eac-ec47-41f6-be88-78ae4bd4eb62)

#output-2:using Pill library image preprocessed,bounding box drawn,and detecting the correct image:
![image](https://github.com/user-attachments/assets/3507aced-2850-4746-b813-fdc1c67ca7ce)

#Output-3:Integrated Map Picture
![image](https://github.com/user-attachments/assets/4a78956c-42f4-4cdd-ab28-cf2879c80022)

OVERALL EXPLANATION:- 🐞 Insect Detection and Mapping System - Project Summary 🌍

This project is a Flask-based web application that utilizes machine learning, image processing, and geolocation mapping to detect insects from uploaded images, provide detailed information about them, and dynamically visualize their common locations on an interactive map.

🚀 Core Functionalities:

Insect Detection using TensorFlow:

The application uses a pre-trained ResNet50 deep learning model to classify insects from uploaded images.
Information Retrieval from Wikipedia:

After detection, the application fetches relevant information, such as general details, eradication methods, and pesticide usage, using the Wikipedia API.
Geographical Location Extraction:

The text from Wikipedia is scanned using GeoText to identify mentions of cities and countries where the insect is commonly found.
Interactive Mapping with Folium:

Detected locations are plotted dynamically on an interactive Folium map, with custom markers based on insect types.
Image Highlighting:

Detected insect regions in the uploaded image are highlighted using bounding boxes with Pillow (PIL).
Dynamic User Interface with Gradio:

The application supports easy interaction for users through an intuitive web interface built with Gradio, allowing users to upload images, view results, and explore detection maps.
🛠️ Technology Stack:

Machine Learning: TensorFlow (ResNet50)
Image Processing: Pillow (PIL)
Information Retrieval: Wikipedia API
Geolocation Mapping: Folium, GeoText
Web Interface: Gradio
Backend Framework: Flask
🌟 Key Features:

Accurate insect identification through deep learning.
Comprehensive details fetched dynamically from Wikipedia.
Real-time visualization of insect locations on a map.
Easy-to-use web interface with multi-image upload support.
Clear bounding boxes highlighting detected insects.
This system is a powerful tool for pest management professionals, researchers, and enthusiasts, offering a streamlined way to identify insects, understand their behavior, and visualize their global distribution. 🐜📊


