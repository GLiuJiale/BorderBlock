# BorderBlock - See Your World in a New Light!

**BorderBlock** is a meticulously crafted resource pack that reinvents the visual style of Minecraft by adding clean, stylish borders to a vast array of blocks. Whether you're a builder, explorer, or redstone engineer, BorderBlock brings a new layer of clarity and artistic flair to your world, making every structure pop and every detail stand out.

![Medieval Castle](https://cdn.modrinth.com/data/cached_images/a306e10f16c8bd53bf4307e10b05ce522f88ff8d.png)
* **A majestic medieval castle showcasing how BorderBlock adds depth and definition to stone, wood, and roof block textures.**

---

## ✨ Features

### 🎨 Universal Visual Enhancement
BorderBlock is designed to be comprehensive. We've added borders to a massive selection of blocks, including but not limited to:
*   **All Stone Variants:** Cobblestone, Stone Bricks, Andesite, Diorite, Granite, and more.
*   **Wood & Planks:** Every type of wood and its plank variant gets a crisp border.
*   **Terracotta & Glazed Terracotta:** The colorful patterns of terracotta are now neatly contained.
*   **Concrete & Wool:** Perfect for pixel art and modern builds.
*   **Prismarine & Deepslate:** Enhance your aquatic and deep dark constructions.
*   **Nether Blocks:** Nether Bricks, Blackstone, and Basalt look more imposing than ever.
*   **End Stones:** Give the End a more structured, alien feel.
*   ...and hundreds more!

### 🛠️ Practical Benefits
It's not just about looks! The added borders provide tangible gameplay benefits:
*   **Builder's Best Friend:** Easily distinguish between similar-looking blocks. Perfect for aligning complex patterns and ensuring your builds are pixel-perfect.
*   **Cave & Mine Navigator:** Never get lost in a dark, monotonous cave again. The borders help define your surroundings, improving spatial awareness.
*   **Accessibility:** For players who have difficulty with visual clutter or distinguishing colors, the clear outlines can make the game world much easier to parse.

### 🔧 Technical Excellence
*   **Seamless Integration:** Borders are designed to tile perfectly, ensuring there are no visual glitches at chunk borders or in large, flat surfaces.
*   **Performance Friendly:** As a pure resource pack, it requires no mods and has a negligible impact on your game's performance.
*   **Wide Version Support:** Faithfully recreated and tested across a huge range of versions, from the classic **1.14** to the latest **1.21.10**.

---

## 🖼️ Gallery

![Village](https://cdn.modrinth.com/data/cached_images/272954bfcd05845a2562cd2b39d8aa9c8a2c52cd.png)
* **A testament to the transformative power of clean lines. BorderBlock adds structure and depth to the entire landscape, unifying diverse materials under a single, elegant visual language.**

![Basalt deltas](https://cdn.modrinth.com/data/cached_images/7dcaf25cdb6b14b5532db2a0dd611ad5f07d806f.png)
* **The harsh landscape of the Basalt Deltas feels even more structured and defined with borders on basalt and blackstone.**

---

---

## 📥 Installation
Download the BorderBlock.zip file from this page.

Open Minecraft and go to Options > Resource Packs.

Click on Open Pack Folder and place the downloaded .zip file inside. (You do not need to extract it!)

Back in the game, select BorderBlock from the available packs list and move it to the "Selected" column.

Enjoy your newly defined world!

---

## 💡 Did You Know?

All the borders in this resource pack are artistically derived from the darkest pixels found within each block's original texture. This ensures a cohesive and authentic look that remains true to Minecraft's vanilla aesthetic.

---

## 🔧 Technical Implementation

For those interested in the technical side, here's the Python script that made this resource pack possible (Since the Python script mechanically extracts the darkest color from each texture for coloring, dual-toned blocks could appear visually unusual. To address this, we've implemented additional manual refinement to optimize border coloring for these specific blocks):

```python
import os
import glob
from PIL import Image
import numpy as np

def get_darkest_pixel_in_block(block):
    """
    Find the darkest pixel in a 16x16 block (considering transparency)
    Returns the pixel's hexadecimal color code
    """
    # Convert block to numpy array
    block_array = np.array(block)
    
    # Only consider opaque or semi-transparent pixels (alpha > 0)
    opaque_pixels = block_array[block_array[:, :, 3] > 0]
    
    if len(opaque_pixels) == 0:
        return None  # Return None if no opaque pixels
    
    # Calculate brightness for each pixel (using weighted formula)
    brightness = opaque_pixels[:, 0] * 0.299 + opaque_pixels[:, 1] * 0.587 + opaque_pixels[:, 2] * 0.114
    
    # Find the darkest pixel
    darkest_idx = np.argmin(brightness)
    darkest_pixel = opaque_pixels[darkest_idx]
    
    # Convert to hexadecimal color code
    hex_color = '#{:02x}{:02x}{:02x}'.format(darkest_pixel[0], darkest_pixel[1], darkest_pixel[2])
    
    return hex_color

def apply_coloring_pattern(image, hex_color):
    """
    Apply coloring to the image according to a specified pattern
    """
    # Define coloring pattern (1 means colorize, 0 means keep original)
    pattern = [
        [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
        [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]
    ]
    
    # Convert hex color to RGB
    hex_color = hex_color.lstrip('#')
    color_rgb = tuple(int(hex_color[i:i+2], 16) for i in (0, 2, 4))
    
    # Create new image
    width, height = image.size
    new_image = image.copy()
    
    # Apply coloring pattern
    for y in range(16):
        for x in range(16):
            if pattern[y][x] == 1:
                # Calculate position in original image
                orig_x = x * (width // 16)
                orig_y = y * (height // 16)
                
                # Get current 16x16 block size (handle edge cases)
                block_width = min(16, width - orig_x)
                block_height = min(16, height - orig_y)
                
                # Apply color to entire 16x16 block (preserve transparency)
                for by in range(block_height):
                    for bx in range(block_width):
                        px = orig_x + bx
                        py = orig_y + by
                        
                        # Get original pixel's alpha value
                        original_pixel = image.getpixel((px, py))
                        if len(original_pixel) == 4:  # RGBA
                            alpha = original_pixel[3]
                        else:  # RGB
                            alpha = 255
                        
                        # Set new color, preserving original alpha
                        new_pixel = color_rgb + (alpha,)
                        new_image.putpixel((px, py), new_pixel)
    
    return new_image

def process_minecraft_textures(input_dir, output_dir):
    """
    Process all PNG files in the Minecraft textures directory
    """
    # Ensure output directory exists
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    
    # Get all PNG files
    png_files = glob.glob(os.path.join(input_dir, "*.png"))
    
    print(f"Found {len(png_files)} PNG files")
    
    for i, file_path in enumerate(png_files):
        try:
            print(f"Processing file {i+1}/{len(png_files)}: {os.path.basename(file_path)}")
            
            # Open image
            with Image.open(file_path) as img:
                # Convert to RGBA mode to ensure transparency handling
                img = img.convert("RGBA")
                width, height = img.size
                
                # Check if image dimensions are multiples of 16
                if width % 16 != 0 or height % 16 != 0:
                    print(f"  Warning: Image dimensions {width}x{height} not multiples of 16, skipping")
                    continue
                
                # Calculate number of 16x16 blocks
                blocks_x = width // 16
                blocks_y = height // 16
                
                # Process each 16x16 block
                for block_y in range(blocks_y):
                    for block_x in range(blocks_x):
                        # Extract 16x16 block
                        left = block_x * 16
                        upper = block_y * 16
                        right = left + 16
                        lower = upper + 16
                        
                        block = img.crop((left, upper, right, lower))
                        
                        # Find darkest pixel
                        darkest_color = get_darkest_pixel_in_block(block)
                        
                        if darkest_color:
                            # Apply coloring to this block
                            block_img = img.crop((left, upper, right, lower))
                            colored_block = apply_coloring_pattern(block_img, darkest_color)
                            
                            # Place colored block back into original image
                            img.paste(colored_block, (left, upper))
                
                # Save processed image
                output_path = os.path.join(output_dir, os.path.basename(file_path))
                img.save(output_path, "PNG")
                print(f"  Saved: {output_path}")
                
        except Exception as e:
            print(f"  Error processing file {file_path}: {str(e)}")

# Usage example
if __name__ == "__main__":
    #"*" is the image address
    input_directory = r"*"
    output_directory = r"*"
    
    process_minecraft_textures(input_directory, output_directory)
    print("Processing complete!")
```

---

**Thank you for downloading BorderBlock! We hope it transforms your Minecraft experience.**
