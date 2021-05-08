---
title: "Ispovesti.ml"
excerpt: "Ispovesti.ml is a site based on gpt-2 text generation."
header:
  #image: /assets/images/foo-bar-identity.jpg
  #teaser: /assets/images/foo-bar-identity-th.jpg
sidebar:
  - title: "Role"
    text: "Maintainer"
  - title: "Takeaways"
    text: "Gained experience in developing and deploying a React based frontend and a python-flask based backend solution. Experimented and successfully deployed an asynchronous client-server text generation service. Used web crawling tools and gpt-2 based libraries to train a gpt-2 model on the target data."
  - title: "Repositories"
    text: "[Backend](https://github.com/dojitza/ispovesti.ml-backend) [Frontend](https://github.com/dojitza/ispovesti.ml-frontend)"
  #- title: "Currently online"
  #  text: "[ispovesti.ml](http://ispovesti.ml)"
---

Ispovesti.ml allows users to generate confessions similar to those of a popular Adriatic region website ispovesti.com. Users can provide an optional prefix and post these confessions under a pseudonym. Also, all users can react to generated confessions.

Ispovesti.ml's frontend is a react-based single-page app. The design is mobile first and uses bootstrap for the main UI components.

The backend service is written in flask and backed by SQLite. The task of confession generation was extracted and assigned its own process due to the large demands on cpu and memory. Inter-process communication was used in the form of message queues handled by rabbitmq.

The generation process was built using a gpt-2 library [gpt-2-simple](https://github.com/minimaxir/gpt-2-simple). With its help the 355M hyperparameter gpt-2 model was tuned to our data, which was taken directly from the source website. A total of around 120K confessions were used.

Currently the website is not functional due to the high costs associated with running the backend service.

