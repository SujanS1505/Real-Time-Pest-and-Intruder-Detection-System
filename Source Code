import gradio as gr
import tensorflow as tf
from tensorflow.keras.utils import img_to_array
from tensorflow.keras.applications.resnet50 import ResNet50, preprocess_input, decode_predictions
from PIL import Image, ImageDraw
import wikipediaapi
import folium
from geotext import GeoText  # For detecting place names

# Initialize the ResNet50 model
model = ResNet50(weights="imagenet")

# Initialize Wikipedia API
user_agent = "DynamicInsectDetectionApp/1.0 (contact@example.com)"
wiki_wiki = wikipediaapi.Wikipedia(language='en', user_agent=user_agent)

# Define more insects and their colors
INSECT_COLORS = {
    "grasshopper": "green",
    "slug": "purple",
    "bee": "yellow",
    "ant": "cyan",
    "weevil": "orange",
    "snail": "blue",
    "beetle": "brown",
    "leafhopper": "pink",
    "earthworm": "teal",
    "spider": "black",
    "butterfly": "red",         # New insect
    "dragonfly": "blue",        # New insect
    "cockroach": "gray",        # New insect
    "mosquito": "lightblue",    # New insect
    "termite": "darkyellow",    # New insect
    "fly": "purple",            # New insect
    "moth": "white",            # New insect
    "locust": "darkgreen",      # New insect
    "tick": "darkbrown",        # New insect
    "wasp": "yelloworange",     # New insect
}

# Global variable to store locations across multiple images
all_locations = {}

def detect_insect_with_tensorflow(img):
    """Detects the insect type from an image using a pre-trained TensorFlow model."""
    img = img.resize((224, 224))
    img_array = img_to_array(img)
    img_array = preprocess_input(img_array)
    img_array = img_array[None, ...]
    preds = model.predict(img_array)
    decoded_preds = decode_predictions(preds, top=1)[0]
    return decoded_preds[0][1]  # Return top prediction insect name

def fetch_insect_info_and_places(insect_name):
    """Fetches insect information, eradication methods, pesticides, and frequent places."""
    page = wiki_wiki.page(insect_name)
    if not page.exists():
        return f"Details for {insect_name} are not available on Wikipedia.", []

    full_info = page.text
    eradication_keywords = ["control", "management", "eradication", "prevent", "remove", "treatment", "solutions"]
    pesticide_keywords = ["pesticide", "insecticide", "treatment", "chemical", "biological control", "prevention", "measures"]

    def extract_relevant_content(keywords):
        return "\n".join(
            [line.strip() for line in full_info.splitlines() if any(k in line.lower() for k in keywords)]
        ) or "No specific information found."

    # Extract eradication methods and pesticide information
    eradication_methods = extract_relevant_content(eradication_keywords)
    pesticides_used = extract_relevant_content(pesticide_keywords)

    # Extract places using GeoText
    places_detected = GeoText(full_info)
    countries = places_detected.countries
    cities = places_detected.cities
    all_places = set(countries + cities)  # Combine countries and cities
    place_list = list(all_places)  # Convert to a list for mapping purposes

    result = f"**Full Information about {insect_name}:**\n\n{full_info}\n\n"
    result += f"**Eradication Methods and Solutions:**\n{eradication_methods}\n\n"
    result += f"**Pesticides and Precautions:**\n{pesticides_used}\n\n"
    result += f"**Places Found Most Frequently:**\n{', '.join(place_list) or 'No specific places mentioned.'}\n\n"

    return result, place_list

def highlight_insect_on_image(img, insect_name):
    """Highlights the detected insect in the image."""
    draw = ImageDraw.Draw(img)
    width, height = img.size
    box = [
        int(width * 0.2), int(height * 0.2),
        int(width * 0.8), int(height * 0.8)
    ]
    draw.rectangle(box, outline="red", width=5)
    return img

def generate_map(locations):
    """Generates an interactive map with insect detection locations."""
    folium_map = folium.Map(location=[0, 0], zoom_start=2)

    # Loop through all locations and add markers for each insect detected
    for insect_name, locs in locations.items():
        color = INSECT_COLORS.get(insect_name.lower(), "gray")
        for loc in locs:
            # Ensure valid coordinates for each place
            if loc != [0, 0]:  # Skip default invalid coordinates
                folium.Marker(
                    location=loc,
                    popup=f"{insect_name} detected here",
                    icon=folium.Icon(color=color),
                ).add_to(folium_map)

    return folium_map._repr_html_()

