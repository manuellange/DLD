# DLD: A Deep Learning Based Line Descriptor for Line Feature Matching #

This repository contains the implementation of our DLD paper.

**Notice:** This repository does not contain the C++ Code which is a part of the project used for training and testing.
However, we added code that enables the testing functionality solely using python (special thanks goes to Robin Niebergall).
We found that due to small numerical differences, most but not all of the Cutouts are identical when created from this Python version instead of from the original C++ implementation.
Thus the results are not exactly the same, but very similar.

**Publication:**
```
@InProceedings{lange2019dld,
  author       = {Lange, Manuel and Schweinfurth, Fabian and Schilling, Andreas},
  title        = {{DLD: A Deep Learning Based Line Descriptor for Line Feature Matching}},
  booktitle    = {2019 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  year         = {2019},
  pages        = {5910--5915},
  organization = {IEEE},
}
```

## 1. Prerequisites
We used Python (3.6.9) and the following libraries:
```
opencv-contrib-python (4.0.1.24)
tensorflow (1.12.0)
tensorpack (0.8.9)
pandas (1.0.5)
matplotlib (3.1.1)
```
## 2. Preparation
The code can be used with precomputed lines (and ground truth information) in order to test the network on the same lines which we used for the benchmarks in our paper.
The line locations are in the *cpp.npz* files which we provide for the images (argument *--keylines_in "{PATH_TO_KEYLINES}"*.

The *cpp.npz* contains the lines for the right and for the left image. Use the argument *--keylines_use_second_image* to read the second/right lines from the file, and in that case also provide the path to the right image *--image_in "{PATH_TO_IMAGE}"*.

The program will calculate the descriptors and save them (use *--save_results*) next to the images.  The descriptors have to be calculated for the left and the right image separately.

The descriptors are in *im0.npz/im1.npz* files, which can be red and visualized by the [*LineLearningInterface*](https://github.com/manuellange/LineLearningInterface).

The corridor dataset including our labeled Ground Truth line information and also including already computed descriptors from the DLD can be found here [*LineDataset*](https://github.com/manuellange/LineDataset).

## 3. Usage
The results are saved next to the images in `{name}.npz` files containing the descriptors and lines.

Usage with precomputed keylines:
```powershell
python3 .\cld.py --image_in "{PATH_TO_IMAGE}" --keylines_in "{PATH_TO_KEYLINES}" --cutout_width 27 --cutout_height 100 --gpu 0 test "{PATH_TO_MODEL}" -n 1 --depth 10 --debug --min_len 15 --fixed_length --save_results
```

Usage with precomputed keylines (provide second/right image):
```powershell
python3 .\cld.py --image_in "{PATH_TO_IMAGE}" --keylines_in "{PATH_TO_KEYLINES}" --keylines_use_second_image --cutout_width 27 --cutout_height 100 --gpu 0 test "{PATH_TO_MODEL}" -n 1 --depth 10 --debug --min_len 15 --fixed_length --save_results
```

Usage without precomputed keylines (lines will be detected):
```powershell
python3 .\cld.py --image_in "{PATH_TO_IMAGE}" --cutout_width 27 --cutout_height 100 --gpu 0 test "{PATH_TO_MODEL}" -n 1 --depth 10 --debug --min_len 25 --fixed_length --save_results
```

There are additional command line arguments which can be used to influence the process (`image_in`, `cutout_{width, height, count}`, `save_results`).
> 👉 Note: `--save_results` will try to save the `.npz` next to the image file.
