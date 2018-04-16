# gdax-orderbook-ml

### Project/File Structure
- test_dataset_scrape.ipynb
    - Notebook file for inital test scrape of GDAX data for a single product (i.e. "BTC-USD") into MongoDB
    - Test dataset: 60-90 seconds of [Level 2](https://docs.gdax.com/#the-code-classprettyprintlevel2code-channel) and [Match](https://docs.gdax.com/#the-code-classprettyprintmatchescode-channel) Data streamed from Websocket into MongoDB

- test_dataset_load.ipynb &  test_data folder
    - Notebook file for inital test dataset loading + parsing into csv format

- test_dataset_model_prototype.ipynb
    - Notebook file for model prototype design and construction
    - Test (static) dataset will be used first 
    - Streaming (live) dataset from MongoDB + Websocket connection hypothetically possible
    
### Backend Structure
- MongoDB local instance, Mongo DB Compass & PyMongo
    - Jupyter notebook & Python for inital data request and scrape 
    - GDAX API -> MongoDB -> Pandas DataFrame -> .csv (for test dataset)
    - Websocket -> MongoDB -> dataframe object (for live dataset functionality)

- [Daniel Paquin's gdax-python](https://github.com/danpaquin/gdax-python) API python client loaded as git submodule (gdax-python is MIT licensed)
    - Development branch checked out; Master branch missing commits and merges essential for stable API connectivity and Mongo intergration

**Tensorflow/Keras local GPU backend (CUDA)**

*Local GPU used to greatly accelerate prototyping, construction, and building of ML model(s) for this project, especially considering the nature of the dataset & machine learning model complexity.*
- [Requirements to run tensorflow with GPU suppport](https://www.tensorflow.org/install/install_windows#requirements_to_run_tensorflow_with_gpu_support)
- Nvidia GPU with CUDA Compute 3.0
    - Nvidia CUDA 9.0
    - Nvidia cuDNN 7.0 (v7.1.2)
        - Install  cuDNN .dlls in CUDA directory
        - Edit environment variables:
            - C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0
    - pip uninstall tensorflow && pip install --ignore-installed --upgrade tensorflow-gpu 
        - Default tensorflow install is CPU-only; install CUDA and cuDNN requirements, then uninstall tensorflow and reinstall tensorflow-gpu (pip install --ignore-installed --upgrade tensorflow-gpu)
