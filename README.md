# DoVs-Project
# 🎨 Interactive Image Transformation App

## Project Overview
This project is an interactive image transformation tool developed entirely in **Google Colab** using **ipywidgets**, **OpenCV**, and **scikit-learn**. It enables users to apply eight artistic filters to any image, transforming them into stylized artworks.

Please Click on the link to interact with the app. [Open in Google Colab](https://colab.research.google.com/drive/1E4MsvuOEOJmaKKoxYC5D_HYIRnglTGbb?authuser=2#scrollTo=dWwLQSQcsAAE)

### Supported Filters
- Impressionism  
- Watercolour Painting  
- Charcoal Sketch  
- Comic Book  
- Oil Canvas  
- Word Art  
- Pointillism  
- Andy Warhol Style  

Users can either upload their own images or select from built-in examples. The platform is optimized for real-time use, with an intuitive UI. It requires no installation beyond fetching default assets via `gdown`, making it highly portable and user-friendly.

The interface dynamically adapts to the selected filter, showing relevant inputs only when needed (e.g., the text box appears only for Word Art). It also includes real-time feedback labels to inform users when processing is underway, helping prevent accidental interruptions. Once a filter is applied, the original and stylized images are displayed side-by-side for comparison. A dedicated download button lets users save their results instantly.

---

## How to Run
1. Open the Colab notebook [Open in Google Colab](https://colab.research.google.com/drive/1E4MsvuOEOJmaKKoxYC5D_HYIRnglTGbb?authuser=2#scrollTo=dWwLQSQcsAAE).
2. Run all cells (**Runtime > Run all**).
3. Choose an image (upload or default).
4. Select a filter from the dropdown.
5. For **Word Art**, type a word/phrase in the input field.
6. Click **“Run Filter”** to apply the effect.
7. Download the result using the provided button.

**Note**: The **Word Art** filter takes 2–3 minutes. Avoid interacting with the UI during processing.

---

##  Filter Reports

### Impressionism
<img src="images/Berthe_Morisot.jpg" alt="Impressionist" width="400" height="300"/>
<p> Figure: “In the Grass” by Berthe Morisot, a prominent French Impressionist painter [1]</p>


**Implementation:**  
The Impressionism filter captures the loose, textured feel of impressionist paintings by layering randomized brush strokes over a simplified color base. The image is resized to 300×300 for performance efficiency, and KMeans clustering reduces the color palette to 16 dominant tones, imitating the restricted color choices typical in impressionist artwork. A soft Gaussian blur of the original image is used as the background to preserve structure. Then, simulated strokes are drawn using the quantized colors, with each line given a randomized angle and position jitter to evoke the expressive, human quality of hand-painted art. These strokes vary in direction and overlap, building a layered, painterly texture that preserves the image’s overall form while abstracting fine detail.

**Development Iterations:**  
Initial attempts used a white canvas, which introduced patchy gaps. Switching to a blurred base image added visual continuity. Parameters like stroke spacing, angle, and blending ratio were adjusted to achieve a balance between abstraction and recognizability.

**Evaluation:**  
It excels with portraits and landscapes but underperforms in crowded or distant group photos where finer details get lost.

**Future Improvements:**  
Incorporating adaptive stroke thickness, variable stroke length based on texture, and gradient-based orientation could enhance realism. These additions would mimic traditional brushwork more faithfully.

<img src="images/Marilyn_Monroe_Impressionism.jpg" alt="Watercolour" width="1200" height="600"/>
<img src="images/Nature_Impressionism.png" alt="Watercolour" width="1200" height="600"/>

---

### Watercolour Painting
<img src="images/Watercolour.jpg" alt="Watercolour" width="300" height="300"/>
<p>Figure: Bird Water Coulour Painting by Christopher P Jones [2]</p>
**Implementation:**  
The Watercolour filter simulates the soft blending and delicate transitions characteristic of watercolor paintings. It begins with a bilateral filter that smooths regions while maintaining edge integrity, creating a base that mimics pigment diffusion on paper. Next, edges are extracted from a grayscale version of the smoothed image using Canny edge detection, slightly thickened via dilation, and then inverted to blend softly with the base layer. This edge blend preserves structure without introducing harsh outlines. To protect natural highlights, a luminance mask is generated from the blended result, ensuring that very bright areas are not dulled during tone adjustments. The final steps involve warming the color tone slightly and applying desaturation in the HSV color space, which collectively give the image its pastel, paper-washed appearance.

**Development Iterations:**  
Initially, the lack of edges made images appear overly blurry. Adding Canny edge detection brought structure back. Brightness masking preserved highlights and avoided flattening. Fine-tuning of bilateral strength, thresholds, and color tone yielded a soft but expressive look.

**Evaluation:**  
Strong lighting and clear edges give the best results. Low-contrast or underexposed images lose definition after desaturation.

**Future Improvements:**  
Adaptive thresholding for highlight masking, watercolor-bleed textures, and region-specific saturation control could yield better results.


<img src="images/Marilyn_Monroe_Watercolour.jpg" alt="Watercolour" width="1200" height="600"/>
<img src="images/Tiger_Watercolour.png" alt="Watercolour" width="1200" height="600"/>

---

### Charcoal Sketch
<img src="images/charcoal.jpg" alt="Charcoal" width="300" height="300"/>
<p> Figure: Charcoal Sketch by Jeff Hainez</p>
**Implementation:**  
The Charcoal Sketch filter recreates the aesthetic of hand-drawn charcoal artwork using a grayscale-focused approach. The image is first converted to grayscale, then inverted to emphasize the contrast between light and shadow. A Gaussian blur is applied to the inverted image to mimic the smudging effect seen in real charcoal sketches. This blurred image is then blended with the original grayscale using equal weighting, producing smooth transitions while preserving important contours. The result is converted back to a 3-channel BGR format to ensure compatibility with color-based processing pipelines. This minimalist approach effectively balances softness and structure, creating a believable charcoal rendering without explicit edge detection.

**Development Iterations:**  
Earlier versions used dodge blending and Laplacian subtraction, but this caused white halos—especially around subjects with black backgrounds (e.g., Marilyn Monroe). Replacing that with grayscale inversion and Gaussian blending resolved the artifact issue.

**Evaluation:**  
Works well on close-up portraits with strong contrast. Cluttered or dark backgrounds can reduce clarity.

**Future Improvements:**  
Adaptive blur based on detail, paper texture overlays, and content-aware enhancement (portrait vs. scenery) could improve balance and realism.


<img src="images/Marilyn_Monroe_CharcoalSketch.jpg" alt="Watercolour" width="1200" height="600"/>
<img src="images/Group_Charcoal.png" alt="Watercolour" width="1200" height="600"/>

---

### Comic Book
<img src="images/Roy_Lichtenstein_comic_pop.jpg" alt="Comic" width="300" height="300"/>
<p>Figure: “Drowning Girl” by Roy Lichtenstein, a prominent American Pop Art painter [4]</p>

**Implementation:**  
The Comic Book filter emulates the bold, graphic style of classic pop art comics. It starts by simplifying the image’s color palette using KMeans clustering, reducing it to six dominant tones to mimic the flat inking style used in comic illustrations. To retain some of the original texture, the clustered image is blended with the original, giving a stylized yet detailed look.
Strong edges are then extracted from a grayscale version of the input using the Canny edge detector. These edges are inverted and used as a mask to overlay bold black outlines on the image, imitating the heavy ink lines typical of comic art.
Finally, a halftone layer is generated by scanning the image in a grid and placing black dots with variable radii based on local brightness—darker areas get larger dots, lighter ones smaller. This halftone texture is subtly blended over the image.


**Development Iterations:**  
Initial Mean Shift clustering was accurate but slow. KMeans offered better performance. Sobel edges were too thick, so Canny was adopted. Morphological closing was tested but led to overconnected edges. Halftone blending and radius tweaking refined the final look.

**Evaluation:**  
Ideal for well-lit, cleanly composed portraits. Less effective for low-contrast or evenly lit images, where outlines become faint and impact is reduced.

**Future Improvements:**  
Mean Shift clustering could improve segmentation in complex scenes, and advanced edge detectors may offer better consistency across varied lighting.


<img src="images/Marilyn_Monroe_Pop.jpg" alt="Watercolour" width="1200" height="600"/>
<img src="images/Solo_Comic.png" alt="Watercolour" width="1200" height="600"/>

---

### Oil Canvas
<img src="images/oil_painting.jpg" alt="Oil" width="300" height="300"/>
<p>Figure: “Woman Holding a Fruit Plate” by Raja Ravi Varma, a renowned Indian painter [5]</p>
**Implementation:**  
The Oil on Canvas filter mimics the layered texture and rich tones of oil painting. It begins by normalizing the image and applying a bilateral filter per channel to smooth color gradients while preserving edges—similar to brush blending. KMeans clustering is used to extract 16 dominant colors, giving the image a stylized, painterly flatness. The original image is converted to grayscale and passed through a Sobel filter to detect edges, which are then inverted and scaled down to create a mask. This mask is used to suppress high-gradient regions in the final composition. Finally, the quantized and smoothed outputs are blended (70% smoothed, 30% quantized) and multiplied by the edge mask, producing a soft, layered effect that emulates brush buildup over textured surfaces.

**Development Iterations:**  
Initial versions used Canny edges and binary masks, which led to sharp, unnatural overlays. Switching to Sobel and using inverted gradients for suppression softened transitions and improved texture blending.

**Evaluation:**  
Performs best on portraits or compositions with strong lighting and color contrast. Struggles with busy scenes or low-contrast images due to excessive flattening.

**Future Improvements:**  
Edge-aware smoothing and region-adaptive clustering could retain more detail in complex zones. Adding brush textures would also enhance authenticity.


<img src="images/Marilyn_Monroe_Oil_Paint.jpg" alt="Watercolour" width="1200" height="600"/>
<img src="images/Group_oil.png" alt="Watercolour" width="1200" height="600"/>

---

### Word Art
<img src="images/Wordart.png" alt="Word" width="300" height="300"/>
 <p> Figure: Word Art Self Portrait by Mariel Labrecque [6]</p>

**Implementation:**  
The Word Art filter transforms an image into typographic artwork by replacing pixels with words whose sizes are dynamically mapped to local brightness. The input image is first validated and converted to grayscale to extract luminance. It is then resized for better performance and readability. A large white canvas is created, and the user’s input phrase is split into words. These words are sequentially drawn across the canvas, mapped to every alternate pixel. Font sizes are determined using an interpolation function, where darker pixels yield larger fonts. The result is a text-based rendering that visually represents tonal gradients using size and spacing, forming an image entirely from words.

**Development Iterations:**  
Early versions used single characters and lacked coherence. Switching to full phrases improved narrative and aesthetic impact. Font size scaling and spacing were refined to maintain balance. Faint backgrounds were avoided as they muddled contrast.

**Evaluation:**  
Performs well on portraits or icons with strong shapes. Struggles on busy or low-contrast scenes. Long phrases lead to layout clutter and repetition artifacts.

**Future Improvements:**  
Support for boldness simulation and smarter text wrapping would improve flexibility. Limiting phrase length or auto-summarizing content could help maintain clarity in dense regions.


<img src="images/Marilyn_Monroe_WordArt.jpg" alt="Watercolour" width="1200" height="600"/>
<img src="images/gorup_word.png" alt="Watercolour" width="1200" height="600"/>

---

### Pointillism
<img src="images/Georges_Seurat_Sunflowers_Pointilism.jpg" alt="Pointillism" width="300" height="300"/>
<p>Figure: “Sunflowers” in the style of Georges Seurat, a pioneering French Post-Impressionist painter [7]</p>
**Implementation:**  
The Pointillism filter emulates Georges Seurat’s technique of reconstructing an image through distinct colored dots. The image is first resized to 300×300 for efficiency and smoother results. KMeans clustering is applied to reduce the color space to 16 dominant tones, capturing the visual essence with a simplified palette.
A white canvas is initialized, and then a regular grid is used to iterate over the clustered image. At each step, a small circular dot (radius 2) is stamped onto the canvas using the corresponding color from the clustered result. This builds a mosaic of tightly spaced dots that mimic brush dabs in traditional pointillism. Finally, the dotted image is resized back to the original dimensions for display.

**Development Iterations:**  
Early versions had tiny, densely packed dots, making images noisy and hard to interpret. Increasing dot spacing and radius improved legibility. A faint background image was also tried, but it muddied the colors. White background was reinstated for contrast.

**Evaluation:**  
Great for colorful or high-contrast images. Loses detail in portraits or low-contrast scenes where dot patterns can't capture nuance.

**Future Improvements:**  
Adaptive dot sizing—small in detailed areas, large in flat regions—could enhance image fidelity while preserving the stylized effect.


<img src="images/Marilyn_Monroe_Pointillism.jpg" alt="Watercolour" width="1200" height="600"/>
<img src="images/nature_pointillism.png" alt="Watercolour" width="1200" height="600"/>

---

### Andy Warhol Style
<img src="images/Andy_Warhol.jpeg" alt="Warhol" width="300" height="300"/>
<p>Figure: “Marilyn Diptych” by Andy Warhol, a leading figure of the American Pop Art movement [8]</p>

**Implementation:**  
The Andy Warhol filter recreates the bold, high-contrast repetition and color blocking of Warhol’s pop art. KMeans clustering reduces the image to four major colors, which are then mapped to predefined pop color schemes. Four different recolored versions are generated using unique Warhol-style palettes. Each pixel is matched to its color cluster and recolored according to the selected scheme. Finally, the four recolored images are arranged in a 2x2 collage, forming a striking visual composition. This repetition of subject with varying color schemes reflects the graphic, screen-printed look Warhol was known for.

**Development Iterations:**  
Mean Shift was tried for better segmentation but was too slow. KMeans offered a good trade-off. Sobel edges were also explored but made the design too noisy, so the filter focused on color blocking only.

**Evaluation:**  
Performs best on single-subject portraits or objects with plain backgrounds. Struggles on landscapes or group images, where clusters get messy and impact is lost.

**Future Improvements:**  
Preprocessing to isolate subjects could improve performance on complex backgrounds. More color map variety or dynamic palette selection could expand creative range.


<img src="images/Marilyn_Monroe_AndyWarholFilter.jpg" alt="Watercolour" width="1200" height="600"/>
<img src="images/Fruiy_Warhol.png" alt="Watercolour" width="1200" height="600"/>

---
## Personal Contribution & Reflection

This project was completed independently. I was responsible for the entire design, development, and documentation process. I explored various artistic styles, researched corresponding image processing techniques, and implemented the filters from scratch in Google Colab using OpenCV, NumPy, and scikit-learn.

### What I Learned

- **Translating Artistic Styles into Algorithms:** I deepened my understanding of how classical and modern art styles can be translated into computational rules. By studying Impressionism, Pop Art, and comic illustration techniques, I learned to abstract their visual elements (e.g., brush strokes, halftones, limited color palettes) and recreate them using OpenCV and NumPy.

- **Adaptive Image Processing Design:** I learned the value of building adaptive systems — where filters dynamically respond to image content (e.g., preserving highlights in Watercolour, or clustering colors in Warhol-style). This required balancing technical precision with visual aesthetics.

- **UI/UX in Notebooks:** I explored how to design an interactive and responsive interface using `ipywidgets` within a Colab environment. I learned how to manage widget state, prevent user interruption during processing, and deliver a smooth user experience — even without a web frontend.

- **Optimizing for Performance in Visual Pipelines:** Performance bottlenecks taught me how to simplify data (e.g., resizing before KMeans), avoid redundant conversions, and tune clustering/edge-detection parameters. I also explored the trade-off between processing time and visual quality.

- **Creative Debugging & Visual Testing:** I developed a visual debugging habit — assessing filters not just by code output, but by their *aesthetic effect*. This was especially important for creative filters where traditional accuracy isn't the goal.

- **Handling Failures & Iteration Loops:** Several early attempts (e.g., Word Art clutter, Charcoal white halo artifacts) failed to meet expectations. I learned to iterate visually, test diverse cases, and refine both logic and layout over time.

- **Design Thinking in a Technical Context:** Beyond coding, I learned to think like a designer — considering user input flow, emotional tone of the visuals, and clarity of communication (e.g., showing original vs. filtered side-by-side, real-time feedback prompts).

---
## Art References

1] Berthe Morisot – “In the Grass” - https://museums.eu/collection/object/236427/young-girl-on-the-grass-mlle-isabelle-lambert-berthe-morisot-18411895?pUnitId=5373
2] Christopher P Jones - Bird Watercolour Painting-  https://christopherpjones.medium.com/bird-watercolor-painting-tutorial-81df32c1338
3]Jeff Haines- Charcoal Sketch - https://in.pinterest.com/pin/792352128200074573/
4] Roy Lichtenstein – “Drowning Girl” - https://www.moma.org/audio/playlist/3/176
5] Raja Ravi Varma – “Woman Holding a Fruit Plate” - https://artsandculture.google.com/asset/madri-or-the-maharashtrian-lady-with-fruit-raja-ravi-varma/sQHU63D0VK3xpA?hl=en
6] Mariel Labrecque - Self Portrait Word Art - https://laconteconsulting.com/2018/10/19/what-i-learned-from-word-art/
7] Georges Seurat – “Sunflowers” - https://www.facebook.com/photo.php?fbid=851107430386079&id=100064604895753&set=a.542901797873312
8] Andy Warhol – “Marilyn Diptych” - https://www.mutualart.com/Artwork/Marilyn-Monroe/87E25A09F4753D4DCE943BD73945BAB2


Used under public domain or for academic reference only.


