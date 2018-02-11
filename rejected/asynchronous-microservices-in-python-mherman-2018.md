# Asynchronous Microservices in Python

## Submitted by

Michael Herman

## Description

Migrating applications to a microservice architecture is a hot topic these days. While "microservice architecture" encompasses a broad spectrum of techniques and tools, generally it means breaking your application down into smaller, more modular services that are independent from each other. This allows for better testing and integration with a wide range of clients (desktop, phone, "Internet of Things" devices, etc). Docker is a frequently used tool to speed up development of a microservice architecture.

In this tutorial, you'll learn how to quickly spin up a reproducible environment with Docker to manage a number of microservices:

1. Sanic web framework + Python
1. React + JavaScript
1. Message Queue + Redis

## Audience

This is an intermediate-level tutorial geared toward those that have experience with full-stack web development with a Python-backend (i.e., Flask, Django) and client-side JavaScript framework or library (i.e., React, Angular, Vue). This tutorial also assumes prior knowledge of Docker and some experience working with a Docker-based microservice stack.

## Objectives

By the end of this tutorial, you will be able to...

1. Develop an API with the Sanic web framework
1. Configure and run services locally with Docker, Docker Compose, and Docker Machine
1. Install Sanic, Ngnix, Gunicorn, React, and Redis on an Amazon EC2 instance 1ia Docker machine
1. Create a Single Page Application with React components
1. Configure a message queue along with Redis


## Outline


Introduction (30 minutes - (100% lecture))

1. About Me
2. Objectives
3. Architecture
4. Why Microservices?
5. Async vs Sync
6. Project Setup

Containers and Services (1.5 hours - (30 minute lecture, 1 hour hands-on))

1. Sanic web framework + Python
2. Message Queue + Redis
3. React + JavaScript
4. Workflow
5. Nginx

Deployment (30 minutes (100% lecture))

1. Docker Machine
2. EC2 Setup
3. Workflow

What's next? (30 minutes)

1. Next Steps
2. Questions

## Additional notes

A few things...

1. This tutorial comes from a modified version of the course at http://testdriven.io/
2. You can see my past speaking history at http://mherman.org/talks
3. I run the NodeJS Meetup in Denver/Boulder - https://www.meetup.com/Node-js-Denver-Boulder/
4. I am a former lead web development instructor for Galvanize, where I taught full-stack JavaScript
5. I lead a similar tutorial at the Boulder Python meetup - http://mherman.org/presentations/microservices-flask-docker
