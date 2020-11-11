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
  - title: "Currently online"
    text: "[ispovesti.ml](http://ispovesti.ml)"
---

Ispovest.ml allows users to generate confessions similar to those of a popular Adriatic region website ispovesti.com. Users can provide an optinal prefix and post these confessions under a pseudonym. Also, all users can react to generated confessions, which are then selected by a simple algorithm to be published on the 'main list' of generated confessions.

Ispovesti.ml's frontend is a simple react-based single-page app. The design is mobile first and uses bootstrap for the main UI components.

The backend service is written in flask and backed by SQLite. Most of the service is simple CRUD logic and database interfacing. However, the task of confession generating was extricated and assigned its own process due to its extreme demands on cpu and memory. Inter-process communication was used in the form of message queues handled by a popular message broker - rabbitmq.

The generation process uses a popular gpt-2 generation library [gpt-2-simple](https://github.com/minimaxir/gpt-2-simple). The same library was used to fine tune the 355M hyperparameter model of gpt-2 with our training data. The training data itself was taken directly from the source website. A total of around 120K confessions were used.

