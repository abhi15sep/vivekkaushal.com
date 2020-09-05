---
title: "Deploying a Spark Model with REST Inference API"
date: 2020-8-28
categories:
- Guide
- MLOps
tags:
- Machine Learning
cover: images/spark.png
thumbnail: images/spark.png
---

Deploying a machine learning model built with Apache Spark isn't as straight forward as the deployment of a PyTorch model or a TF model. Especially when you're planning on having  a REST API for inference requests. One way of going about it is use [MLeap](https://mleap-docs.combust.ml/), but that would require modifications to training code, as MLeap relies on it's own serialization.

The best approach that I've found is using [Openscoring](https://github.com/openscoring/openscoring) and **PMML (Predictive Model Markup Language)**. PMML is a an XML based markup language that stores your predictive model and openscoring is used to create the inference REST API. The steps for doing so are as follows:

<!-- more -->

## Converting your Model to PMML

There are two ways of getting this done. One way is to use the [JPMML Converter](https://github.com/jpmml/jpmml-sparkml). The documentation is fairly intuitive and can be quickly setup for conversion. The second, and perhaps much easier way to convert your model ot PMML is to use the inbuit [Spark PMML Model Export](https://spark.apache.org/docs/2.3.0/mllib-pmml-model-export.html). Once you've exported your Spark model to PMML, we will look at deployment over the following steps.

## Setting up Openscoring

In my opinion the quickest way to setup openscoring is to use it's docker deployment. If you do not have docker setup, you can follow the [documentation on Docker](https://docs.docker.com/get-docker/). Once docker is setup, do the following:

- clone the [openscoring-docker] repository
- in *openscoring-docker/application.conf* change the *adminAddress* from *[localhost]* to *[*]*. This would enable you to deploy models from the host machine while accessing the docker deployment.
- From the openscoring repository root, open *Dockerfile* and change *ARG version=2.0.1* to *ARG version=2.0.2*. Openscoring version 2.0.2 has added support for PMML 4.4. 
- From the repository root, build the docker image
  ```
    docker build -t openscoring/openscoring:latest .
  ```
- Run a docker image with suitable port mapping
  ```
  docker run -p <_REST_API_inference_port_>:8080 openscoring/openscoring:latest
  ```

## Model Deployment

Almost done! You have your model converted and the deployment environment setup. Now comes the easy part -- actually deploying your model and testing for inference. You can use [openscoring's REST API](https://github.com/openscoring/openscoring#rest-api) to deploy your model. Make sure you replace the port *8080* in the mentioned sample API requests with the *_REST_API_inference_port_* you entered while setting up your openscoring docker container.

This should get your job done. If you have any queries, feel free to drop a comment below or reach out to me.