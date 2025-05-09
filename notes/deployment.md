# Deployment

## The problem (money & motivation)

I've been developing web applications for years. But I never deployed them to production. Why? Because I didn't know how. I was drowning in an ocean of providers and platforms and expensive fees.

Back then I was using Next.js and was forced to use Vercel to deploy these applications. Not a big problem, but my applications needed a database. My only options for a database were expensive ready-made options. Paying $10/application/month was too steep for me.

Not being able to deploy my projects to production often lead to me abandoning projects out of lack of motivation. After years of this, I wanted a simple solution. I rented a cheap VPS from DigitalOcean, and I `git clone`'d my source code from GitHub onto the server. I'd `npm install` and `npm build` and then `npm start` my application. Following the DigitalOcean tutorials, I was able to configure Nginx to forward traffic for `myapp.jensmeindertsma.com` to `localhost:3000` and then use a different port for each application I'd make. Lastly I installed PostgreSQL and created a database for each application.

## Redemption

For the first time, something I made was running on the internet for an affordable price. But every time I'd modify my source code, I'd have to SSH to the server, `git pull` my latest changes, stop my app, rebuild the project and start the new app. I wanted automated deployment.

This is when I came across [Dokku](https://dokku.com/), a CLI interface focused around orchestrating Docker and Nginx to automatically deploy applications. You could build your Dockerfile locally, upload the image over SSH, and then issue a redeployment. Dokku would configure Nginx for you. This was the first time I heard about Docker, but I immediately liked the idea of deployment multiple applications on the same system in an isolated way, each container only exposing a port for requests and a port to connect to the database.

However, Dokku does not manage the database for you. Running a single PostgreSQL server which all applications connect to is both a single point of failure, and dangerous in terms of security. If one application is compromised all the data can be extracted for all applications. But it was okay for a start, at least I had something working after all these years and could focus on development.

I worked up a GitHub Actions workflow to, on new commits, build a new Docker image, upload it to the server, and issue a redeploy.

## The idea

Dokku is not a running service on the machine. It is a large command line interface that can manage Nginx, Docker, certificates and more. After pushing your new image, you have to SSH into the server and use the CLI to issue a new deployment. I felt this was not optimal but I accepted it for now.

**TODO explain `deploy.sh` and the `/var/lib/sail` directory.**
