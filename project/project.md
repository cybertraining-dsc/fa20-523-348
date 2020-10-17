Team:
Gregor von Laszewski, Anthony Orlowski, Caleb Wilson, Vishwanadh Mandala

Project:
Cloudmesh Benchmarking

Topic:
Benchmarking Cloud service performance with cloudmesh openapi rest services and sklearn algorithms replicated as pytests. 

Dataset:
Sklearn algorithms replicated and pytests. How the pytests perform on various cloud environments. 

For the final project, the Cloudmesh Benchmarking team will develop benchmark tests that are pytest replications of Sklearn artificial intelligent alogrithms. These pytests will then be ran on different cloud services to benchmark different statistics on how they run and how the cloud performs. The team will obtain cloud service accounts from AWS, Azure, Google, and OpenStack. To deploy the pytests, the team will use Cloudmesh and its Openapi based REST services to benchmark the performance on different cloud services. Benchmarks will include components like data transfer time, model train time, model prediction time, and more. The final project will include scripts and code for others to use and replicate our tests. The team will also make a report consisting of research and findings. So far, we have installed the Cloudmesh OpenAPI Service Generator on our local machines. We have tested some microservices, and even replicated a Pipeline Anova SVM example on our local machines. We will repeat these processes, but with pytests that we build and with cloud accounts. 

Here is the installation process for Cloudmesh OpenAPI:
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

Here is the quickstart that we worked through:
cd ~/cm/cloudmesh-openapi

cms openapi generate get_processor_name \
    --filename=./tests/server-cpu/cpu.py

cms openapi server start ./tests/server-cpu/cpu.yaml

curl -X GET "http://localhost:8080/cloudmesh/get_processor_name" \
     -H "accept: text/plain"
cms openapi server list

cms openapi server stop cpu


Here is an example of the return a float and return a Json object microservices we tested:
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

cms openapi generate add --filename=./tests/add-float/add.py
cms openapi server start ./tests/add-float/add.yaml 
curl -X GET "http://localhost:8080/cloudmesh/add?x=1&y=2" -H  "accept: text/plain"
#This command returns
> 3.0
cms openapi server stop add


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

cms openapi generate add --filename=./tests/add-json/add.py
cms openapi server start ./tests/add-json/add.yaml 
curl -X GET "http://localhost:8080/cloudmesh/add?x=1&y=2" -H  "accept: text/plain"
#This command returns
> {"result":3.0}
cms openapi server stop add



Here is the Pipeline Anova SVM example we replicated:

$ pwd
~/cm/cloudmesh-openapi

$ cms openapi generate PipelineAnovaSVM \
      --filename=./tests/Scikitlearn-experimental/sklearn_svm.py \
      --import_class --enable_upload

$ cms openapi server start ./tests/Scikitlearn-experimental/sklearn_svm.yaml

After running these commands, we opened a web user interface. In the user interface, we uploaded the file iris data located in ~/cm/cloudmesh-openapi/tests/ Scikitlearn-experimental/iris.data

We then trained the model on this data set by inserting the name of the file we uploaded “iris.data”. Next, we tested the model by clicking on make_prediction and giving it the name of the file iris.data and the parameters 5.1, 3.5, 1.4, 0.2

The response we received was "Classification: ['Iris-setosa']"

Lastly, we closed the server:
$ cms openapi server stop sklearn_svm


We will replicate this process with pytests that we create ourselves from sklearn examples. We will then benchmark test our pytests on various cloud services. 

Cloudmesh is a service that enables users to access multi-cloud environments easily. Cloudmesh is an evolution of previous tools that have been used by thousands of users. Cloudmesh makes interacting with clouds easy by creating a service mashup to access common cloud services across numerous cloud platforms. Documentation for Cloudmesh can be found at: https://cloudmesh.github.io/cloudmesh-manual/
Code for cloud mesh can be found at: https://github.com/cloudmesh/
Examples in this paper came from the cloudmesh openapi manual which is located here: https://github.com/cloudmesh/cloudmesh-openapi.
Information about cloudmesh can be found here: https://cloudmesh.github.io/cloudmesh-manual/preface/about.html
Various cloudmesh installations for various needs can be found here: https://cloudmesh.github.io/cloudmesh-manual/installation/install.html

Thanks to Dr. Gregor von Laszewski for all the work he has put into the cloudmesh project  and for setting up and facilitating the benchmark team this semester. 


 

