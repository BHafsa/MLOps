# MLFlow 
![mlflow](./figures/mlflow.png)


Machine Learning Flow is python package that allow to efficiently manage and document the performed experiments with an easy syntax and code lines that can be integrated inside of the code easily. Even though MLflow was designed for machine learning projects, it can be used within any type of python scripts to organise and facilitate the experiments that are performed. 

## Installation

MLFLow can be easily installed using pip or conda as follows:

1. It you are using pip: ``` pip install mlflow```

2. Or if you are using conda

``` 
conda install -c conda-forge mlflow
conda install -c conda-forge/label/broken mlflow
conda install -c conda-forge/label/cf202003 mlflow
```

> **_NOTE:_**   You should be careful about the version if you are willing to use the auto_log function of MLflow with famous frameworks.

## Logging experiments

MLflow allow to organize the different executions into experiments where each experiment can have several runs. Organising the the executions within the same experiment would allow to compare between them. While the it could be a bit hard to compare cross experiments. This could seem a bit confusing but will be more clarified later on!

Ok! Let's get out hand s dirty ;)

Automating the tracking of your script would have to follow the following steps:

1. <strong> Starting an experiment </strong>

2. <strong>Starting a run </strong>

3. <strong> Logging the parameters </strong>

4. <strong>Logging the metrics </strong>

5. <strong>Logging the artifacts </strong>


### Puting it all together , the program would be similar to following script :)

```python
parameters = {
    'param1': val1
     ...
}
mlflow.set_experiment('myexeperiment')

with mlflow.start_run(self.run_id[appliance]):
    mlflow.log_params(params)
    # define the model with the params
    model = MyModel(params)
    # iteraive loop
    for i in range(ierations):
        # perform some calculations
        train_metric = model.train()
        mlflow.log_metrics('train_metric', train_metric)
    # log generated artifacts (figures/ results files ....)
    mlflow.log_artifacts(artifact_path="run_results")
```


## MLflow with Optuna

Optuna is an automatic hyperparameter package that allow the exploration of sevral different configurations (models) and thus would make more sense to log each configuration in a seperate run inside the same experiment. The question is how to automate that using MLflow. Well, one solution is to use MLflow inside of the objective function as follows:
```python
def objective(trial):
    run_params = {
        'param1': trial.suggest_something(...)
         ...
    }
    # Select the appropriate experiment under which you would like to save your documents.
    mlflow.set_experiment('optuna_study')

    with mlflow.start_run(): # This would create a new run for each model/iteration
        mlflow.log_params(run_params)
        # define the model with the params
        model = MyModel(run_params)
        # iteraive loop
        for i in range(ierations):
            # perform some calculations
            train_metric = model.train()
            mlflow.log_metrics('train_metric', train_metric)
        # log generated artifacts (figures, results files, model files ....)
        mlflow.log_artifacts(artifact_path="run_results")
    return metric
```

Running the optuna study as usual will be in this case automatically  tracked and you can get the results of different explored models using MLFLOW tracking API or MLFLOW UI.  


## Mlflow UI
Once the execution is done, you can simply check what happened and the obtained results from the MLflow user interface directly. The execution with MLFLOW results in folder with following hierarchy:
```
mlruns
|- 0
|- 1
  |- runId
     |- artifacts
     |- metrics
     |- params
     |- tags
     |- meta.yaml
  |- ...
  |- meta.yaml
|- ...
```

To view the interface, you have to go the parent folder where the ```mlruns``` folder was generated and run the following command in a terminal/shell: ```mlflow ui```.


