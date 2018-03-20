# Predict Car Sale Prices with ML Studio
This demo will create a very simple Azure ML experiment which predicts car sale prices based on features.

Time: Approx 10 minutes

### Pre Reqs
* Have `Car prices.csv` avaliable. A modified version of one of the Azure sample data sets. [get it here](https://raw.githubusercontent.com/martinkearn/Content/master/Demos/Machine%20Learning%20and%20Cognitive/ML%20Supporting%20Files/Car%20prices.csv)
* Have a printed version of ![ML Car Sales Finished Experiment.png](https://github.com/martinkearn/Content/raw/master/Demos/Machine%20Learning%20and%20Cognitive/ML%20Supporting%20Files/ML%20Car%20Sales%20Finished%20Experiment.PNG)

## Create an experiment and load data
Sign into https://studio.azureml.net

Datasets > New > From Local File >  `Car prices.csv` (or use the one that is already there)

Experiments > New > Blank experiment

## Add data set

Drag `Car prices.csv` to the canvas (Saved Datasets > My Datasets)

Visualize the dataset (Right-click the output port > Visualise)

## Clean data - remove rows
Drag the Transforms > `Clean missing data` module

Connect to the `Select Columns in Dataset` module

Cleaning mode = "Remove entire row"

Run the experiment and observe green ticks

## Split data
Drag the Data Transformation > Sample & Split > `Split Data` module

Connect to output of `Clean Missing Data`

`Fraction of rows in the first output dataset` to 0.75

Run the experiment (this allows the cleaning modules to pass definitions down the workflow)

## Add Linear Regression
Drag the Machine Learning > Initialize Model > Regression > `Linear Regression` module

PLace next to the `Split data` module

## Train the model on Price
* Drag the Machine Learning > Train > `Train Model` module
* Connect the left input to `Linear regression`
* Connect the right input to the left output of `Split Data`
* `Launch column selector`
* Add `price` as a selected column

Run the experiment. This will use 75% of our data to train the model

## Score the model
We now need to score out model by comparing the model we've train against the remaining 25% of data to see how accurate the price prediction is

Drag the Machine Learning > Score > `Score Model` module

Connect the left input to `Train Model`

Connect the right input to right output of `Split data`

Run the experiment

Visualise the output of `Score Model`
* Compare `price` to `scored label` (the predicted price given other data)


## Convert to Predictive Experiment
So far the experiment has just been a 'training experiment'. We now need to convert it to a model that can be used to score new data.

Run the experiment

Setup Web Service > Predictive Web Service

Run the new predictive experiment (takes approx 30 seconds)

Deploy Web Service

## Test the Web Service
Web Services > Car Price Prediction [Predictive Exp]

Point out API key

Click `Request/response` to see API help page with endpoint inputs/outputs and sample code

Click `Test (preview)` and apply the folloiwng data
* make = `audi`
* fuel = `diesel`
* doors =  `four`
* body = `hatchback`
* drive = `fwd`
* weight = `1900`
* engine-size = `150`
* bhp = `150`
* mpg = `55`
* price = `23000`
* Observe scored label is £20,914
    * We know the model is right because it is an Audi and therefore it is overpriced
