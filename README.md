# Tilehut

| PROD                                                                                                    |
| ------------------------------------------------------------------------------------------------------- |
| ![Build Status](https://bamboo.intapps.it/plugins/servlet/wittified/build-status/LAB-LABTILEHUTSERVICE) |
| ![prod Deployment](https://bamboo.intapps.it/plugins/servlet/wittified/deploy-status/95978064)          |

**Description**: see [README](./TILEHUT_README.md).

Endpoints:

```txt
api-tilehut-a.<endpoint>
api-tilehut-b.<endpoint>
api-tilehut-c.<endpoint>
api-tilehut-d.<endpoint>
```

## Usage

### Create/Update S3 Bucket

The S3 bucket is managed via CFN Template in the .aws folder

```bash
aws cloudformation <create|update>-stack \
  --profile movelab \
  --stack-name lab-tilehut-tiles \
  --template-body file://./.aws/tiles.s3.yaml \
  --parameters ParameterKey=ServiceName,ParameterValue=lab-tilehut-tiles
```

### Build Image

```bash
docker build . -t docker.intapps.it/tilehut:$(git rev-parse HEAD)
```

### Start

```bash
docker run -p 8000:8000 docker.intapps.it/tilehut:$(git rev-parse HEAD)
```

### Add Tiles

Upload the tiles into the s3 bucket via

```bash
aws s3 cp /path/to/file.name s3://lab-tilehut-tiles --profile movelab --region eu-west-1 --recursive
```

Add the following line to the dockerfile

```bash
RUN wget -O /usr/src/app/data/<filename>.mbtiles http://lab-tilehut-tiles.s3-website-eu-west-1.amazonaws.com/<filename>.mbtiles
```
