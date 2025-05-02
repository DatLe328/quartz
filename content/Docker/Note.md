# Introduction
## Why docker?
If we take a look at **virtual machines (VMs)**, they tend to have:
- A **large footprint**
- **Slow boot times**
However, if we only need to:
- Run **small, lightweight tasks**
- Ensure **portability** across environments
- Execute with **simple commands**
Then **Docker** is the better choice.
## Some basic docker command
### Management commands

|     Command      |        Description         |
| :--------------: | :------------------------: |
|  `docker info`   | Display system information |
| `docker version` |  Display system's version  |
|  `docker login`  | Login to a Docker registry |
### Running and stopping
| Command                            | Description                         |
| ---------------------------------- | ----------------------------------- |
| `docker pull [imageName]`          | Pull an image from a registry       |
| `docker run [imageName]`           | Run containers                      |
| `docker run -d [imageName]`        | Detached mode                       |
| `docker start [containerName]`     | Start stopped containers            |
| `docker ps`                        | List running containers             |
| `docker ps -a`                     | List running and stopped containers |
| `docker stop [containerName]`      | Stop containers                     |
| `docker kill [containerName]`      | **Kill containers**                 |
| `docker image inspect [imageName]` | Get image info                      |
| `docker logs [containerName]`      | Get container's logs                |
#### Run simple nginx server
`-p 8080:80`: port forwarding, it mean:
- `8080` (the left part): This is the port on your host machine. Docker will listen for incoming connections on this port of the host. 
- `80` (the right part): This is the port inside the container. Any traffic sent to port 8080 on the host will be forwarded to port 80 inside the container."

```bash
docker run -d -p 8080:80 --name webserver nginx
docker container exec -it webserver bash
```
#### Open bash with root permission
When you get access to container then we want to type `apt-get update` it will say that we don't have permission, now we need root permission but we don't know what root password is so we need to access with root permission at the beginning 
```bash
docker exec -u root -it webserver bash
```
### Cleaning up
| Command                        | Description                                     |
| ------------------------------ | ----------------------------------------------- |
| `docker rm [containerName]`    | Removes stopped containers                      |
| `docker rm $(docker ps -a -q)` | Removes all stopped containers                  |
| `docker images`                | Lists images                                    |
| `docker rmi [imageName]`       | Deletes the image                               |
| `docker system prune -a`       | Removes all images not in use by any containers |
### Building
| Command                                    | Description                                                       |
| ------------------------------------------ | ----------------------------------------------------------------- |
| `docker build -t [name:tag] .`             | **Builds an image using a Dockerfile located in the same folder** |
| `docker build -t [name:tag] -f [fileName]` | Builds an image using a Dockerfile located in a different folder  |
| `docker tag [imageName] [name:tag]`        | Tag an existing image                                             |
|                                            |                                                                   |
### Volumes
| Command                              | Description                                                    |
| ------------------------------------ | -------------------------------------------------------------- |
| `docker create volume [volumeName]`  | Creates a new volume                                           |
| `docker volume ls`                   | Lists the volumes                                              |
| `docker volume inspect [volumeName]` | Display the volume info                                        |
| `docker volume rm [volumeName]`      | Deletes a volume                                               |
| `docker volume prune`                | Deletes all volumes not mounted **!Careful with this command** |
Example:
```bash
# create a volume
docker volume create myvol

# inspect the volume
docker volume inspect myvol

# list the volumes
docker volume list

# run a container with a volume
docker run -d -p 8080:8080 --name testvol -v myvol:/app myimage

# Open bash
docker exec -it testvol bash

# Example run django app with port 8080
python manage.py runserver 0.0.0.0:8080
```

## Exercise
- Create a volume named `myvol`, then list and inspect the volume
- Create a `.env` file, install Django, then start a simple app named `courseapp`.
- Add a Dockerfile, build the image, and list images.
- Create a container instance from the image: Map host port `8080` to container port `80`.
- Connect to the running container and start the server.
- Try to run `apt-get update`
- Clean up everything you have done (**container**, **images**, **volume**)
# Docker Compose
| Command                                | Description                  |
| -------------------------------------- | ---------------------------- |
| `docker compose build`                 | Build the images             |
| `docker compose start`                 | Start the containers         |
| `docker compose stop`                  | Stop the containers          |
| `docker compose up -d`                 | Build and start              |
| `docker compose ps`                    | List what's running          |
| `docker compose rm`                    | Remove from memory           |
| `docker compose down`                  | Stop and remove              |
| `docker compose logs`                  | Get the logs                 |
| `docker compose exec [container] bash` | Run a command in a container |
When we build and run compose we can change config in comepose.yaml file (port, database, etc...)