def get_coordinates_for_places(places):
    """Simulates or retrieves coordinates for given place names."""
    coordinates = {
        "India": [20.5937, 78.9629],
        "Australia": [-25.2744, 133.7751],
        "Africa": [1.6508, 10.2679],
        "USA": [37.0902, -95.7129],
        "Brazil": [-14.2350, -51.9253],
        "China": [35.8617, 104.1954],
        "France": [46.6034, 1.8883],
        "London": [51.5074, -0.1278],
        "Spain": [40.4637, -3.7492],
        "United States": [37.0902, -95.7129],
        "Asia": [34.0479, 100.6197],
        "Antarctica": [-82.8628, 135.0000],
        "Florissant": [38.8033, -90.4437],
        "Germany": [51.1657, 10.4515],
        "Canada": [56.1304, -106.3468],
        "Japan": [36.2048, 138.2529],
        "Russia": [55.7558, 37.6173],
        "Italy": [41.9028, 12.4964],
        "South Africa": [-30.5595, 22.9375],
        "Mexico": [23.6345, -102.5528],
        "Egypt": [26.8206, 30.8025],
        "Argentina": [-38.4161, -63.6167],
        "United Kingdom": [51.5074, -0.1278],
        "South Korea": [35.9078, 127.7669],
        "Turkey": [38.9637, 35.2433],
        "Thailand": [15.8700, 100.9925],
        "Malaysia": [4.2105, 101.9758],
        "Philippines": [12.8797, 121.7740],
        "Vietnam": [14.0583, 108.2772],
        "Chile": [-35.6751, -71.5430],
        "Colombia": [4.5709, -74.2973],
        "Peru": [-9.19, -75.0152],
        "Pakistan": [30.3753, 69.3451],
        "Nigeria": [9.0820, 8.6753],
        "Bangladesh": [23.685, 90.3563],
        "Indonesia": [-0.7893, 113.9213],
        # New Coordinates
        "New Zealand": [-40.9006, 174.8860],
        "Sweden": [60.1282, 18.6435],
        "Norway": [60.4720, 8.4689],
        "Finland": [61.9241, 25.7482],
        "Denmark": [56.2639, 9.5018],
        "Switzerland": [46.8182, 8.2275],
        "Netherlands": [52.1326, 5.2913],
        "Belgium": [50.8503, 4.3517],
        "Ireland": [53.1424, -7.6921],
        "Poland": [51.9194, 19.1451],
        "Czech Republic": [49.8175, 15.4730],
        "Slovakia": [48.6690, 19.6990],
        "Austria": [47.5162, 14.5501],
        "Hungary": [47.1625, 19.5033],
        "Romania": [45.9432, 24.9668],
        "Bulgaria": [42.7339, 25.4858],
        "Serbia": [44.0165, 21.0059],
        "Croatia": [45.1, 15.2],
        "Bosnia and Herzegovina": [43.9159, 17.6791],
        "Montenegro": [42.7087, 19.3744],
    }
    return [coordinates.get(place, [0, 0]) for place in places]

def process_single_image(img):
    """Processes a single image to detect and annotate the insect."""
    if not img:
        return "No image uploaded.", None, {}

    insect_name = detect_insect_with_tensorflow(img)
    insect_info, places = fetch_insect_info_and_places(insect_name)
    img_with_box = highlight_insect_on_image(img, insect_name)
    locations = {insect_name: get_coordinates_for_places(places)}

    # Add new locations to the global all_locations variable
    for insect_name, locs in locations.items():
        if insect_name not in all_locations:
            all_locations[insect_name] = locs
        else:
            all_locations[insect_name].extend(locs)

    return f"*Detected Insect:* {insect_name}\n\n{insect_info}", img_with_box, locations

def process_multiple_images(img1, img2, img3):
    """Processes multiple images and combines results dynamically."""
    results, images_with_boxes = [], []

    for img in [img1, img2, img3]:
        if img:
            result, img_with_box, _ = process_single_image(img)
            results.append(result)
            images_with_boxes.append(img_with_box)
        else:
            results.append("No image uploaded.")
            images_with_boxes.append(None)

    # Generate map after processing all images, using accumulated locations
    folium_map_html = generate_map(all_locations)
    return (*results, *images_with_boxes, folium_map_html)

# Gradio Interface
iface = gr.Interface(
    fn=process_multiple_images,
    inputs=[
        gr.Image(type="pil", label="Upload Pest Image 1"),
        gr.Image(type="pil", label="Upload Pest Image 2"),
        gr.Image(type="pil", label="Upload Pest Image 3"),
    ],
    outputs=[
        gr.Textbox(label="Insect Detection and Information for Image 1", lines=15),
        gr.Textbox(label="Insect Detection and Information for Image 2", lines=15),
        gr.Textbox(label="Insect Detection and Information for Image 3", lines=15),
        gr.Image(label="Highlighted Image with Bounding Box for Image 1"),
        gr.Image(label="Highlighted Image with Bounding Box for Image 2"),
        gr.Image(label="Highlighted Image with Bounding Box for Image 3"),
        gr.HTML(label="Common Detection Locations Map"),
    ],
    live=True,
)

iface.launch()
