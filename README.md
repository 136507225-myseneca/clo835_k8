# Install the required MySQL package

sudo apt-get update -y
sudo apt-get install mysql-client -y

# Running application locally

pip3 install -r requirements.txt
sudo python3 app.py

# Building and running 2 tier web application locally

### Building mysql docker image

`docker build -t my_db -f Dockerfile_mysql . `

### Building application docker image

`docker build -t my_app -f Dockerfile . `

### Running mysql

`docker run -d -e MYSQL_ROOT_PASSWORD=pw  my_db`

### Get the IP of the database and export it as DBHOST variable

`docker inspect <container_id>`

### Example when running DB runs as a docker container and app is running locally

```
    export OBJECT_NAME='senaca_image.png'
    export BUCKET_NAME="clo800backgroundimages"
    export IMAGE_URL='images/background.jpg'
    export HEADER_NAMES="Teddy, Niharika, Tobe"
    export DBUSER=root
    export DATABASE=employees
    export DBPWD=pw
    export APP_COLOR=blue
    export DBHOST=172.17.0.2
    export DBPORT=3306
```

### Run the application, make sure it is visible in the browser

## HPA Testing

### Step 2: Deploy the Metrics Server (If Not Already Done)

If you haven't already deployed the metrics server in your cluster, you can do so by applying the appropriate YAML file from the official Kubernetes repository. This step is crucial for HPA to function.

```shell
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

### Step 3: Define and Deploy the Horizontal Pod Autoscaler

This HPA targets the web-app-deployment deployment in the application namespace. It adjusts the number of replicas between 1 and 10 to maintain an average CPU utilization across all pods at 50%.

### Step 4: Deploy the HPA

```shell
kubectl apply -f hpa.yaml
```

### Step 5: Generate Load to Test Auto-Scaling

You can generate load to your application to see the auto-scaling in action. One way to do this is by using a tool like hey or ab

```shell
hey -n 10000 -c 100 http://<your-application-url>/
```

Replace <your-application-url> with the actual URL where your application is accessible.
