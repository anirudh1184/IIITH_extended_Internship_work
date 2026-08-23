Image and video generation using ComfyUI templates.

Models Used:
Image: Flux (9B parameter model)
Video: Wan 2.2 (5B parameter model)

Used Flux (9B) to generate a landscape image of a city with a beautiful sky and tall buildings, then fed that exact image into Wan 2.2 (5B) using an Image-to-Video workflow to animate it.

All models were downloaded and the entire workflow was run completely offline on my machine.

Applied Optimizations:
To bypass laptop VRAM limits while running locally, I downscaled the input image to 512x512 and restricted the video length to 25–33 frames. I also swapped to a VAE Decode (Tiled) node with a tile size of 256 and temporal size of 16 to process the video in small, safe chunks without crashing.
