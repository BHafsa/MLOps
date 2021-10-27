# Optuna

Optuna is a tool used to automate the hyper-parameter search space. It implements several  recent algarithms that allow to  guide the search towards a more in-depth exploration of areas of interest.


## Installation

Optuna can be installed using pip or anaconda as folows:

``` 
pip install optuna
```
Or
```
conda install -c conda-forge optuna
conda install -c conda-forge/label/cf202003 optuna 
```

## Objective function and hyper-parameters definition

The firt step in using Optuna is to define the interval in which varies each value of the considered hyper-parameters. This is done inside a function called the objective that defines the values performs the calculations (e.g. training of the model) and returns the metric based on which a the best values of the params are selected is selected.  

```python
def objective(trial):
    # 1. Hyper-parameters definition.
    x = trial.suggest_float('x', -10, 10)
    # 2. Perform the core calculations.
    metric = (x - 2) ** 2
    # 3. Return the value of the metric that help to select a model
    return metric
```

After defining the hyper-parameters, the study is defined as follows. By default, the problem is a minimzation problem but this can be changed during the definition of the study.

```python
import optuna 

study = optuna.create_study()
study.optimize(objective, n_trials=100)
```