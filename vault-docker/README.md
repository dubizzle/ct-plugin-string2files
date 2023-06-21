# Consul-Template Plugin: string2files

- This is used to use that plugin in the vault injector to save the secrets to seperate files

# Create the image on two different machines - ARM and AMD machine and later push it using the manifest.
# Update the tag whenever creating a new image
IMAGE_HOST=dubizzledotcom
IMAGE_NAME=vault-injector-plugin
IMAGE_VERSION=v4

docker build -t $IMAGE_HOST/$IMAGE_NAME:${IMAGE_VERSION} .

docker tag $IMAGE_HOST/$IMAGE_NAME:$IMAGE_VERSION $IMAGE_HOST/$IMAGE_NAME:${IMAGE_VERSION}-arm64
docker push ${IMAGE_HOST}/${IMAGE_NAME}:${IMAGE_VERSION}-arm64

#### Manifest

docker manifest create $IMAGE_HOST/$IMAGE_NAME:$IMAGE_VERSION \
    $IMAGE_HOST/$IMAGE_NAME:$IMAGE_VERSION-amd64 \
    $IMAGE_HOST/$IMAGE_NAME:$IMAGE_VERSION-arm64

docker manifest annotate $IMAGE_HOST/$IMAGE_NAME:$IMAGE_VERSION $IMAGE_HOST/$IMAGE_NAME:$IMAGE_VERSION-amd64 --arch amd64
docker manifest annotate $IMAGE_HOST/$IMAGE_NAME:$IMAGE_VERSION $IMAGE_HOST/$IMAGE_NAME:$IMAGE_VERSION-arm64 --arch arm64

docker manifest push $IMAGE_HOST/$IMAGE_NAME:$IMAGE_VERSION