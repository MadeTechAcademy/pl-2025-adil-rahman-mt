# Using docker
There's a basic node app in the docker-app folder.

## Tasks

- Build a docker image using the given dockerfile
- Run the image you have built in a local container (in detached mode)
- Visit the site at http://localhost:3000
- Check the logs of the running app in the container
- Stop and remove the container

### Notes

I initially ran the docker image using the command `docker run pipelines2025/ex1/app`. On the terminal, it seemed to be successful because there was an output that said `... Listening on port 3000` so I tried to view the app by visiting `http://localhost:3000/` but the browser said localhost refused to connect. I couldn't figure out what the issue was but Tess directed me to the docker docs where I found a command to publish a port using the `-p` flag. I managed to view the app after running the command `docker run -p 3000:3000 pipelines2025/ex1/app`. This mapped port 3000 on my machine to port 3000 inside the container, allowing me to access the site at `http://localhost:3000/` on my machine.

## Stretch task
Consider how you might use `docker compose` tools to locally to build, spin up, shut down and clean up containers

### Record your results
Add your thoughts here and add any files you created into the exercise folder


