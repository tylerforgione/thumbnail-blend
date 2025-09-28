# Thumbnail Blend
This script was inspired by someone on Twitter who did something similar (and likely better).
It takes some number of thumbnails from a YouTube channel (passed in by ID) and blends the images.
It does this by summing the RGB values and then normalizing. Then, multiplying by 255 to get RGB values back.
A note is that for a large number of very different thumbnails (> 5000), the image may become a static brown image.

## Resizing
Sometimes, thumbnails are not maxresdefault (1280x720) and instead are 380x120 or something else. Thus, they must be resized.
I resize them by using Pillow's resize function and padding with black (0,0,0).

## Aligning
Sometimes, thumbnails are so similar that just blending them doesn't give a result we expect.
Below is an example of Markiplier's last 100 thumbnails as 1 image (using alignment).
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/268b1428-6cb2-4263-b04d-3ef076b9cf10" />

Next, we have the same thumbnails used but with no alignment (all else is equal).
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/6ba7af44-a6f2-44e7-a76f-2bc70b657750" />

If you look at Markiplier's last 100 thumbnails, you'll notice that the first image seems like it makes more sense, but we are technically modifying the images.
Realistically, both work! It just depends what you like more. Seeing the common structures in thumbnails (using alignment) is cool,
but so is seeing what colours/items are most common.

My alignment function selects some image to be the focus/base and makes sure the other passed in image matches the features of the focus image.

### How it Works
It first starts by converting the images to grayscale. We don't need colours for this.
Then, it uses ORB to find key parts of the image. Next, it matches those descriptors from both images.
Finally, if there are enough matches, we can compute the coordinates of those key points and then perform an affine transformation.
This effectively makes it so that similar thumbnails which are slightly shifted end up as equal (or near equal) images.
