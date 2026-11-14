# Software environment

All notebooks ran on Google Colab. Each notebook writes the versions it used to a text file in `MyDrive/Paper2/catatan/` (`lingkungan_langkah3.txt` to `lingkungan_langkah8.txt`, `lingkungan_analisis_tambahan.txt`, `lingkungan_pelengkap_naskah.txt`). Copy those files into `docs/environment_logs/`; they are the authoritative record.

Versions recorded for the main steps:

| Runtime | Used by | Versions |
|---|---|---|
| GPU (Tesla T4) | notebooks 06, 07, 08, 09 | Python 3.13.16, PyTorch 2.14.1+cu130, torchvision 0.29.1+cu130, NumPy 2.4.6, SciPy 1.16.3, scikit-learn 1.6.1, OpenCV 4.14.0, Pillow 11.3.0 |
| CPU | notebooks 06b, 07b | Python 3.13.18, PyTorch 2.14.1+cpu, NumPy 2.4.6, pandas 2.2.3, SciPy 1.16.3, Matplotlib 3.10.0 |
| CPU | notebook 05 | OpenCV 5.0.0 (the Colab CPU image at the time) |

Notebook 05 ran with OpenCV 5.0.0 and notebooks 06 and 07 with OpenCV 4.14.0. Notebook 06 recomputed the design measures of notebook 05 with new maps, and the effect sizes of the two runs correlated between 0.91 and 0.98.

## Fixed settings

- Random seed: 42 (sampling, folds, training seed 42 + fold index). Second-seed replication: 1042 + fold index. Cross-generator test: seed 52, repeated with 1052, 2052, and 3052.
- Detector: ResNet50 pretrained on ImageNet, all layers fine-tuned, Adam (learning rate 1e-4), cosine annealing to 1e-6, batch size 32, at most 30 epochs, early stopping with patience 5 on a validation set of 10% of the training folds, horizontal flip only, input 224 x 224.
- Occlusion: patch 32 x 32, stride 16, mean-color fill, logit margin of the predicted class, full precision.
- Deletion test: ten steps of 5% of the pixels; replacements: Gaussian blur (kernel 21, sigma 7) and mean color; controls: center prior and the mean of ten random maps.
- Map region: the 20% of pixels with the highest map value.
- Design measures: hue entropy (36 bins, chroma above 0.10), saturation, colorfulness (Hasler and Süsstrunk), brightness, tonal contrast, Sobel gradient, Canny edge density (100, 200), gray-level co-occurrence matrix with 32 levels (contrast, homogeneity, energy, correlation).
- Statistics: Wilcoxon signed-rank test on paired differences against a shuffled control, rank-biserial correlation as effect size, Benjamini-Hochberg correction at a false discovery rate of 5%, bootstrap intervals with 2,000 resamples.
