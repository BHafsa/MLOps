# Data Versioning Control

Data verisioning control is a git like tool that extends github and allow o track changes made to large files while saving them in remote storage. It thus allows to go backward and forward in time and tracking all changes that occured to the data. The term data can be confusing but in reality it refers to all large files that are of interest to the developement process (e.g. the model weights, the output of a simulation, ...).

## Installation 

It can be installed using conda or pip. Please check the following [link](https://github.com/iterative/dvc#installation) for other installation options.

``` 
pip install dcv 
```

Or

```
conda install -c conda-forge mamba # installs much faster than conda
mamba install -c conda-forge dvc 
```


## Initializing dvc

The syntaxe is straight forward and very similar to git. The following steps provide a more details of the required steps.

1. Create a folder for the data and add download the data.
```
mkdir data
dvc get link_to_the_data_registry -o data/dataset.xml
```

2. Add the dataset/file to dvc and add the dvc files to git so that it they are tracked. The ```.dvc``` file contains a metadata summary for the versions of the dataset.

``` 
dvc add data/dataset.xml
git add data/.gitignore data/dataset.xml.dvc
git commit -m "data added"
```

## Connecting DVC with your Google Drive

Google drive is the simplest storage that can be used with dvc especially if you have an account 

1. Create a folder in Google drive.
2. Go to the link and copy the ```folder_id``` https://drive.google.com/drive/folder/folder_id.

```
dvc remote add -d storage gdrive:folder_id
```

3. Commit your changes.

``` 
git commit .dvc/config -m "added remote storage
```

4. Push the data from local storage to remote registry. After this command, you will be provided with a link to authenticate and provide an authentification token that will allow to verify your identity and link dvc to you Google drive account.

```
dvc push 
```

