# 🥗 Fridge Chef – Your AI Kitchen Assistant

**Building AI course project**

> "One photo, infinite possibilities. Stop food waste before it happens."

## Summary
Fridge Chef is an intelligent application that analyzes your fridge's content to tell you exactly what to cook. Simply upload a photo, and our AI identifies ingredients to generate matching, delicious recipes—no grocery run needed.

## Background
### What problem are we solving?
We all know the struggle: The fridge is full of "leftovers" (half a yogurt, two eggs, a pepper, mustard), but you have no idea what to cook.
*   **Decision Fatigue:** We are tired of making trivial decisions every single day.
*   **Food Waste:** In Germany alone, approx. 11 million tons of food end up in the trash annually. A large part of this happens in private households simply because ideas for utilization are missing.
*   **Healthy Eating:** It's often hard to stick to a diet. With Fridge Chef, users can focus the output on high protein, low calorie, gluten-free, or countless other dietary styles via simple prompts.

### Motivation
We want to make cooking simpler, cheaper, and more sustainable. Instead of throwing food away or ordering takeout unnecessarily, we use technology to be creative with what is already available.

## Data & AI Techniques
This project combines two powerful AI disciplines: **Computer Vision** and **Large Language Models (LLM)**.

1.  **Object Detection (Computer Vision):**
    *   We use a **Convolutional Neural Network (CNN)** to identify ingredients in images.
    *   **Model:** For the prototype, we use **YOLOv8** (You Only Look Once), an extremely fast state-of-the-art model pretrained on the **COCO Dataset** (containing many food items like apples, oranges, broccoli, bottles, etc.).
    *   **Data Source:** COCO Dataset (Common Objects in Context).

2.  **Recipe Generation (NLP):**
    *   The detected ingredients (e.g., `["Egg", "Milk", "Flour"]`) are sent as a text prompt to an **LLM** (like GPT-4o or Llama 3).
    *   The LLM acts as a professional chef to create a structured recipe.

## Tech Stack & Demo
We have created an initial prototype in Python.

### Prerequisites
```bash
pip install ultralytics
# Optional: OpenAI Library for the recipe part
# pip install openai
The Demo Code
This code loads an image, detects food items, and prints them to the console.
code
from ultralytics import YOLO

def detect_ingredients(image_path):
    # 1. Load the pretrained YOLOv8 model (nano version for speed)
    # The model downloads automatically on the first run.
    model = YOLO('yolov8n.pt') 
    
    # 2. Perform prediction on the image
    results = model(image_path)
    
    detected_items = set()
    
    # 3. Process results
    for result in results:
        for box in result.boxes:
            class_id = int(box.cls[0])
            class_name = model.names[class_id]
            confidence = float(box.conf[0])
            
            # Filter only relevant food classes (based on COCO IDs)
            # For the demo, we simply take everything with > 50% confidence
            if confidence > 0.5:
                detected_items.add(class_name)
                
    return list(detected_items)

# --- App Simulation ---
if __name__ == "__main__":
    img_path = "fridge_test.jpg" # Place a test image in this folder
    
    print(f"📸 Analyzing {img_path}...")
    ingredients = detect_ingredients(img_path)
    
    if ingredients:
        print(f"✅ Ingredients found: {', '.join(ingredients)}")
        print("\n--- 👨‍🍳 AI Chef Suggestion ---")
        print(f"How about a salad or stir-fry using {ingredients[0]} and {ingredients[1]}?")
        # In the next step, this would be connected to the ChatGPT API
    else:
        print("❌ No known ingredients detected.")
```
## Use Cases (User Journey)
The Student: End of the month, budget is tight. A photo of the fridge -> The app suggests "Pasta with leftover veggie sauce."
The Family: Parents want to teach children not to waste food. The app turns it into a game ("What can we rescue today?").
The Hobby Cook: Looks for inspiration for unusual combinations based on random finds.
## Challenges & Limitations
Hidden Objects: The camera cannot see what is inside an opaque Tupperware container.
Food Condition: The AI detects a banana, but it does not know (yet) if it is fresh or rotten.
Spices & Basics: The app must assume that basics like salt, pepper, or oil are available, as they are often stored in cupboards and not visible in the fridge photo.
Hallucinations: As with any AI, the LLM might invent a recipe that doesn't taste good in practice (e.g., "Cucumber in hot milk").
Roadmap

API Integration: Connection to the OpenAI API for real step-by-step cooking instructions.

Barcode Scanner: Supplement image recognition with barcode scanning for precise product identification.

Profile Settings: Filters for "I am Vegan" or "I hate Cilantro."

Shopping List: Automatically add missing items for the perfect recipe to a list.
## Future Vision & Perspectives
This project is designed as the foundation for a comprehensive Smart Nutrition Ecosystem. The journey goes far beyond a simple app:
🏠 Smart Kitchen Integration (IoT):
Instead of taking photos manually, we integrate directly into Smart Fridges with internal cameras.
Real-Time Inventory: The fridge knows exactly what is in stock at any time and proactively reports expiring products.
⚖️ Bio-Feedback Loop (Smart Scale):
Integration with an intelligent body scale.
The app knows not only the Input (fridge content) but also the Status (body weight/body fat).
If weight plateaus, the AI automatically adjusts recipe suggestions (e.g., reducing carbohydrates).
🎯 The Holistic Nutrition Assistant:
An AI Chat Interface (Voice & Text) where users define their goals in natural language: "I want to build muscle but I'm lactose intolerant" or "I want to lose 5kg by summer."
The AI plans not just single meals, but creates entire weekly schedules based on current stock and physical goals.
## Acknowledgments
This project stands on the shoulders of open-source giants:
Ultralytics YOLOv8: For the incredible Object Detection framework. GitHub
COCO Dataset: For the training data used in object detection.
Inspiration: Apps like ChefGPT and SuperCook.
