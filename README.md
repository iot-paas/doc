# IoT PaaS

![架构图](https://github.com/iot-paas/doc/blob/master/iot.png)

## Features

- Supports MQTT And HTTP Publish

- Supports HTTP And MQ Publish Message To Device

- Cluster Support, Distributed, High Availability

- Helm Support

- API Documentation

- Device Data Persistence

- Supports User And Client Authentication

- Command API Tool Support

- Data Visualization

## Device Connect Support

- mqtt over tcp
- mqtt over websocket
- http publish

## JWT And Signature Authentication Support

## Topic Auth Support

- publish topic with prefix `productKey/deviceKey/`
- subscribe device topic with prefix `productKey/deviceKey/`
- subscribe broadcast topic with prefix `productKey/$broadcast/`

## DataBackend(RuleEngine) Support

#### Backend Support List

- Mysql, PostgreSQL, ClickHouse, MongoDB
- Nats, RabbitMQ, Kafka
- Republish To Device

#### DB Config Example

```
{
   "type": "mysql",
   "address": "root:paas-iot123@tcp(iot-db-mysql:3306)/db_iot?charset=utf8mb4&parseTime=True&loc=Local"
   "database": "mongo",
   "table_name": "tbl_device",
   "data_mapper": {
      "$productKey": "product_column_name",
      "$deviceKey": "device_column_name",
      "$data.cpu": "cpu_column_name"
   }

}
```

#### MQ Config Example

```kafka
{
   "address": "127.0.0.1:9200",
   "topic": "device.publish",
   "queue": "", //RabbitMQ May Use This Config
   "data_mapper": {
      "$productKey": "product_column_name",
      "$deviceKey": "device_column_name",
      "$data.cpu": "cpu_column_name"
   }
}
```

#### Republish Config Example

```kafka
{
   "topic": "productKey/deviceKey/downlink",
   "data_mapper": {
      "$(productKey)": "product_column_name",
      "$(deviceKey)": "device_column_name",
      "cpu": "cpu_column_name"
   }
}
```

#### Data Mapping Config Support

| Param         | Type   | Description          |
| ------------- | ------ | -------------------- |
| $(productKey) | string | Product Key          |
| $(deviceKey)  | string | Device Key           |
| $(topic)      | string | Device Publish Topic |
| $(node)       | string | Device Connect Node  |
| \*            | string | origin message       |

Json Path uses [**github.com/tidwall/gjson**](https://github.com/tidwall/gjson) for validation. Check the full docs on tags usage [here](https://pkg.go.dev/github.com/tidwall/gjson@v1.7.4).

## BI Support

- Metabase Visualization Support [Detail](https://github.com/metabase/metabase)

## Command API Tool Support

```
   $ cd tools/iotctl && make
   $ ./iotctl user login -u admin -p admin
   $  ./iotctl product info -p 046d1f34f5111ead
┌──────────────────┬─────────────┬──────────────────────────┬─────────────┬─────────────────────┐
│ ProductKey       │ ProductName │ ProductSecret            │ Description │ CreatedAt           │
├──────────────────┼─────────────┼──────────────────────────┼─────────────┼─────────────────────┤
│ 046d1f34f5111ead │ admin2222   │ aafd1c9bb91a0a81c923095a │             │ 1617369767272633700 │
└──────────────────┴─────────────┴──────────────────────────┴─────────────┴─────────────────────┘
```
