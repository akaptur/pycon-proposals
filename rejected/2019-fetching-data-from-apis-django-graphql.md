# Fetching data from APIs(GitHub) using Django and GraphQL without hitting the rate limits 

## Submitted by

Manaswini Das

## Description

In this talk, I will present how I fetched data from GitHub API without hitting rate limits along with a walkthrough of my project and the pros of using GraphQl over REST in my project. In addition to this, I’ll also highlight various tools I have used to accomplish this and why I chose to add this data source and the tools. As far as GitHub API is concerned, the data which is to be pulled is already sorted and classified which makes it an excellent source for visualization. The data that is available includes meticulous information regarding pull requests, issues, activity feeds, repositories, organizations, etc, which is why I thought GitHub will be a relevant data source to add to Open Humans.

My project is important because fetching data is troublesome when it comes to rate limits and taking care of the number of API calls, and will be helpful for both beginners and intermediate developers.

I had developed this project as a part of my Outreachy internship with Open Humans Foundation. Open Humans Foundation provides a single platform to upload, connect and privately store data from a plethora of sources including 23andMe, AncestryDNA, Fitbit, Runkeeper and so on. My proposal included adding GitHub and Twitter data sources to this platform.

The workflow of this project can be described as follows:

1. A user goes to the website provided by this repo
2. A user signs up/signs in with Open Humans and authorizes the GitHub integration on Open Humans
3. This redirects the user back to this GitHub-integration website
4. The user is redirected starts the authorization with GitHub. For this, they are redirected to the Github page
5. After a user has authorized both Open Humans & GitHub, their GitHub data will be requested and ultimately saved as a file in Open Humans.
6. Regular updates of the data should be automatically triggered to keep the data on Open Humans up to date.

The whole project uses Django to fetch data from the GitHub API. Now, fetching data has a couple of challenges:

1. The GitHub API uses rate limits, which need to be respected and going over the rate limit would not yield more data but just errors
2. Getting all the data from Github takes a while, not only because of the rate limits but also because it can be a lot of data

For this reason, this application makes good use of background tasks with Celery and the Python module requests_respectful, which keeps track of API limits by storing limits in a Redis database. As Redis is already used for Celery as well this does not increase the number of non-python dependencies. To exercise rate limiting, I came across requests_respectful, a wrapper that Open Humans is using in its API integrations. It is a rate-limiting wrapper built on top of Requests by SerpentAI. This enables users to work within rate limits(user rate limiting) of any amount of services simultaneously. Requests_respectful maximizes allowed requests without going over rate limits.

Going through the API documentation, I found the v4 of GitHub API, also known as GraphQL API. GraphQL is a query language for your API, for executing queries by using a type system you define for your data and gives the power to request exactly the data that they need. GraphQL provides a complete and understandable description of the data in your API and makes it easier to aggregate from multiple sources. GraphQL requests are POST requests to the GraphQL endpoint and the response is returned as JSON matching the query. Using GraphQL in Github API v4 has enabled me to easily extract required data in a single API call.

## Audience

This talk is suitable for both beginner and intermediate level audience who have some knowledge about baics of Django and OAuth.

## Outline

1. Facts about GitHub API, what are rate limits and challenges of fetching data from APIs[4-5 mins]
2. Tools for rate limiting[2-3 mins]:
    1. Celery
    2. requests_respectful
3. GraphQL and reasons for using GraphQL[3-4 mins]
4. How does GraphQL work?[2-3 mins]
5. How OAuth works?[2-3 mins]
6. Workflow of the project and demo[5-6 mins]

## Content URLs

GitHub repository: https://github.com/manaswinidas/oh-github-source

Blog post: https://medium.com/outreachy-manaswini-das/graphql-for-building-github-source-2fed277c84c0

Slides: https://docs.google.com/presentation/d/1hejDhcNE_R5vThjLejN2TSMFULxSQenWx29GJx7WZ54/edit?usp=sharing

## Additional notes

This talk was not considered suitable for PyCon India 2019 but was accepted for DjangoCon Europe 2019. 

## Feedback

No clear feedback
