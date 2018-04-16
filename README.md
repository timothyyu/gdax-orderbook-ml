# gdax-orderbook-ml



### Project/File Structure

- test_dataset_scrape.ipynb
    - Notebook file for inital test scrape of GDAX data for a single product (i.e. "BTC-USD") into Mongo DB

- test_dataset_load.ipynb &  test_data folder
    - Notebook file for inital test dataset loading + parsing into csv format

### Backend Structure
- MongoDB local instance, Mongo DB Compass & PyMongo
    - Jupyter notebook & Python for inital data request and scrape 
    - GDAX API -> MongoDB -> Pandas DataFrame -> .csv
- gdax-python repository loaded as git submodule
    - Development branch checked out; Master branch missing commits and merges essential for stable API connectivity and Mongo intergration

