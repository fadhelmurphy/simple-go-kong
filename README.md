# Simple API Gateway with Golang & Kong 3

## How to run

### Build Image

``
cd header-validation && docker build -t go-kong-image .
``

### Run Container based on image name

``
docker run -d --name go-kong-container \
  -e "KONG_DATABASE=off" \
  -e "KONG_DECLARATIVE_CONFIG=/kong/config.yml" \
  -e "KONG_PLUGINS=bundled,header-validation" \
  -e "KONG_PLUGINSERVER_NAMES=header-validation" \
  -e "KONG_PLUGINSERVER_HEADER_VALIDATION_START_CMD=/kong/go-plugins/header-validation" \
  -e "KONG_PLUGINSERVER_HEADER_VALIDATION_QUERY_CMD=/kong/go-plugins/header-validation -dump" \
  -p 8000:8000 \
  --mount type=bind,source="$(pwd)"/config.yml,target=/kong/config.yml \
  go-kong-image
``