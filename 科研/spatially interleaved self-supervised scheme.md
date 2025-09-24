# spatially interleaved self-supervised scheme

A spatially interleaved self-supervised scheme is a method used in machine learning, particularly for image restoration tasks like denoising and super-resolution, that extracts training data from a single noisy image. It avoids the need for external clean data by leveraging the inherent redundancy and statistical properties of the image itself. 

The core idea is to train a neural network using pairs of images derived from the same noisy input, where one image serves as the "target" and the other as the "input." This is achieved by separating the pixels of the original noisy image into multiple sets based on their spatial arrangement. 

How it works

1. **Pixel-realignment strategy:** A common approach is to extract spatially independent pixel sets from the noisy input image. For example, a single image can be decomposed into four subsampled images by extracting pixels based on a checkerboard-like pattern.
2. **Input and target generation:** These subsampled images are then used to create the input-target pairs for network training. A key principle is that the network is trained to predict the missing pixels or the denoised values for one set of pixels, using the other, spatially interleaved pixels as context. For example, the network might use one of the subsampled images as input and another as the target.
3. **Self-supervision:** The training relies on the assumption that adjacent pixels in a clean image are statistically correlated, but a noise distribution is not. By training the network to predict a missing pixel from its neighbors, it learns to suppress noise while preserving the underlying image structure.
4. **Application to image reconstruction:** In microscopy, this scheme can be combined with other algorithms. For example, in super-resolution microscopy, it can be used to denoise the raw images, which are then fed into a conventional reconstruction algorithm. 

Key applications and benefits

- **Self-supervised denoising:** It provides an effective way to train a denoising network using only noisy images, circumventing the need for clean, noise-free ground truth data.
- **Super-resolution microscopy:** By denoising raw data before reconstruction, it can improve the quality of super-resolution images.
- **Reduced data requirements:** Because the scheme generates its own training pairs from the input, it can be effective even with limited data.
- **Robustness:** Since it does not rely on prior assumptions about the imaging process or the sample, it is versatile and can be extended to various imaging modalities.