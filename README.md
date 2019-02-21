# tracmip-miami-zarr
This Github repo will serve as documentation for the process of converting the TRACMIP netCDF4 datasets stored on a server at the University of Miami to the zarr format, and then send these zarr datasets to the cloud bucket made available at Pangeo.
From the ground up, this includes:

* Installing some version of Python 2.7.x
* Installing Google Cloud SDK
* Establishing an environment to access and convert datasets
* Connecting to Pangeo's cloud bucket to upload data

First, we want to get a version of Python 2 up and running.
Miniconda 2 will do the trick, and allows us to more easily control the environment we are using:

```bash
wget https://repo.anaconda.com/miniconda/Miniconda2-latest-Linux-x86_64.sh
./Miniconda2-latest-Linux-x86_64.sh
```

After installing, we want to get the [latest versioned archive](https://cloud.google.com/sdk/docs/downloads-versioned-archives) of Google Cloud SDK; in this case, it is 234.0.0:

```bash
wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-234.0.0-linux-x86_64.tar.gz
tar -xzf google-cloud-sdk-234.0.0-linux-x86_64.tar.gz
./google-cloud-sdk/install.sh
```

Now that we have installed a version of Python 2.7.x and Google Cloud SDK, we are ready to set up an environment and access our data.
We can create a named environment using Miniconda like so:

```bash
conda create -n tracmip-miami-zarr python=2.7
conda activate tracmip-miami-zarr
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