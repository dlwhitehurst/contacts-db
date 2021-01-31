# contacts-db
Configured, schema, and test data for a customized MySQL container

This project contains the artifacts necessary to create a custom MySQL image 
that uses the Docker Hub reference image mysql:latest and when this container
is first run, any schema or data scripts are run thereby pre-configuring this
database.

When the `Dockerfile` is used to create the `contacts-db` MySQL image, a database
and a contacts table are created with the image. The database table is also pre-
loaded with contact data. This image will be used with the `contacts` application
as part of a set of Kubernetes objects that make up a hosted `contacts` application
whole.

### Docker
Building the Docker image is easy. The following command assumes a `Dockerfile`
is available in the current directory.

```shell
sudo docker build -t contacts-db .
```
Next start your new image with:

```shell
docker run -d -p 3306:3306 --name contacts-db \
-e MYSQL_ROOT_PASSWORD=oursecret contacts-db
```

Connect to your running image and you will find that the database and contacts
table are there and the table is pre-loaded for use with the application
`contacts`.

### MicroK8S
Building the image for MicroK8S is not much different than the Docker build
described above. We do however, need to tag the built image for upload into
the MicroK8S image registry. This is done with a build:

```shell
sudo docker build -t localhost:32000/contacts-db:registry .
```

Build the image in the same environment as your MicroK8S. Once the image is
built, push the image to the MicroK8S registry. Please note that `registry`
and `storage` need to be enabled. When you enable `registry`, the `storage`
add-on is enabled by default. Now push your new MySQL custom image:

```shell
sudo docker push localhost:32000/contacts-db:registry
```
The registry tag is just a marker to `tag` or identify the artifact as the
one that's slated for our MicroK8S (Kubernetes) cluster.