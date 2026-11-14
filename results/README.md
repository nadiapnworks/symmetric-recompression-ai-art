# Result files

Copy here, from `MyDrive/Paper2/hasil/` on Google Drive:

- every `.csv` file in `hasil/`
- the folder `hasil/detektor_runs/` (training records, `.json`)
- `hasil/gambar_naskah/fig3_daftar_gambar.csv` (IDs of the images shown in Figure 3)

Do not copy `models/`, `peta/`, `peta_logit/`, `peta_simetris/`, `peta_seed2/` (model weights and raw maps, large) or any image file.

## Where each table of the article comes from

| Article | Source file | Produced by |
|---|---|---|
| Table 1 (file properties) and metadata baseline | metadata tables of notebook 02 | `02_metadata_normalization` |
| Table 2 (detector performance; unseen generator over four seeds) | `rq1_detektor_ringkasan.csv`, `detektor_runs/simetris_fold*.json`, `rq1_lintas_generator_per_seed.csv` | `03`, `07`, `09` |
| Table 3 (compression matrix, two seeds) | `rq1_matriks_kompresi.csv`, `rq1_matriks_kompresi_seed2.csv` | `07`, `09` |
| Table 4 (color and blur ablation) | `rq1_uji_warna_detail_simetris_per_fold.csv` | `07_symmetric_condition` |
| Deletion test and its intervals (Section 3.5) | `rq2_simetris_deletion.csv`, `rq2_simetris_deletion_selang_kepercayaan.csv`, `rq2_deletion_selang_kepercayaan.csv` | `07`, `07b`, `06b` |
| Table 5 (design measures in map regions, two conditions by two seeds) | `rq2_seed_elemen.csv` | `08_second_seed` |
| Map agreement between trainings and conditions | `rq2_seed_kesepakatan_peta.csv`, `rq2_kesepakatan_antar_kondisi.csv` | `08`, `07b` |
| Figure 2 and design-measure baseline | `rq2_simetris_baseline_ukuran_desain.csv`, `rq1_lintas_generator_per_seed.csv` | `07`, `09` |

## Glossary (Indonesian to English)

| In the files | Meaning |
|---|---|
| `normal` | normalized condition |
| `riwayat_disamakan`, `satu sisi` | one-sided condition |
| `simetris` | symmetric condition |
| `manusia` | human |
| `LD`, `SD` | Latent Diffusion, Stable Diffusion |
| `benar`, `salah`, `lolos` | correct, wrong, generated image accepted as human |
| `lintas` | cross-generator test |
| `akurasi` | accuracy |
| `peta`, `peta_kosong` | map, empty map |
| `tengah`, `acak` | center-prior control, random control |
| `ukuran`, `kelompok`, `bagian` | measure, group, part (fold or model) |
| `bermakna` | significant after false discovery rate correction |
| `kecerahan`, `kontras`, `gradasi` | brightness, tonal contrast, gradient |
| `kepadatan tepi`, `tekstur`, `kekayaan` | edge density, texture, colorfulness |
| `selang kepercayaan`, `keunggulan` | confidence interval, advantage over the stronger control |
