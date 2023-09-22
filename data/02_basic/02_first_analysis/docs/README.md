# 01_determine_center_of_image

## overview

This experiment analyzes the center of image.

## input_data

* [05_arrange_data](https://dg.nii.ac.jp/thiraki-nii/tutorial-basic/src/master/experiments/01_main_experiment/output_data/05_arrange_data)
    * The results of the experiment [01_main_experiment](https://dg.nii.ac.jp/thiraki-nii/tutorial-basic/src/master/experiments/01_main_experiment).
    * Please see the [README.md](https://dg.nii.ac.jp/thiraki-nii/tutorial-basic/src/master/experiments/01_main_experiment/README.md) of the experiment.

## source

* [06_determine_center_of_image.ipynb](./source/06_determine_center_of_image.ipynb)
    * This experiment includes codes that determines the center of image for the experimental results in [05_arrange_data](https://dg.nii.ac.jp/thiraki-nii/tutorial-basic/src/master/experiments/01_main_experiment/output_data/05_arrange_data).

## output_data

* result.pickle
    * The output of [06_determine_center_of_image.ipynb](./source/06_determine_center_of_image.ipynb).
    * This includes the center of image.
    * This file has the type of `scipy.optimize._optimize.OptimizeResult`.
