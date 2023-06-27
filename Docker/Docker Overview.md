[Docker](https://www.docker.com/)

[Docker Docs](https://docs.docker.com/?_gl=1*puty78*_ga*MTA5MTIxODg4NC4xNjg2MjI3NTcx*_ga_XJWPQMJYHQ*MTY4NzgwNDUxNy4zLjEuMTY4NzgwNDUxOC41OS4wLjA.)

[.NET and Docker](https://learn.microsoft.com/en-us/dotnet/core/docker/introduction)


# How to run a container?

1. Get sample app via github:
```
   Examples:
   git clone https://github.com/docker/welcome-to-docker
   
   https://github.com/dannydel/AspNetRestApiContainerFinal.git
```

2. In terminal or command line go to where you cloned the repo.
3.  Go into the folder that has the docker proj
4. Run the following command (this depends on what the name of the docker file is for the )
```
<file-name> build
```
Examples:
`docker-compose build`
`docker-test build`

5. Once it is done building you can run it by either writing
```
   docker run -t <file-name> 
	or 
	<file-name> up
```

6. When it is up and running you can visit the localhost an port you designated for the project.