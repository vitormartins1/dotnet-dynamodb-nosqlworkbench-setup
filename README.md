# localstack setup

 Exemplo de projeto dotnet core com setup local do DynamoDB usando localstack e NoSQLWorkbench. 

## Docker Compose

```bash
docker compose up
```

### **docker-compose.yml**

```bash
version: "3.0"

services:
  localstack:
    image: localstack/localstack:latest
    environment:
      - AWS_DEFAULT_REGION=us-east-1
      - EDGE-PORT=4566
      - SERVICES=dynamodb
      - KINESIS_PROVIDER=kinesalite
    ports:
      - "4566:4566"
    volumes:
      - "${TMPDIR:-/tmp/localstack}:/tmp/localstack"
      - "/var/run/docker.sock:/var/run/docker.sock"
```

## DynamoDB

### **Create Table**

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

### **Check Table Status**

```bash
aws --endpoint-url=http://localhost:4566 dynamodb describe-table --table-name Music | grep TableStatus
```

### **Insert Data to Table**

```bash
aws --endpoint-url=http://localhost:4566 dynamodb put-item \
    --table-name Music \
    --item \
        '{"Artist": {"S": "No One You Know"}, "SongTitle": {"S": "Call Me Today"}, "AlbumTile": {"S": "Somewhat Famous"}, "Awards": {"N":"1"}}'
```

### **Read Table**

```bash
aws dynamodb scan --endpoint-url=http://localhost:4566 --table-name Music
```

### **List Tables**

```bash
aws dynamodb list-tables --endpoint-url=http://localhost:4566
```

## NoSQLWorkbench

### **Setup localstack local Connection**

- Operation Builder
- Add Connection
- DynamoDB local
- Connection name = localstack
- Hostname = localhost
- Port = 4566
- Open localstack Connection
- Refresh Table List
- Select table

### **Setup new data model and table**

- Create new data model
- Select Make model from scratch
- Data modeler
- Select data model
- Add table

### **Upload table to localstack DynamoDB**

- Select table
- Visualize data model
- Commit to Amazon DynamoDB
- Use saved connections
- Select localstack Saved connection
- Click commit

## dotnet core local dynamo config

### **AmazonDynamoDBClient**

```csharp
AmazonDynamoDBClient client = new(
    new BasicAWSCredentials("test", "test"), 
    new AmazonDynamoDBConfig
    {
        ServiceURL = "http://localhost:4566",
        UseHttp = true
    });
```
#### Referências

- [Using LocalStack for Development Environments](https://www.maxcode.net/blog/using-localstack-for-development-environments/)
- [5 Ways To Query Data From Amazon DynamoDB using .NET](https://www.rahulpnath.com/blog/dynamodb-querying-dotnet/)