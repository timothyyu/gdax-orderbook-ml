# gdax-orderbook-ml

### Project/File Structure
- 1_test_dataset_scrape.ipynb
    - Notebook file for inital test scrape of GDAX data for a single product (i.e. "BTC-USD") into MongoDB
    - Test dataset: 10 minutes of seconds of [Level 2](https://docs.gdax.com/#the-code-classprettyprintlevel2code-channel) and [Match](https://docs.gdax.com/#the-code-classprettyprintmatchescode-channel) Data streamed from Websocket into MongoDB
- 2_test_dataset_load.ipynb &  test_data folder
    - Notebook file for inital test dataset loading + parsing into csv format
- 3_test_dataset_model_prototype.ipynb
    - Notebook file for model prototype design and construction
    - Test (static) dataset will be used first 
    - Streaming (live) dataset from MongoDB + Websocket connection hypothetically possible
    - Mock models/testing getting LSTM/CNN/GRU models to compile and fit with basic input data (main focus is input shape for LSTM/GRU)
- 4_test_input_feature_expansion.ipynb
    - Expansion of features/attributes for model design + further refinement of dataset shaping/program structure components
    - 15 minute update cycle --> past 15 minutes of candlestick data pulled from GDAX API
        - Used to generate chart + support/resistance levels + proximity to said support/resistance levels for each price level as a feature
    - Implementation of base logic for applying l2 update states orderbook snapshot
- 5_test_input_feature_refinement.ipynb:
    - Further reshaping of input/test data for LSTM/GRU model before update functions/definitions for continuous updates is implemented
    - Data structures for scrape/request log and support/resistance (predicted vs actual)


- MongoDB local instance, Mongo DB Compass & PyMongo
    - Jupyter notebook & Python for inital data request and scrape 
    - GDAX API -> MongoDB -> Pandas DataFrame -> .csv (for test dataset)
    - Websocket -> MongoDB -> DataFrame object (for live dataset functionality)

- [Daniel Paquin's gdax-python](https://github.com/danpaquin/gdax-python) API python client loaded as git submodule (gdax-python is MIT licensed)
    - Development branch checked out; Master branch missing commits and merges essential for stable API connectivity and Mongo intergration
    - If imported as git submodule: cd into directory folder in project folder and git pull, or `git submodule update --init`  from root project directory from git bash 
        - Once respository as submodule pulled/updated, cd into gdax-python and install the package: `python setup.py install`
        - Then deactivate/reactivate environment

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

#### GDAX L2 snapshot and L2 update response structure
L2 snapshot is a snapshot of the entire orderbook for a specified product at a given point in time. L2 update responses are subsquent updates to the snapshot (of the orderbook state).

#####**L2 snapshot structure**

- [side,price,size]
- 'side' added as part of structure for classification
    - Bid = buy side
    - Ask = sell side
- [size delta, position, size_delta, sr_prox_value,sr_prox_line]
    - size delta is change in size since last l2 update
    - position is change in position since last l2 update
    - sr_prox_value/sr_prox_line is price point promixity to major support/resistance lines for the past 15 minutes
        - [size delta, position, size_delta, sr_prox_value,sr_prox_line]
            - updated with every l2 update applied to l2 snapshot (before inputs loaded as features into model)
        - Auto support/resistance line generation adapted from nakulnayyar SupResGenerator:
            - https://github.com/nakulnayyar/SupResGenerator
        - Chart data generated from gdax API request:
        `chart_15m =public_client.get_product_historic_rates('BTC-USD', granularity=60)`

#####**L2 update structure**

- [side, price, size, time]
    - size of "0" indicates the price level can be removed
    - 'size_delta' feature calculated from difference
