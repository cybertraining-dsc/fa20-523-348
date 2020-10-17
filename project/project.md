# Benchmarking Multi-Cloud Auto Generated AI Services

[Gregor von Laszewski](https://laszewski.github.io), 
Anthony Orlowski, [fa20-523-310](https://github.com/cybertraining-dsc/fa20-523-310/), 
Caleb Wilson, [fa20-523-348](https://github.com/cybertraining-dsc/fa20-523-348/), 
Vishwanadham Mandala, [fa20-523-325](https://github.com/cybertraining-dsc/fa20-523-325/), 

[Edit](https://github.com/cybertraining-dsc/fa20-523-348/blob/master/project/project.md)

{{% pageinfo %}}

## Abstract

In this wor we are benchmarking auto generated cloud REST services on various clouds. In todays application scientist want to share their services with a wide number of collegues while not only offereing the services as bare metal programs, but exposing the functionality as a software as a service. For this reason a tool has been debveloped that takes a regular python function and converts it automatically into a secure REST service. We will create a number of AI REST services while using examples from ScikitLearn and benchmark the execution of the resulting REST services on various clouds. The code will be accompanied by benchmark enhanced unit tests as to allow replication of the test on the users computer. A comparative study of the results is included in our evaluation.

Contents

{{< table_of_contents >}}

{{% /pageinfo %}}

**Keywords:** cloudmesh, AI service, REST, multi-cloud

## Introduction

We will develop benchmark tests that are pytest replications of Sklearn artificial intelligent alogrithms. These pytests will then be ran on different cloud services to benchmark different statistics on how they run and how the cloud performs. The team will obtain cloud service accounts from AWS, Azure, Google, and OpenStack. To deploy the pytests, the team will use Cloudmesh and its Openapi based REST services to benchmark the performance on different cloud services. Benchmarks will include components like data transfer time, model train time, model prediction time, and more. The final project will include scripts and code for others to use and replicate our tests. The team will also make a report consisting of research and findings. So far, we have installed the Cloudmesh OpenAPI Service Generator on our local machines. We have tested some microservices, and even replicated a Pipeline Anova SVM example on our local machines. We will repeat these processes, but with pytests that we build and with cloud accounts. 

## Cloudmesh

Cloudmesh is a service that enables users to access multi-cloud environments easily. Cloudmesh is an evolution of previous tools that have been used by thousands of users. Cloudmesh makes interacting with clouds easy by creating a service mashup to access common cloud services across numerous cloud platforms. Documentation for Cloudmesh can be found at: 

* <https://cloudmesh.github.io/cloudmesh-manual/> [^1]

Code for cloud mesh can be found at: 

* <https://github.com/cloudmesh/> [^2]

Examples in this paper came from the cloudmesh openapi manual which is located here: 

* <https://github.com/cloudmesh/cloudmesh-openapi> [^3].

Information about cloudmesh can be found here: 
* <https://cloudmesh.github.io/cloudmesh-manual/preface/about.html> [^4]

Various cloudmesh installations for various needs can be found here: 

* <https://cloudmesh.github.io/cloudmesh-manual/installation/install.html> [^5]


## Algorithms and Datasets

This project uses a number of simple example algorithms and datasets. We have chosen to use the once included in Scikit Learn as they are widel known and can be used by others to replicate our benchmarks easily. Nevertheless, it will be possible to integrate easily other data sources, as well as algorithms due to the generative nature of our base code for creating REST services. 

Within Skikit Learn we have chosen the following examples:

* **Pipelined ANOVA SVM**: A code thet shows a pipeline running successively a univariate feature selection with anova and then a SVM of the selected features [^6].




aSklearn algorithms replicated and pytests. How the pytests perform on various cloud environments. 

## Deployment

The project is easy to replicate with our detailed instructions. First you must install Cloudmesh OpenAPI whihch can be done by the follwoing steps:

```
python -m venv ~/ENV3
source ~/ENV3/bin/activate 
mkdir cm
cd cm
pip install cloudmesh-installer
cloudmesh-installer get openapi 
cms help
cms gui quick
#fill out mongo variables
#make sure autinstall is True
cms config set cloudmesh.data.mongo.MONGO_AUTOINSTALL=True
cms admin mongo install --force
#Restart a new terminal to make sure mongod is in your path
cms init
```

As a first example we like to test if the deployment works by using a number of simple commands we execute in a terminal.

```
cd ~/cm/cloudmesh-openapi

cms openapi generate get_processor_name \
    --filename=./tests/server-cpu/cpu.py

cms openapi server start ./tests/server-cpu/cpu.yaml

curl -X GET "http://localhost:8080/cloudmesh/get_processor_name" \
     -H "accept: text/plain"
cms openapi server list

cms openapi server stop cpu
```
The output will be a string containing your computer.

TODO: how does the string look like

Next you can test a more sophiticated example. Here we generate from a python function a rest servive. We consider the following function definition in which a float is returned as a simple integer

```
def add(x: float, y: float) -> float:
    """
    adding float and float.
    :param x: x value
    :type x: float
    :param y: y value
    :type y: float
    :return: result
    :return type: float
    """
    result = x + y

    return result
```

Once we execute the following lines in a terminal, the result of the addition will be calculated in the REST service and it is returned as a string.

```
cms openapi generate add --filename=./tests/add-float/add.py
cms openapi server start ./tests/add-float/add.yaml 
curl -X GET "http://localhost:8080/cloudmesh/add?x=1&y=2" -H  "accept: text/plain"
#This command returns
> 3.0
cms openapi server stop add
```

As we often also need the information as a REST service, we provide in our next example a jsonified object specification. 


```
from flask import jsonify

def add(x: float, y: float) -> str:
    """
    adding float and float.
    :param x: x value
    :type x: float
    :param y: y value
    :type y: float
    :return: result
    :return type: float
    """
    result = {"result": x + y}

    return jsonify(result)

```

The result will include a json string returned by the service.

```
cms openapi generate add --filename=./tests/add-json/add.py
cms openapi server start ./tests/add-json/add.yaml 
curl -X GET "http://localhost:8080/cloudmesh/add?x=1&y=2" -H  "accept: text/plain"
#This command returns
> {"result":3.0}
cms openapi server stop add
```

These examples are used to demonstrate the ease of use as well as the functionality for those that want to replicate our work.

## Pipiline ANOVA SVM

Next we demonstrate how oto run the Pipeline ANOVA example.

```
$ pwd
~/cm/cloudmesh-openapi

$ cms openapi generate PipelineAnovaSVM \
      --filename=./tests/Scikitlearn-experimental/sklearn_svm.py \
      --import_class --enable_upload

$ cms openapi server start ./tests/Scikitlearn-experimental/sklearn_svm.yaml
```

After running these commands, we opened a web user interface. In the user interface, we uploaded the file iris data located in ~/cm/cloudmesh-openapi/tests/ Scikitlearn-experimental/iris.data

We then trained the model on this data set by inserting the name of the file we uploaded `iris.data`. Next, we tested the model by clicking on make_prediction and giving it the name of the file iris.data and the parameters `5.1`, `3.5`, `1.4`, `0.2`

The response we received was `Classification: ['Iris-setosa']`

Lastly, we close the server:

```
$ cms openapi server stop sklearn_svm
```

This process can easily be replicated when we create more service examples that we derive from existing sklearn examples. We benchmark these tests while wrapping them into pytests and run them on various cloud services. 

## Using unit tests for Benchmarking

TODO: This section will be expanded upon

* Describe why we can unit tests
* Describe how we access multiple clouds

  ```
  cms set cloud=aws
  # run test
  cms set cloud=azure
  # run test
  ```
  
* Describe the Benchmark class from cloudmesh in one sentence and how we use it


## Limitations

Azure has updated their libraries and discontinued the version 4.0 Azure libraries. We have not yet identified if code changes in Azure need to be conducted to execute our  code on Azure


## References

[^1]: Cloudmesh Manual, <https://cloudmesh.github.io/cloudmesh-manual/> 
[^2]: Cloudmesh Repositories, <https://github.com/cloudmesh/>
[^3] Cloudmesh OpenAPI Repository for automatically generated REST services from Python functions <https://github.com/cloudmesh/cloudmesh-openapi>.
[^4] Cloudmesh Manaual Preface for cloudmesh,  <https://cloudmesh.github.io/cloudmesh-manual/preface/about.html>
[^5]: Cloudmesh Manual, Instalation instryctions for cloudmesh <https://cloudmesh.github.io/cloudmesh-manual/installation/install.html>
[^6]: Scikit Learn, Pipeline Anova SVM, <https://scikit-learn.org/stable/auto_examples/feature_selection/plot_feature_selection_pipeline.html>



 

