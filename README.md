# tracmip-zarr
This Github repo will serve as documentation for the process of converting the TRACMIP netCDF4 datasets stored on a server at the University of Miami to the zarr format, and then send these zarr datasets to the cloud bucket made available at Pangeo.
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

After installing, we immediately want to set up an environment to install and use Google Cloud SDK in; it will only work with Python 2.7.x:

```bash
conda create -n gcloud python=2.7
conda activate gcloud
```

Now in our newly created environment, we want to get the [latest versioned archive](https://cloud.google.com/sdk/docs/downloads-versioned-archives) of Google Cloud SDK:

```bash
wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-235.0.0-linux-x86_64.tar.gz
tar -xvzf google-cloud-sdk-235.0.0-linux-x86_64.tar.gz
sh google-cloud-sdk/install.sh
```

Now that we have installed of Python and Google Cloud SDK, we are ready to set up an environment to access our data:

```bash
conda create -n tracmip-zarr python
conda activate tracmip-zarr
```

Once we're in the environment, we install a series of required packages (which will eventually be compiled in an `environment.yml` file for easy installation):

```bash
conda install xarray netCDF4 dask zarr jupyterlab
```

Once the packages are installed, we end our `ssh` session and sign in again, this time redirecting listening ports to our `localhost`:

```bash
ssh -L %port%:localhost:%port% tracmip@weather.rsmas.miami.edu
```

Where `%port%` can be filled in with any specified listening port.
All that is left is to run Jupyterlab and interact with the data:

```bash
jupyter lab --no-browser --port=%port%
```