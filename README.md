<img width="752" alt="Screenshot 2024-06-18 at 2 55 49 AM" src="Screenshot%202024-06-18%20at%202.55.49%E2%80%AFAM.png">import os
import cv2
import numpy as np
from PIL import Image, ImageDraw, ImageFont
import random


# Function to load and preprocess the dog image
def load_dog_image(image_path):
    img = cv2.imread(image_path)
    img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)  # Convert from BGR to RGB
    return img

# Function to apply glasses to the dog image
def apply_glasses(image, glasses_path):
    glasses_img = Image.open(glasses_path).convert("RGBA")
    glasses_img = glasses_img.resize((120, 50))  # Resize glasses to fit the dog
    image.paste(glasses_img, (80, 50), glasses_img)

# Function to apply clothes to the dog image
def apply_clothes(image, clothes_path):
    clothes_img = Image.open(clothes_path).convert("RGBA")
    clothes_img = clothes_img.resize((200, 200))  # Resize clothes to fit the dog
    image.paste(clothes_img, (50, 200), clothes_img)

# Function to apply hats to the dog image
def apply_hat(image, hat_path):
    hat_img = Image.open(hat_path).convert("RGBA")
    hat_img = hat_img.resize((200, 150))  # Resize hat to fit the dog
    image.paste(hat_img, (50, 0), hat_img)

# Function to change background of the dog image
def change_background(image, background_color):
    background = Image.new('RGBA', image.size, background_color)
    image.paste(background, (0, 0), background)

# Function to add animation effect (for demonstration)
def add_animation_effect(image):
    # Example: Simulate animation effect by altering colors
    image = image.convert("RGBA")
    data = np.array(image)
    red, green, blue, alpha = data[:, :, 0], data[:, :, 1], data[:, :, 2], data[:, :, 3]
    red = (red + 100) % 255
    green = (green + 50) % 255
    blue = (blue + 150) % 255
    new_image = Image.fromarray(np.dstack((red, green, blue, alpha)))
    return new_image

# Main function to generate variations
def generate_variations(base_image_path, output_folder, num_variations=500):
    os.makedirs(output_folder, exist_ok=True)
    base_image = load_dog_image(base_image_path)
    
    for i in range(1, num_variations + 1):
        # Make a copy of the base image
        dog_image = Image.fromarray(base_image.copy())
        
        # Apply wearables randomly
        if random.random() < 0.3:
            glasses_path = 'glasses.png'  # Replace with actual path to glasses image
            apply_glasses(dog_image, glasses_path)
        if random.random() < 0.3:
            clothes_path = 'clothes.png'  # Replace with actual path to clothes image
            apply_clothes(dog_image, clothes_path)
        if random.random() < 0.3:
            hat_path = 'hat.png'  # Replace with actual path to hat image
            apply_hat(dog_image, hat_path)
        
        # Change background randomly
        if random.random() < 0.2:
            background_color = (random.randint(0, 255), random.randint(0, 255), random.randint(0, 255), 255)
            change_background(dog_image, background_color)
        
        # Add animation effect randomly
        if random.random() < 0.1:
            dog_image = add_animation_effect(dog_image)
        
        # Save the variation
        output_path = os.path.join(output_folder, f'dog_variation_{i}.png')
        dog_image.save(output_path)
        print(f'Saved variation {i}/{num_variations}: {output_path}')


