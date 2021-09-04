These Docker files allows you to run a PHP Symfony Framework project.



## Extra
The container has XDebug and MYSQL dependencies loaded.

## Usage

You can execute the container using this command:

First time
```bash
docker-compose up -d --build
```

If you shutdown your computer or you down the container and if you don't make any changes to the Dockerfile or docker-compose.yml settings, you can run the container again using this command:
```bash
docker-compose up -d
```

## Adjust Docker volume permissions
If you are using Docker Engine in a Linux Virtual Machine, you need put the following permissions to Docker volume

```bash
chown -R [user]:www-data volumes/project
chmod 775 -R  volumes/project
```

If you add new files remember to fix the permissions again.

This is necessary to modify the volume files outside the Docker container.

## Create a new project

You can create a new project following these steps:

1. Access the php container using this command: docker exec -it [container name] bash
2. To  create a new project 
2.1 Create a directory in (for example, app)
2.2 Execute cd app.
2.3 Execute the command: "symfony new ."

PHP Symfony Framework has your own test server. You can execute it with this command: symfony server:start

If the server execution doesn't have any trouble you can see the mesage:

```bash
 [OK] Web server listening
      The Web server is using PHP FPM 7.4.14       
      http://127.0.0.1:8000      
```
If your case is Docker Engine in a Virtual Machine you can access using the brigde IP address asigned for VirtualBox.


PHP Symfony Framework requires a permissions fixing  to make sure that some directories are writable.(https://symfony.com/doc/current/setup/file_permissions.html)

Inside the Docker container execute these commands:

```bash
HTTPDUSER=$(ps axo user,comm | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1)
setfacl -dR -m u:"$HTTPDUSER":rwX -m u:$(whoami):rwX var
setfacl -R -m u:"$HTTPDUSER":rwX -m u:$(whoami):rwX var
```

## Debugging application

These docker-compose has enable xDebug to allow you to debug your PHP Symfony application.

I did the xDebug configuration using Visual Studio Code(https://code.visualstudio.com/) and PHP Debug extension(https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug)

This is the Visual studio launch.json configuration:

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for Xdebug",
            "type": "php",
            "request": "launch",
            "port": 9003
        },
        {
            "name": "Launch currently open script",
            "type": "php",
            "request": "launch",
            "program": "${file}",
            "cwd": "${fileDirname}",
            "port": 0,
            "runtimeArgs": [
                "-dxdebug.start_with_request=yes"
            ],
            "env": {
                "XDEBUG_MODE": "debug,develop",
                "XDEBUG_CONFIG": "client_port=${port}"
            }
        },
        {
            "name": "Launch Built-in web server",
            "type": "php",
            "request": "launch",
            "runtimeArgs": [
                "-dxdebug.mode=debug",
                "-dxdebug.start_with_request=yes",
                "-S",
                "localhost:0"
            ],
            "program": "",
            "cwd": "${workspaceRoot}",
            "port": 9003,
            "serverReadyAction": {
                "pattern": "Development Server \\(http://localhost:([0-9]+)\\) started",
                "uriFormat": "http://localhost:%s",
                "action": "openExternally"
            }
        }
    ]
}
```

## Use MySQL container.
This repository includes a MySQL's Docker image. If the container is running, you can access via any MySQL client, for example Squirrel SQL or MySQL Workbench via 127.0.0.1 ip and 3306 port.

The communication between containers is through the "mysql" alias, the name of the service in docker-compose.yaml

## Import an existing project

If you want to import and existing project, you only need to copy the files inside symfony directory container. You remmeber that this directory is accesible beacuse is the Docker volume.

If you want to clone a Git repository, you only should execute the git clone command inside Docker container pointing to the project directory, in this case **/var/www/symfony**

**Example**: git clone [repository url]  /var/www/symfony

## License
[MIT](https://choosealicense.com/licenses/mit/)