| Command                                                  | Description                   |
| -------------------------------------------------------- | ----------------------------- |
| `docker compose --project-name test1 up -d`              | Run an instance as a project  |
| `docker compose -p test2 up -d`                          | Shortcut                      |
| `docker compose ls`                                      | List running projects         |
| `docker compose cp [containerID]:[SRC_PATH] [DEST_PATH]` | Copy files from the container |
| `docker compose cp [SRC_PATH] [containerID]:[DEST_PATH]` | Copy files to the container   |
Example for 2 last command:
- `docker compose cp my_web_app:/var/log/application.log .`: This command will copy log to the current folder
- `docker compose cp text.txt my_service:/etc/app/`: Copy file text into container
## Exercise
- With the same introduction excercise try to build and run compose with command line
# Dockerfile

## Dockerfile
## compose.yaml
### Default ports for some popular databases
- **MySQL:** `3306` (TCP/UDP)
- **PostgreSQL:** `5432` (TCP)
- **Microsoft SQL Server (MSSQL):** `1433` (TCP), `1434` (UDP - for SQL Server Browser)
- **Oracle Database:** `1521` (TCP)
- **MongoDB:** `27017` (TCP)
- **Redis:** `6379` (TCP)
- **Cassandra:** `9042` (TCP)
### Environment variables
![[Pasted image 20250413155834.png]]
![[Pasted image 20250413155859.png]]
### Networking
![[Pasted image 20250413155655.png]]
### Resource Limits
![[Pasted image 20250413155749.png]]
### Dependence
`db` need to run completely before run `app`
```yaml
services:
	app:
		image: myapp
		depends on:
			- db
	db:
		image: mysql
		networks:
			- back-tier
```
### Resart Policy
![[Pasted image 20250413162005.png]]
### Example(mysql)
> If some command require root access just add tag -u root -p then type in password
```yaml
services:
  db:
    image: mysql:latest
    container_name: mysql_container
    environment:
      - MYSQL_ROOT_PASSWORD=1234
    ports:
      - "8080:8080"
    volumes:
      - mysqlvol:/mysql
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      timeout: 5s
      retries: 5
  
volumes:
  mysqlvol:
```

## .dockerignore
```
**/.DS_Store
**/__pycache__
**/.venv
**/.classpath
**/.dockerignore
**/.env
**/.git
**/.gitignore
**/.project
**/.settings
**/.toolstarget
**/.vs
**/.vscode
**/*.*proj.user
**/*.dbmdl
**/*.jfm
**/bin
**/charts
**/docker-compose*
**/compose.y*ml
**/Dockerfile*
**/node_modules
**/npm-debug.log
**/obj
**/secrets.dev.yaml
**/values.dev.yaml
LICENSE
README.md

```
# Database initialize
- Should use Compose with environment variables.
- Two scenarios if using one init file: should not use `USE mydatabase`.
- If using two or more init files, it's fine."
Example: python flask app, mysql

`compose.yaml` we must add `[folder_name]:/docker-entrypoint-initdb.d` into volumes
> create folder name db then add .sql in there
```yaml
services:
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      DB_HOST: db
      DB_USER: root
      DB_PASSWORD: your_root_password
      DB_NAME: my_database
    depends_on:
      - db

  db:
    image: mysql:latest
    container_name: mysql-app
    environment:
      MYSQL_ROOT_PASSWORD: your_root_password
      MYSQL_DATABASE: my_database
    ports:
      - "3333:3306"
    volumes:
      - db_data:/var/lib/mysql
      - ./db:/docker-entrypoint-initdb.d
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      timeout: 20s
      retries: 3

volumes:
  db_data:
```
Dockerfile
```dockerfile
FROM python:3.9-slim-buster

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```
app.py
```python
from flask import Flask, render_template
import pymysql
import os

app = Flask(__name__)

DB_HOST = os.environ.get('DB_HOST')
DB_USER = os.environ.get('DB_USER')
DB_PASSWORD = os.environ.get('DB_PASSWORD')
DB_NAME = os.environ.get('DB_NAME')

@app.route('/')
def index():
    connection_successful = False
    users = []
    error_message = None
    try:
        connection = pymysql.connect(host=DB_HOST,
                                     user=DB_USER,
                                     password=DB_PASSWORD,
                                     database=DB_NAME,
                                     cursorclass=pymysql.cursors.DictCursor)
        connection_successful = True
        with connection.cursor() as cursor:
            sql = "SELECT * FROM users"
            cursor.execute(sql)
            users = cursor.fetchall()

    except pymysql.Error as e:
        error_message = f"Lỗi kết nối hoặc truy vấn cơ sở dữ liệu: {e}"

    finally:
        if 'connection' in locals() and connection.open:
            connection.close()

    return render_template('index.html',
                           users=users,
                           connection_successful=connection_successful,
                           error_message=error_message)

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```
# Dockerhub
## Push image to Dockerhub
Step to push your image to Docker hub
```shell
# Build your image
docker build -t my-image .

# Add image to push
docker tag my-image yourusername/my-repos:tag
# yourusername: your Dockerhub's username
# if repos hasn't created it will automatic create for you

# Push image to your repo
docker push yourusername/my-repos:tag
```
## Pull image from Dockerhub
```shell
docker pull yourusername/my-repos:tag
```
## Delete images, repos
Access https://hub.docker.com/ to remove by hand
