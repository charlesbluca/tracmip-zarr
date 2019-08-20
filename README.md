# tracmip-zarr
This Github repo serves as documentation for the process of converting the TRACMIP netCDF4 datasets stored on a server at the Karlsruhe Institute of Technology (KIT) to the `zarr` format, and then sending these datasets to the cloud bucket made available at Pangeo.
The process of this conversion was done across three different machines - one located at the University of Miami, one located at KIT, and one located at the Lamont-Doherty Earth Observatory.
Though the individual tasks of each machine were different, the process of initializing an environment on them remained consistent.
From the ground up, this includes:

* Installing Python via Miniconda
* Installing Google Cloud SDK
* Establishing an environment to access and convert datasets
* Connecting to Pangeo's cloud bucket to upload data

First, we want to get multiple versions of Python up and running.
Miniconda 3 will do the trick, and allows us to more easily control the environment we are using:

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh
```

Now we want to get the [latest versioned archive](https://cloud.google.com/sdk/docs/downloads-versioned-archives) of Google Cloud SDK:

```bash
wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-235.0.0-linux-x86_64.tar.gz
tar -xvzf google-cloud-sdk-235.0.0-linux-x86_64.tar.gz
sh google-cloud-sdk/install.sh
```

To make sure the `gcloud` command line interface will work in or out of our environement, we append the following to our `.bashrc`:

```bash
echo "export CLOUDSDK_PYTHON=/usr/bin/python2.7" >> .bashrc
```

This tells our version of Google Cloud SDK to work with the system native version of Python 2.7.
Now that we have installed Python and Google Cloud SDK, we are ready to set up an environment to access our data:

```bash
conda create -n tracmip-zarr python
conda activate tracmip-zarr
```

Once we're in the environment, we want to install the most recent development versions of`xarray` and `zarr`, available on Github:

```bash
pip install git+https://github.com/pydata/xarray.git
pip install git+https://github.com/zarr-developers/zarr.git
```

With these packages taken care of, we install the remainder of our requirements using `conda`:

```bash
conda install netCDF4 dask jupyterlab
```

Optionally, we install `dask-labextension`, which allows for a more intuitive view of our `dask` distributed cluster:

```bash
conda install -c conda-forge nodejs
pip install dask_labextension
jupyter labextension install dask-labextension
```

Once the packages are installed, we are ready to enter the JupyterLab environment and begin interacting with data:

```bash
jupyter lab --port=%port%
```

In order to access the URL generated by Jupyter, we must open another instance of the terminal and redirect this port to our local machine using the `ssh -L` command:

```bash
ssh -L %port%:localhost:%port% tracmip@weather.rsmas.miami.edu
```

Where `%port%` can be filled in with any specified listening port.
