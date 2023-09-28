# localstack setup

## DynamoDB

**Create Table**

```bash
aws dynamodb --endpoint-url=http://localhost:4566 create-table \
    --table-name Music \
    --attribute-definitions \
        AttributeName=Artist,AttributeType=S \
        AttributeName=SongTitle,AttributeType=S \
    --key-schema \
        AttributeName=Artist,KeyType=HASH \
        AttributeName=SongTitle,KeyType=RANGE \
--provisioned-throughput \
    ReadCapacityUnits=10,WriteCapacityUnits=5
```

**Check Table Status**

```bash
aws --endpoint-url=http://localhost:4566 dynamodb describe-table --table-name Music | grep TableStatus
```

**Insert Data to Table**

```bash
aws --endpoint-url=http://localhost:4566 dynamodb put-item \
    --table-name Music \
    --item \
        '{"Artist": {"S": "No One You Know"}, "SongTitle": {"S": "Call Me Today"}, "AlbumTile": {"S": "Somewhat Famous"}, "Awards": {"N":"1"}}'
```

**Read Table**

```bash
aws dynamodb scan --endpoint-url=http://localhost:4566 --table-name Music
```

**List Tables**

```bash

```