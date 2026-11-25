# T1w-to-T1w Registration

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Registers a moving T1-weighted image to a fixed T1-weighted image with ANTs antsRegistration (translation, rigid, affine and optional SyN stages) and outputs the transforms and the registered image.**

Runs a multi-stage ANTs registration whose last stage is set by transformation (translation, rigid, affine or nonlinear SyN) and whose multiresolution depth is set by settings, then applies the result to the moving T1w. Use it for intra-subject longitudinal alignment or inter-subject/template normalisation of T1w images; the transforms can be reused by other Apps (for example tract transformation).

| | |
|---|---|
| 📥 **Inputs** | `neuro/anat/t1w` ×2 |
| 📤 **Outputs** | `neuro/transform/nifti` · `neuro/anat/t1w` |
| 🐳 **Container** | `brainlife/ants:2.2.0-1bc` |
| ⚙️ **Resources** | 1 CPUs · 2 h walltime |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.861](https://doi.org/10.25663/brainlife.app.861) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `t1_moving` | `neuro/anat/t1w` | yes | T1-weighted image to be registered (moving). |
| `t1_fixed` | `neuro/anat/t1w` | yes | T1-weighted reference image (fixed/target space). |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `transformations` | `neuro/transform/nifti` | ANTs affine (affine.txt) and, for nonlinear registration, the warp and inverse warp fields. |
| `transformed` | `neuro/anat/t1w` | Moving T1w resampled into the fixed image space. |

<details>
<summary><b>⚙️ Parameters</b> (2)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `settings` | enum | `4` | The `settings` parameter controls the depth of the multiresolution (pyramidal) registration strategy used by ANTs. Higher settings correspond to more levels in the registration pyramid, starting from coarse alignment and progressively refining toward finer spatial correspondence. Options: `1`, `2`, `3`, `4`. |
| `transformation` | enum | `nonlinear` | Last registration stage to run: translation, rigid, affine or nonlinear (SyN); earlier stages are always included. Options: `translation`, `rigid`, `affine`, `nonlinear`. |

</details>

<!-- brainlife-app:end -->

An automated and reproducible pipeline to perform image registration between two T1-weighted anatomical MRI scans using ANTs. This tool ensures high-precision alignment through a multi-stage transformation process suitable for intra-subject longitudinal studies or inter-subject normalization workflows.

---

## Author

**Gabriele Amorosino**  
(email: [gabriele.amorosino@utexas.edu](mailto:gabriele.amorosino@utexas.edu))

---

## Description

This workflow registers a moving T1-weighted (T1w) image to a fixed T1w reference image using ANTs' registration framework. The pipeline performs translation, rigid, affine, and optionally nonlinear (SyN) registration steps. Outputs include the transformation matrices, warp fields, and the registered T1w image.

The pipeline is configurable through a `config.json` file and is compatible with containerized execution (e.g., Singularity), enhancing portability and reproducibility across computing environments.

---

## Requirements

- [Singularity](https://sylabs.io/guides/latest/user-guide/)

---

## Usage

### Running on Brainlife.io

#### Via Web UI

1. Go to [Brainlife.io](https://brainlife.io) and search for the `app-registration-t1w2t1w` app.
2. Click the **Execute** tab.
3. Upload the following inputs:
   - Moving T1w image (`.nii.gz`)
   - Fixed T1w reference image (`.nii.gz`)
   - (Optional) `config.json` file to specify registration parameters
4. Submit the job to generate aligned T1w outputs and transformation metadata.

#### Via CLI

1. Install the Brainlife CLI: https://brainlife.io/docs/cli/
2. Log in:
   ```bash
   bl login
   ```
3. Run the app:
   ```bash
   bl app run --id <app_id> --project <project_id> \
     --input moving_t1:<moving_t1_id> \
     --input fixed_t1:<fixed_t1_id>
   ```

Replace dataset and project IDs as appropriate.

---

### Running Locally

#### Using a Configuration File

1. Clone the repository:
   ```bash
   git clone https://github.com/gamorosino/app-registration-t1w2t1w.git
   cd app-registration-t1w2t1w/main
   ```

2. Prepare a `config.json` file:
   ```json
   {
       "moving_t1": "sub-01_acq-01_T1w.nii.gz",
       "fixed_t1": "sub-01_acq-02_T1w.nii.gz",
       "transformation": "nonlinear",
       "settings": "1"
   }
   ```

3. Run the pipeline:
   ```bash
   bash ./main
   ```

---

## Outputs

- `ANTs_outputs/` — Registration outputs from ANTs (affine, warp, inverse warp)
- `transformations/` — Exported transformation matrices and warp fields
- `transformed/` — Final registered T1w image

---

## Citation

If you use this repository, please cite the relevant tools and frameworks:

- Avants, B. B., Tustison, N. J., Song, G., Cook, P. A., Klein, A., & Gee, J. C. (2011). A reproducible evaluation of ANTs similarity metric performance in brain image registration. Neuroimage, 54(3), 2033-2044.
- Hayashi, S., Caron, B. A., Heinsfeld, A. S., ... & Pestilli, F. (2024). brainlife. io: a decentralized and open-source cloud platform to support neuroscience research. Nature methods, 21(5), 809-813.

---

## License

This project is released under the [MIT License](LICENSE).
