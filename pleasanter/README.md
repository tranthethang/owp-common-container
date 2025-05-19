### Start Pleasanter

1. Execute CodeDefiner
```
docker compose run codedefiner _rds
```

2. Start Pleasanter
```
docker compose run -p 50001:8080 pleasanter
```

### Accessing
The port number for accessing Pleasanter is specified with -p at startup and is 50001. Please change it according to your environment. Let's access it now.
```
http://localhost:50001/⁠
```
Now, enter the initial user and password.
After logging in, you will be prompted to change your password.

### Terminate
You can terminate the container by pressing Ctrl-C on the screen where you launched Pleasanter.
This will delete the created resources and exit.

docker compose down -v --remove-orphans


### Docs
https://hub.docker.com/r/implem/pleasanter

https://github.com/Implem/Implem.Pleasanter
