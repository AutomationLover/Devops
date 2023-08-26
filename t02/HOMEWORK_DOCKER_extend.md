---
tags: [dns, cnd, devops11, homework] 
---


In order to search for repositories in Github for docker installation practice, you can follow the steps below:

1. Go to the main Github website: https://github.com/
2. Use the search bar at the top of the page. You can input the keywords relevant to what you're looking for, for example, "Docker" or "Dockerfile" or "Docker Compose".
3. Press Enter to search. On the results page, you can filter the search results by clicking on 'Repositories' tab.
4. You can further refine the search by using the options on the left - filter by language, sort by stars, etc.



Here's an example repository that you can use for your docker lesson:

`https://github.com/docker/example-voting-app`

This example voting app consists of:

- A Python webapp which lets you vote between two options
- A Redis queue which collects new votes
- A .NET worker which consumes votes and stores them in…
- A Postgres database backed by a Docker volume
- A Node.js webapp which shows the results of the voting in real time

It's even got a diagram that shows the whole system architecture, and it has a simple 'Getting started' section with easy-to-follow installation instructions. 

To get this repository, you can clone it to your local machine using the command:

```
git clone https://github.com/docker/example-voting-app.git
```
Then, navigate to the cloned repository:
```
cd example-voting-app
```
There, you will find a `README.md` file with detailed instructions on how to get the app up and running. It uses Docker Compose, so your students will learn how to use a `docker-compose.yml` file to define and run multi-container Docker applications. 

By following the instructions in the README file, you should be able to install the app in Docker. This would be a practical and interesting exercise for your students, as it involves multiple components working together in a Dockerized environment.


## FAQ
if you get error below, please make sure you login docker well.
```
 % docker compose up

[+] Running 0/2
 ⠿ db Error                                                                                                                                    2.7s
 ⠿ redis Error                                                                                                                                 2.7s
Error response from daemon: Head "https://registry-1.docker.io/v2/library/redis/manifests/alpine": unauthorized: incorrect username or password
```
