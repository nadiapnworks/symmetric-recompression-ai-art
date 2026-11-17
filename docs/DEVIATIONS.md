# Deviations from the first plan and pre-set reading rules

This file records every change made to the analysis plan after work began, the reason for it, and the rules for reading results that were fixed before those results were computed.

## Changes to the plan

1. **One-sided condition added.** The first plan had two conditions, original files and normalized files. After the metadata baseline showed that human files were stored at JPEG quality 95 and generated files at quality 75, a third condition was added before any detector was trained: human images are re-encoded at quality 75 and then normalized.

2. **Occlusion became the main attribution method.** The plan named Grad-CAM as the main method and occlusion as a comparison. After the deletion test, occlusion beat both controls in every group and Grad-CAM failed on Latent Diffusion images that passed as human in the cross-generator test. The roles were swapped.

3. **Shuffled control added to the design-measure analysis.** The plan compared each measure inside and outside the map region. A control was added in which the region of another image from the same source is applied to the image, to separate the content of a region from its position.

4. **Occlusion recomputed on the logit scale.** Occlusion on the probability scale was saturated: the median largest drop for one patch was between 0.000 and 0.002, and 5% to 10% of the maps were empty. Occlusion was recomputed on the logit margin in full precision. The effect sizes of the two versions correlated between 0.91 and 0.98. Maps for the normalized condition were not recomputed and remain on the probability scale.

5. **Color and blur ablation and design-measure baseline added.** Neither was in the first plan. They were added after the whole-image measures showed a large color difference between sources.

6. **Symmetric condition added.** After reading Kuturin et al. (2026), who report that correcting the encoding of one class relocates a compression shortcut, a fourth condition was added in which every image passes through the same two encodings. Reading rule: see below.

7. **Per-style analysis of images that passed as human was dropped.** It had been run on the one-sided models. Once the symmetric condition showed that those models used the trace of the added encoding, the per-style results were not used.

8. **Manual coding of shape and structure was dropped.** It was planned as a seventh design element. Its purpose was to name the detailed regions marked on human paintings, and that pattern was attributed to compression history.

9. **Replication with further seeds added.** The symmetric and one-sided conditions were retrained with a second seed to separate the effect of compression history from the effect of retraining (notebook 08). After that replication failed for one pattern, the two results that still rested on single trainings were also repeated (notebook 09): the compression matrix with second-seed models of all three conditions, and the cross-generator test with three further seeds. Reading rules: see below.

12. **Comparison detector added.** To address the use of a single architecture, the compression matrix, the cross-generator test, and the color ablation were repeated with a frozen CLIP ViT-L/14 encoder (OpenAI weights) and a logistic regression (notebook 11). Two failed runs are recorded here for transparency: the first used a model configuration with the wrong activation for the OpenAI weights (open_clip warned of a QuickGELU mismatch), and its features were discarded; the second ran out of memory while preparing the 50% saturation images. Only the third run, with the correct configuration, is used. Reading rules: see below.

11. **Audit of the whole dataset added.** The compression asymmetry had been measured on the sample of 3,000 images. The JPEG settings of all 185,015 files were then read (notebook 10) so that the first claim refers to the dataset. Reading rule: see below.

10. **Cross-generator models were trained twice.** They were retrained in notebook 04 so that their weights could be stored. Both runs gave the same numbers.

## Other notes

- The dataset stores the Ukiyo-e style under two folder names (`ukiyo-e` for generated images, `ukiyo_e` for human paintings). They were merged.
- Notebook 05 ran with OpenCV 5.0.0 and notebooks 06 and 07 with OpenCV 4.14.0 (see `ENVIRONMENT.md`).
- The symmetric condition equalizes the last two encodings of every image. It cannot equalize the first encoding, and Stable Diffusion images also lose their original block grid when they are downsized from 768 to 256 pixels.

## Reading rules fixed before the results

**Symmetric condition (notebook 07).** If the pattern of detailed and textured regions on human paintings persists under the symmetric condition, the finding is kept. If it weakens to the level of the normalized condition, the claim is changed to: the regions marked on human paintings depend on compression history.
Outcome: the pattern weakened (effect sizes of 0.21 under the one-sided condition fell to between 0.04 and 0.08), and the claim was changed.

**Second seed (notebook 08).** For the four detail measures on human paintings (gradient, edge density, texture contrast, homogeneity): the pattern is attributed to compression history if, with the second seed, the one-sided condition repeats the direction of the first seed with an absolute effect size of at least 0.15 on at least three of the four measures, and the symmetric condition has an absolute effect size of at most 0.10 on at least three of the four. If either condition fails, the third claim of the article is weakened.
Outcome: the symmetric condition met its criterion (four of four measures at or below 0.10). The one-sided condition did not (none of four measures reached 0.15; the effects fell from 0.21 to between 0.08 and 0.12). The pattern is not attributed to compression history, and the third claim of the article was changed: occlusion maps are faithful for each model but differ between repeated trainings as much as between compression conditions.

**Compression matrix (notebook 09).** The relocation of the shortcut counts as replicated if, for the one-sided models of the second seed, Stable Diffusion images in the symmetric version are recognized at least 10 points less often than in the one-sided version, and human paintings in the normalized version at least 10 points less often than in the one-sided version.
Outcome: replicated. The drops were 22.7 and 16.7 points with the first seed and 23.1 and 18.3 points with the second.

**Cross-generator test (notebook 09).** The gain under the symmetric condition counts as replicated if, in each direction, the mean over four seeds is higher than under the one-sided and normalized conditions, and the symmetric run exceeds the one-sided run in at least three of four seeds.
Outcome: replicated. Means were 21.4%, 12.1%, and 1.2% (train Latent Diffusion, test Stable Diffusion; symmetric ahead in three of four seeds) and 10.6%, 2.1%, and 2.0% (train Stable Diffusion, test Latent Diffusion; symmetric ahead in four of four).

**Audit of the whole dataset (notebook 10).** The claim that the asymmetry applies to the whole dataset is used if, in both the training and the test split, at least 99% of human files have an estimated JPEG quality of 90 or higher, at least 99% of generated files have a quality of 80 or lower, and every file is a JPEG.
Outcome: the claim applies. All 60,000 human files have quality 95 and all 125,015 generated files have quality 75 (estimated 74.8), with one quantization table per class and no exception in any style or split.

**Comparison detector (notebook 11).** (1) The relocation counts as present in CLIP if both drops in the one-sided row of the compression matrix are at least 10 points, and as absent if both are below 5. (2) The cross-generator pattern counts as the same as for ResNet50 if the symmetric condition has the highest mean in both directions. (3) Color dependence counts as present in CLIP if grayscale input lowers recognition of each generator by at least 20 points.
Outcome: (1) present, and stronger than in ResNet50 (drops of 35.7 and 27.3 points); (2) different: the symmetric condition was highest only for training on Stable Diffusion and testing on Latent Diffusion, while in the other direction the one-sided condition was highest (37.9% against 16.9%); (3) present (drops of 43.3 and 50.3 points). The claim that symmetric recompression improves transfer was therefore limited to ResNet50 in the article.
