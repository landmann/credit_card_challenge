
# How to Run

Usability of the model can be seen in `notebooks/run.ipynb`.

# Production Plan

Although I don't have much experience deploying machine learning models, I have a rough understanding of industry practices based on varying literature.

Although implementation details may vary, a general pipeline is as follows:

We first gather as much data from our customers as possible and upload to a cloud-based database such as AWS Redshift.  We then explore this data and build a model like the one on the Jupyter notebooks.  Once the model is validated and ready for deployment, we can refactor the code, optimizing for speed and readability.  With an optimized model, we can now wrap the interface logic into a flask applicaition. The flask application is containerized into a docker container and deploy on an AWS ec2 instance hosted on kubernetes for new data to be classified on the fly.  We can later build a GUI and let our sales/customer facing team play with the interface to understand the risks associated with certain clients.  A tradeoff we face with cloud deployment, as opposed to on-premise hosting, is the risk of hacking and other cyber attacks, so we must ensure the safety of our systems before deploying to the cloud.

When more data is gathered, we can retrain the model with the newly available data for more up-to-date predictability, as consumer patterns can shift. A few things to keep in mind during production:

* Git discipline must be maintained during development to ensure a faulty system isn't deployed. 

* Documentation has to aid reproducibility in order to prevent key-man dependencies.

If we foresee that data loads are large enough to warrant paralellization or multithreading, we should refactor the code with this in mind.  If multiple users are going to load the system heavily, we can set up a server that spawns intances on request, with the models pre-loaded and orchestrated by kubernetes.


# Improvements

### Weighted Penalization
The current system penalizes the model per miscategorized sample. Realistically speaking, it is more expensive for a company to miscategorize a sample with high limit balance than with low limit balance. We should, therefore, tweak the model to minimize total miscategorized balance as opposed to total number of accounts miscategorized.

### Model Ensembles
In order to get higher accuracy, one can build several models and take the average of the predictions to make a classification - a technique called "ensemble."

### Alternative Data
This is a relatively common problem - it wouldn't be too difficult to get extraneous data sources in order to get generalized information regarding spending patterns of demographic groups.
