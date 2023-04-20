# Desafio Dio DynamoDB
## Criação de Um Banco de Dados Cardápio de Restaurante
  ### Serviços de DB Usado
  - Amazon DynamoDB
### Comandos para execução do experimento:
- Criar uma Tabela
    ```json
    aws dynamodb create-table \
        --table-name Cardapio \
        --attribute-definitions \
            AttributeName=Tamanho,AttributeType=S \
            AttributeName=Proteina,AttributeType=S \
            AttributeName=Acompanhamento,AttributeType=S \
            AttributeName=Bebidas,AttributeType=S \
        --key-schema \
            AttributeName=Tamanho,KeyType=HASH \
            AttributeName=Proteina,KeyType=HASH \
            AttributeName=Acompanhamento,KeyType=HASH \
            AttributeName=Bebidas,KeyType=HASH \
        --provisioned-throughput \
            ReadCapacityUnits=10,WriteCapacityUnits=5
    ```
- Inserir um item
  
  ```json
  aws dynamodb put-item \
    --table-name Cardapio \
    --item file://itemcardapio.json \
  ```

- Inserir Multiplos Itens
  
  ```json
  aws dynamodb batch-write-item \
    --request-items file://batchcardapio.json
  ```
- Criar um index global secundário baseado na proteína
  
  ```json
  aws dynamodb update-table \
    --table-name Cardapio \
    --attribute-definitions\
        AttributeName=Proteina,AttributeType=S \
        AttributeName=Acompanhamento,AttributeType=S \
    --global-secondary-index-updates \
        "[{\"Create\":{\"IndexName\": \"ProteinaAcompanhamento-index\",\"KeySchema\":[{\"AttributeName\":\"Proteina\",\"KeyType\":\"HASH\"}, {\"AttributeName\":\"Acompanhamento\",\"KeyType\":\"HASH\"}], \
        \"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5      },\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"
  ```
- Criar um index global secundario bseado em Bebidas
  
  