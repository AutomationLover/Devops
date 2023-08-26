---
tags: [docker, devops11, homework] 
---

## Error
```
Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:5000 -> 0.0.0.0:0: listen tcp 0.0.0.0:5000: bind: address already in use
```

## TS Steps

This error is occurring because the port 5000 on your machine is already in use by another application. Here are a few ways to solve it:

1. **Stop the application using the port**: If you know which application is using the port and it's not necessary for it to be running, you can stop it to free the port.

2. **Change the port in the Docker Compose file**: If you can't stop the application using the port, or you don't know what it is, you can change the port that Docker is trying to use in your `docker-compose.yml` file.

Here's how to do it: You will find a line in your `docker-compose.yml` file that looks like this:

```yaml
ports:
  - "5000:5000"
```

This line is telling Docker to map port 5000 inside the Docker container to port 5000 on your host machine. You can change the host port to something else, like 5001:

```yaml
ports:
  - "5001:5000"
```

This way, your application inside Docker will still be running on port 5000, but it will be accessible on port 5001 on your host machine.

Don't forget to save the changes. Now, re-run the `docker compose up` command. Your application should now be accessible at `localhost:5001`.

If you still face issues, you might want to check which application is using port 5000. You can do this with the following command in your terminal:

For Linux/Mac:
```bash
sudo lsof -i :5000
```

For Windows:
```bash
netstat -ano | findstr :5000
```

This will give you the PID (process ID) of the application that's using the port. If the application is not needed, you can kill it using `kill <PID>` on Linux/Mac or `taskkill /PID <PID>` on Windows. Then try running `docker compose up` again.


