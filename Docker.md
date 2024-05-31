# L3MON Docker:


## Building and Running a Docker Container

```
mkdir l3monDocker
cd l3monDocker
whet https://raw.githubusercontent.com/efxtv/L3MON/main/L3mon_Dockerfile
mv L3mon_Dockerfile Dockerfile
```


## Navigate to the Directory Containing the Dockerfile 

```
cd /path/to/your/dockerfile
```

## Build the Docker Image

```
docker build -t lemon .
```

## Run the Docker Container

```
docker run -d --name lemon_container lemon
```
