## Background

Here, in this script we will learn how to.

1. Read a Markdown file.
2. Convert it to HTML.
3. Embed all images in the HTML as base64 data URIs.

### Python Script to Convert Markdown to Embedded HTML

Here’s the  script:

```python
import base64
import re
from pathlib import Path
import markdown2

def embed_images_in_markdown(md_content, image_folder):
    # Find all image links in markdown (e.g., ![](images/example.png))
    image_links = re.findall(r'!\[.*?\]\((.*?)\)', md_content)
    print("Image links found in Markdown:", image_links)  # Debug

    for image_path in image_links:
        print(f"Processing image path: {image_path}")

        # Check for both direct and nested paths
        direct_image_file = Path(image_folder) / image_path
        nested_image_file = Path(image_folder) / Path(image_path).name

        print(f"Checking direct path: {direct_image_file}")
        print(f"Checking nested path: {nested_image_file}")

        if direct_image_file.exists():
            image_file = direct_image_file
            print(f"Using direct path for {image_path}")
        elif nested_image_file.exists():
            image_file = nested_image_file
            print(f"Using nested path for {image_path}")
        else:
            print(f"Warning: {image_path} not found in either {direct_image_file} or {nested_image_file}. Skipping.")
            continue

        try:
            with open(image_file, "rb") as img_f:
                img_base64 = base64.b64encode(img_f.read()).decode('utf-8')
                img_data_uri = f"data:image/{image_file.suffix[1:]};base64,{img_base64}"
                print(f"Base64 encoding for {image_file.name[:10]}: {img_base64[:50]}...")
                
                md_content = md_content.replace(f"![]({image_path})", f'<img src="{img_data_uri}">')
                print(f"Embedded image for {image_path}")
        except Exception as e:
            print(f"Error encoding {image_file}: {e}")

    return md_content

def convert_markdown_to_html(md_file, image_folder, output_html):
    print(f"Markdown file: {md_file}")
    print(f"Image folder: {image_folder}")
    print(f"Output HTML file: {output_html}")

    # Read the Markdown file
    with open(md_file, "r", encoding="utf-8") as f:
        md_content = f.read()

    # Embed images in the Markdown content
    md_content_with_images = embed_images_in_markdown(md_content, image_folder)

    # Convert Markdown to HTML
    html_content = markdown2.markdown(md_content_with_images)

    # Write the modified HTML to the output file
    try:
        with open(output_html, "w", encoding="utf-8") as f:
            f.write(html_content)
        print(f"Embedded HTML file saved as '{output_html}'")
    except Exception as e:
        print(f"Error saving output HTML file: {e}")

if __name__ == "__main__":
    import sys
    if len(sys.argv) != 4:
        print("Usage: python markdown_to_embedded_html.py <markdown_file> <image_folder> <output_html>")
        sys.exit(1)

    md_file = sys.argv[1]
    image_folder = sys.argv[2]
    output_html = sys.argv[3]

    convert_markdown_to_html(md_file, image_folder, output_html)
```

### Explanation of Changes

- **markdown2 Library**: We use `markdown2` to convert the Markdown content to HTML after embedding the images.
- **Image Embedding**: The `embed_images_in_markdown` function finds image references in the Markdown (using the `![]()` syntax), checks for the images in `image_folder`, and embeds them as base64 data URIs.
- **Markdown Conversion**: The modified Markdown content (with embedded images) is then converted to HTML and saved to `output_html`.

### How to Use the Script

1. **Install `markdown2` Library**:
   - Run the following command to install the `markdown2` library if it’s not already installed:
     ```bash
     pip install markdown2
     ```

2. **Save the Script**: Save the code above in a file named `markdown_to_embedded_html.py`.

3. **Prepare Markdown and Image Folder**: Place your Markdown file and images in a folder structure like this:
   ```
   ├── markdown_to_embedded_html.py
   ├── yourfile.md
   └── images/
       ├── image1.png
       ├── image2.jpg
       └── ...
   ```

4. **Run the Script**:
   ```bash
   python markdown_to_embedded_html.py yourfile.md images output.html
   ```
   - Replace `yourfile.md` with the path to your Markdown file.
   - Replace `images` with the folder containing all images referenced in the Markdown.
   - Replace `output.html` with the desired name for the output HTML file.

### Example

If your Markdown file is `example.md` and your images are in the `images` folder, the command would look like this:

```bash
python markdown_to_embedded_html.py example.md images embedded_example.html
```

### Output

The result is an HTML file named `embedded_example.html` (or whatever name you specify), which contains all images embedded as base64 data URIs. This file is now a standalone, self-contained HTML document that can be easily shared or viewed offline without needing external image files.

---

This script allows you to convert any Markdown document with external images into a single HTML file with embedded images, making it portable and accessible offline. Let me know if you have any questions!