# Bone Segmentation and Landmark Detection in 3D Knee CT Scans

This project processes 3D CT scans of the knee to segment bone regions (femur and tibia), perform contour modifications, and detect anatomical landmarks. It includes image processing techniques only, and produces multiple segmentation masks and landmark outputs.


---

## 📌 Tasks Overview

### ✅ Task 1.1 – Bone Segmentation

- **Goal**: Segment femur and tibia bones from a 3D CT scan.
- **Method**: 
  - Threshold-based segmentation on voxel intensity.
  - Connected component labeling to extract femur and tibia.
  - Hole-filling on coronal slices to refine masks.
- **Reference**: Adapted from [Lukas Snoek's Python for MRI](https://lukas-snoek.com/NI-edu/fMRI-introduction/week_1/python_for_mri.html).
- **Output**:
  - `femur_intensity.nii.gz`
  - `tibia_intensity.nii.gz`

---

### ✅ Task 1.2 – Contour Expansion

- **Goal**: Expand the segmentation masks outward by a fixed distance (default: 2 mm).
- **Steps**:
  - Convert millimeter distance to voxel radius using voxel spacing.
  - Use 3D spherical structuring element to apply binary dilation.
- **Parameters**:
  - `expansion_mm = 2.0` (default)
- **Output**:
  - `expanded_mask_2mm.nii.gz`
  - `expanded_mask_4mm.nii.gz`

---

### ✅ Task 1.3 – Randomized Contour Adjustment

- **Goal**: Simulate annotation variability by generating masks between the original and the 2 mm expanded mask.
- **Constraints**:
  - Mask must not shrink below the original.
  - Mask must not exceed the 2 mm limit.
- **Steps**:
  - Randomly choose expansion value `r` such that `0 <= r <= 2`.
  - Use the same expansion method with the random value.
- **Output**:
  - `randomized_mask_1.nii.gz`
  - `randomized_mask_2.nii.gz`

---

### ✅ Task 1.4 – Tibial Landmark Detection

- **Goal**: Detect two anatomical landmarks on the tibia:
  1. **Medial lowest point**
  2. **Lateral lowest point**
- **Method**:
  - Find the lowest (minimum z) slice of the tibia mask.
  - Identify the most medial (max x) and most lateral (min x) voxels in that slice.
  - Convert voxel coordinates to world coordinates using the affine matrix.


---

## 🔄 Steps Followed

1. **Data Preparation**
   - Download the CT scan (`3702_left_knee.nii.gz`)
   - Load using `nibabel` and visualize central slice

2. **Bone Segmentation (Task 1.1)**
   - Apply global threshold (350)
   - Label and extract femur/tibia
   - Fill holes slice-wise

3. **Contour Expansion (Task 1.2)**
   - Compute expansion in voxels from millimeters
   - Apply binary dilation using spherical kernel
   - Save 2 mm and 4 mm expansions

4. **Randomized Mask Generation (Task 1.3)**
   - Randomly select mm between 0 and 2
   - Generate new masks
   - Ensure bounds are respected

5. **Landmark Detection (Task 1.4)**
   - Load tibia masks
   - Detect landmarks on lowest Z slice
   - Save medial/lateral points in world coordinates

---

## References
- https://lukas-snoek.com/NI-edu/fMRI-introduction/week_1/python_for_mri.html
- https://www.udemy.com/course/deep-learning-with-pytorch-for-medical-image-analysis

  
## 🛠️ Dependencies

Install all dependencies with:

```bash
pip install numpy scipy nibabel matplotlib scikit-image
# Load the required packages that are given in the file. 

