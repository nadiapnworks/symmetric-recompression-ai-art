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

9. **Second-seed replication added.** The symmetric and one-sided conditions were retrained with a second seed to separate the effect of compression history from the effect of retraining. Reading rule: see below.

10. **Cross-generator models were trained twice.** They were retrained in notebook 04 so that their weights could be stored. Both runs gave the same numbers.

## Other notes

- The dataset stores the Ukiyo-e style under two folder names (`ukiyo-e` for generated images, `ukiyo_e` for human paintings). They were merged.
- Notebook 05 ran with OpenCV 5.0.0 and notebooks 06 and 07 with OpenCV 4.14.0 (see `ENVIRONMENT.md`).
- The symmetric condition equalizes the last two encodings of every image. It cannot equalize the first encoding, and Stable Diffusion images also lose their original block grid when they are downsized from 768 to 256 pixels.

## Reading rules fixed before the results

**Symmetric condition (notebook 07).** If the pattern of detailed and textured regions on human paintings persists under the symmetric condition, the finding is kept. If it weakens to the level of the normalized condition, the claim is changed to: the regions marked on human paintings depend on compression history.
Outcome: the pattern weakened (effect sizes of 0.21 under the one-sided condition fell to between 0.04 and 0.08), and the claim was changed.

**Second seed (notebook 08).** For the four detail measures on human paintings (gradient, edge density, texture contrast, homogeneity): the pattern is attributed to compression history if, with the second seed, the one-sided condition repeats the direction of the first seed with an absolute effect size of at least 0.15 on at least three of the four measures, and the symmetric condition has an absolute effect size of at most 0.10 on at least three of the four. If either condition fails, the third claim of the article is weakened.
Outcome: to be added after notebook 08 has run.
