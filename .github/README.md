# Anthropic Computer Use Demo - Arch Linux Version

This is an Arch Linux-based version of the Computer Use Demo, which provides the same functionality as the original but uses Arch Linux as the base system and includes `paru` for AUR package management.

## Quickstart: Running the Docker Container

### Building the Image

First, build the local Docker image. Choose the appropriate version based on your architecture:

For x86_64/AMD64:
```bash
docker build -t computer-use-demo-arch computer-use-demo/
```

For ARM64/AArch64:
```bash
docker build -f computer-use-demo/Dockerfile.arm -t computer-use-demo-arch-arm computer-use-demo/
```

### Running with Anthropic API

```bash
export ANTHROPIC_API_KEY=%your_api_key%
docker run \
    -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \
    -v $HOME/.anthropic:/home/computeruse/.anthropic \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -it computer-use-demo-arch
```

### Running with Bedrock

#### Option 1: Using host's AWS credentials file and AWS profile

```bash
export AWS_PROFILE=<your_aws_profile>
docker run \
    -e API_PROVIDER=bedrock \
    -e AWS_PROFILE=$AWS_PROFILE \
    -e AWS_REGION=us-west-2 \
    -v $HOME/.aws:/home/computeruse/.aws \
    -v $HOME/.anthropic:/home/computeruse/.anthropic \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -it computer-use-demo-arch
```

#### Option 2: Using AWS access key and secret

```bash
export AWS_ACCESS_KEY_ID=%your_aws_access_key%
export AWS_SECRET_ACCESS_KEY=%your_aws_secret_access_key%
export AWS_SESSION_TOKEN=%your_aws_session_token%
docker run \
    -e API_PROVIDER=bedrock \
    -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID \
    -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \
    -e AWS_SESSION_TOKEN=$AWS_SESSION_TOKEN \
    -e AWS_REGION=us-west-2 \
    -v $HOME/.anthropic:/home/computeruse/.anthropic \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -it computer-use-demo-arch
```

### Running with Vertex

```bash
gcloud auth application-default login
export VERTEX_REGION=%your_vertex_region%
export VERTEX_PROJECT_ID=%your_vertex_project_id%
docker run \
    -e API_PROVIDER=vertex \
    -e CLOUD_ML_REGION=$VERTEX_REGION \
    -e ANTHROPIC_VERTEX_PROJECT_ID=$VERTEX_PROJECT_ID \
    -v $HOME/.config/gcloud/application_default_credentials.json:/home/computeruse/.config/gcloud/application_default_credentials.json \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -it computer-use-demo-arch
```

### Accessing the Demo App

Once the container is running, you can access the interfaces through:

- Combined interface (agent chat + desktop view): [http://localhost:8080](http://localhost:8080)
- Streamlit interface only: [http://localhost:8501](http://localhost:8501)
- Desktop view only: [http://localhost:6080/vnc.html](http://localhost:6080/vnc.html)
- Direct VNC connection: `vnc://localhost:5900` (for VNC clients)

### Screen Size Configuration

You can customize the screen size using the `WIDTH` and `HEIGHT` environment variables:

```bash
docker run \
    -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \
    -v $HOME/.anthropic:/home/computeruse/.anthropic \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -e WIDTH=1920 \
    -e HEIGHT=1080 \
    -it computer-use-demo-arch
```

### Development

For development, you can mount the local repository into the container:

```bash
docker run \
    -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \
    -v $(pwd)/computer_use_demo:/home/computeruse/computer_use_demo/ \
    -v $HOME/.anthropic:/home/computeruse/.anthropic \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -it computer-use-demo-arch
```

## Architecture Support

This project provides two Dockerfile variants:
- `Dockerfile`: Uses `archlinux:base-devel` as base image, optimized for x86_64/AMD64 systems
- `Dockerfile.arm`: Uses `menci/archlinuxarm:base-devel` as base image, optimized for ARM64/AArch64 systems (e.g., Apple Silicon, Raspberry Pi 4)

When running the container, make sure to use the appropriate image tag:
- For x86_64/AMD64: Use `computer-use-demo-arch` in the docker run commands
- For ARM64/AArch64: Use `computer-use-demo-arch-arm` in the docker run commands

For example, running on ARM with the Anthropic API:
```bash
export ANTHROPIC_API_KEY=%your_api_key%
docker run \
    -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \
    -v $HOME/.anthropic:/home/computeruse/.anthropic \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -it computer-use-demo-arch-arm
```

## Note About Platform Compatibility

This image is built with `linux/amd64` as the base platform. If you're running on a different architecture (e.g., ARM64/Apple Silicon), you may see a warning about platform compatibility. The image should still work, but you might experience slightly slower performance due to architecture emulation. 