# 01_main_exepriment

## overview
This experiment executes numerical simulations calculating the Bragg spots from a nano-scale single crystals. This experiments uses [BraggPy](https://github.com/Surpris/BraggPy) to simulate the spots.

## source

* [01_simulation.ipynb](./source/01_simulation.ipynb)
    * Calculates the Bragg reflection from the (111) plane of single FCC spherical crystals.
* [02_normalization.ipynb](./source/02_normalization.ipynb)
    * Normalizes the Bragg spot intensity from each crystal with respect to the Bragg spot intensity from the largest crystal (equivalent to the stable state) calculated in `01_simulation.ipynb`.
* [03_calculate_exp_data.ipynb](./source/03_calculate_exp_data.ipynb)
    * Pseudo Poisson noise and detector noise are added to the normalized Bragg spots to get results closer to the experiment.
* [04_simulate_delay_time.ipynb](./source/04_simulate_delay_time.ipynb)
    * Calculates pseudo delay time after NIR irradiation.
* [05_arrange_data.ipynb](./source/05_arrange_data.ipynb)
    * For each crystal, the results of the above four calculations are combined into a single HDF5 file.

## input_data
This experiment has the following input data:

* [param.json](./input_data/param.json)
    * This file includes parameters for simulation of Bragg reflection.

## output_data

* 01_simulation/fourier_modulus_Rx.yA.h5
    * Results of simulations conducted in [01_simulation.ipynb](./source/01_simulation.ipynb).
    * `x.y` represents the radius of the target crystal.
* 02_normalization/factor.h5
    * This file is the output of [02_normalization.ipynb](./source/02_normalization.ipynb).
    * This file includes the normalization factor.
    * This file is supposed to be loaded by [h5py](https://www.h5py.org/) or other compatible libraries.
* 03_calculate_exp_data/images_R{x.y}A_{n}shots.h5
    * These files are the output of [03_calculate_exp_data.ipynb](./source/03_calculate_exp_data.ipynb).
    * Each file includes `n` distributed single Bragg spots from the crystal with the radius of `x.y` angstroms.
    * These files are supposed to be loaded by [h5py](https://www.h5py.org/) or other compatible libraries.
* 03_calculate_exp_data_img/images_R{x.y}A_{n}shots.png
    * These files are the output of [03_calculate_exp_data.ipynb](./source/03_calculate_exp_data.ipynb).
    * Each file shows the summed pattern of the images in `images_R{x.y}A_{n}shots.h5`.
* 04_simulate_delay_time/delay_time_R{x.y}A.h5
    * These files are the output of [04_simulate_delay_time.ipynb](./source/04_simulate_delay_time.ipynb).
    * Each file includes the time delay corresponding to `exp_results_R{x.y}A.h5.`
* 05_arrange_data/exp_results_R{x.y}A.h5
    * These files are the output of [05_arrange_data.ipynb](./source/05_arrange_data.ipynb).
    * Each file includes distributed images in `images_R{x.y}A_{n}shots.h5` and some parameters for analysis.
    * These files are supposed to be loaded by [h5py](https://www.h5py.org/) or other compatible libraries.

### Structure of `02_normalization/factor.h5`

```
./output_data/02_normalization/factor.h5
├── input_parameters
│   ├── find_fwhm_factor: factor to find the FWHM with (1/angstrom, float)
│   ├── fine_step: step of fine coordinates fow interpolation (pixel, int)
│   ├── normalized_intensity: the intensity to normalize the sum of Bragg spot intensity to (photons, float)
│   ├── pos_expected: the spot position expected from the Miller index and the simulated image in 'srcpath' (1/angstrom, tuple of float)
│   ├── srcpath: the path of source including the image for calculation of the normalization factor (none, str)
│   ├── threshold: the intensity threshold to extract the Bragg spot (arbitrary, float)
│   └── width: the half width of the spot region (1/angstrom, float)
└── outputs
    ├── main_outputs
    │   ├── fwhm_x: the FWHM in the x direction (1/angstrom, float)
    │   ├── fwhm_y: the FWHM in the x direction (1/angstrom, float)
    │   └── normalization_factor: the normalization factor (photons, float)
    └── positions
        ├── pos_spot_peak_x: the x-direction position of the pixel with the peak intensity (pixel, int)
        ├── pos_spot_peak_y: the y-direction position of the pixel with the peak intensity (pixel, int)
        ├── pos_spot_range_x: the x-direction range of the spot region (pixel, tuple of int)
        └── pos_spot_range_y: the y-direction range of the spot region (pixel, tuple of int)
```

### Structure of `03_calculate_exp_data/images_R{x.y}A_{n}shots.h5`

```
./output_data/03_calculate_exp_data/images_R{x.y}A_{n}shots.h5
├── input_parameters
│   ├── detector_noise
│   │   ├── mean: the mean of detector noise following a Gaussian distribution (photons, float)
│   │   └── std: the standard deviation of detector noise following a Gaussian distribution (photons, float)
│   ├── image
│   │   ├── number_of_images: the number of images (none, int)
│   │   └── radius_std: the standard deviation of the radial spot position following a Gaussian distribution (1/angstrom, float)
│   ├── random_seed: seed of random number used by 'numpy.random.seed()' (none, int)
│   └── source
│       ├── factor_result_path: the path of source including the normalization factor generated by `02_normalization.ipynb` (none, str)
│       └── srcpath: the path of source including the images generated by the `test_to_use_braggpy_module` experiment (none, str)
└── outputs
    └── main_outputs
        └── images: the generated images (numpy.ndarray(photons, numpy.float64))
```

### Structure of `04_simulate_delay_time/delay_time_R{x.y}A.h5`

```
./output_data/04_simulate_delay_time/delay_time_R{x.y}A.h5
└── timing_monitor
    └── delay_time: time delay (femtosecond, float)
```

### Structure of `05_arrange_data/exp_results_R{x.y}A.h5`

```
./output_data/05_arrange_data/exp_results_R{x.y}A.h5
├── data
│   └── detector_images: the images generated by `03_calculate_exp_data.ipynb` (photons, numpy.ndarray(numpy.float64))
├── instruments_info
│   │── timing_monitor
│   │   └── delay_time: time delay (femtosecond, float)
│   ├── xray_imaging_detector
│   │   ├── detector_name: detector name (none, str)
│   │   ├── detector_type: detectr type (none, str)
│   │   └── momentum_space
│   │       ├── momentum_x: the x-direction momentum coordinates (1/angstrom, numpy.ndarray(numpy.float64))
│   │       └── momentum_y: the y-direction momentum coordinates (1/angstrom, numpy.ndarray(numpy.float64))
│   └── xray_source
│       ├── source_name: X-ray source name (none, str)
│       ├── source_type: X-ray source type (none, str)
│       └── wavelength: the wavelength of the incident X-ray photons (angstrom, float)
├── run_info
│   └── number_of_shots: the number of XFEL shots (none, int)
└── sample_info
    ├── sample_name: sample name (none, str)
    └── sample_type: sample type (none, str)
```
