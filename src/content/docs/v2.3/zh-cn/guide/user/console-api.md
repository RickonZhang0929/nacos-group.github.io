---
title: Console API 指南
keywords: [Console API,指南]
description: Console API 指南
sidebar:
    order: 8
---

# Console API 指南

## 文档规定

<h2 id="0.1">API 统一返回体格式</h2>

3.0版本Admin API，所有接口请求的响应均为`json`类型的返回体，返回体具有相同的格式

```json
{
  "code": 0,
  "message": "success",
  "data": {}
}
```

返回体中各字段的含义如下表所示

|   名称    |   类型   | 描述                                                   |
| :-------: | :------: | ------------------------------------------------------ |
|  `code `  |  `int`   | 错误码，`0`代表执行成功，非`0`代表执行失败的某一种情况 |
| `message` | `String` | 错误码提示信息，执行成功为"`success`"                  |
|  `data`   | 任意类型 | 返回数据，执行失败时为详细出错信息                     |

> 由于执行成功的情况下code字段与message字段相同，后续在介绍接口的返回结果时，只介绍返回数据的data字段

<h2 id="0.2">API 错误码汇总</h2>

API接口返回体中的错误码及对应提示信息汇总见下表

| 错误码  | 提示信息                     | 含义                       |
| ------- | ---------------------------- | -------------------------- |
| `0`     | `success`                    | 成功执行                   |
| `10000` | `parameter missing`          | 参数缺失                   |
| `10001` | `access denied`              | 访问拒绝                   |
| `10002` | `data access error`          | 数据访问错误               |
| `20001` | `'tenant' parameter error`   | `tenant`参数错误           |
| `20002` | `parameter validate error`   | 参数验证错误               |
| `20003` | `MediaType Error`            | 请求的`MediaType`错误      |
| `20004` | `resource not found`         | 资源未找到                 |
| `20005` | `resource conflict`          | 资源访问冲突               |
| `20006` | `config listener is null`    | 监听配置为空               |
| `20007` | `config listener error`      | 监听配置错误               |
| `20008` | `invalid dataId`             | 无效的`dataId`（鉴权失败） |
| `20009` | `parameter mismatch`         | 请求参数不匹配             |
| `21000` | `service name error`         | `serviceName`服务名错误    |
| `21001` | `weight error`               | `weight`权重参数错误       |
| `21002` | `instance metadata error`    | 实例`metadata`元数据错误   |
| `21003` | `instance not found`         | `instance`实例不存在       |
| `21004` | `instance error`             | `instance`实例信息错误     |
| `21005` | `service metadata error`     | 服务`metadata`元数据错误   |
| `21006` | `selector error`             | 访问策略`selector`错误     |
| `21007` | `service already exist`      | 服务已存在                 |
| `21008` | `service not exist`          | 服务不存在                 |
| `21009` | `service delete failure`     | 存在服务实例，服务删除失败 |
| `21010` | `healthy param miss`         | `healthy`参数缺失          |
| `21011` | `health check still running` | 健康检查仍在运行           |
| `22000` | `illegal namespace`          | 命名空间`namespace`不合法  |
| `22001` | `namespace not exist`        | 命名空间不存在             |
| `22002` | `namespace already exist`    | 命名空间已存在             |
| `23000` | `illegal state`              | 状态`state`不合法          |
| `23001` | `node info error`            | 节点信息错误               |
| `23002` | `node down failure`          | 节点离线操作出错           |
| ...     | ...                          | ...                        |
| 30000   | `server error`               | 其他内部错误               |

## 配置管理

<h2 id="1.1">获取配置详情</h2>

### 接口描述

获取指定的配置信息详情。

### 请求方式

`GET`

### 请求URL

`/v3/console/cs/config`

### 请求参数

| 参数名        | 类型     | 必填   | 参数描述                            |
| ------------- | -------- | ------ | ----------------------------------- |
| `namespaceId` | `String` | 否     | 命名空间，默认为`public`与 `''`相同 |
| `group`       | `String` | **是** | 配置分组名                          |
| `dataId`      | `String` | **是** | 配置名                              |

### 返回数据

| 参数名 | 参数类型        | 描述               |
| ------ | --------------- | ------------------ |
| `data` | `ConfigAllInfo` | 配置的详细信息对象 |

`ConfigAllInfo`对象包含以下字段：

| 字段名             | 类型     | 描述          |
| ------------------ | -------- | ------------- |
| `dataId`           | `String` | 配置名        |
| `group`            | `String` | 配置分组名    |
| `content`          | `String` | 配置内容      |
| `appName`          | `String` | 应用名        |
| `md5`              | `String` | 配置内容的MD5 |
| `type`             | `String` | 配置类型      |
| `tags`             | `String` | 配置标签      |
| `namespaceId`      | `String` | 命名空间ID    |
| `configTags`       | `String` | 配置标签列表  |
| `desc`             | `String` | 配置描述      |
| `schema`           | `String` | 配置的Schema  |
| `lastModifiedTime` | `String` | 上次修改时间  |

### 示例

* 请求示例

  ```shell
  curl -X GET 'http://127.0.0.1:8848/v3/console/cs/config?dataId=nacos.example&group=DEFAULT_GROUP&namespaceId=public'
  ```

* 返回示例

  ```json
  {
    "code": 0,
    "message": "success",
    "data": {
      "dataId": "nacos.example",
      "group": "DEFAULT_GROUP",
      "content": "配置内容",
      "appName": "示例应用",
      "md5": "9f67e6977b100e00cab385a75597db58",
      "type": "yaml",
      "tags": "tag1,tag2",
      "namespaceId": "public",
      "configTags": "tag1,tag2",
      "desc": "配置描述",
      "schema": "",
      "lastModifiedTime": "2023-12-05T01:48:03.380+0000"
    }
  }
  ```

<h2 id="1.2">发布或更新配置</h2>

### 接口描述

添加或更新配置。

### 请求方式

`POST`

`Content-Type:application/x-www-form-urlencoded`

### 请求URL

`/v3/console/cs/config`

### 请求Body

| 参数名        | 类型     | 必填   | 参数描述                             |
| ------------- | -------- | ------ | ------------------------------------ |
| `namespaceId` | `String` | 否     | 命名空间，默认为`public`与 `''`相同  |
| `group`       | `String` | **是** | 配置组名                             |
| `dataId`      | `String` | **是** | 配置名                               |
| `content`     | `String` | **是** | 配置内容                             |
| `type`        | `String` | 否     | 配置类型，例如`yaml`、`properties`等 |
| `appName`     | `String` | 否     | 应用名                               |
| `tag`         | `String` | 否     | 配置标签                             |
| `desc`        | `String` | 否     | 配置描述                             |
| `srcUser`     | `String` | 否     | 源用户                               |
| `configTags`  | `String` | 否     | 配置标签列表，多个标签用逗号分隔     |

### 返回数据

| 参数名 | 参数类型  | 描述         |
| ------ | --------- | ------------ |
| `data` | `boolean` | 是否执行成功 |

### 示例

* 请求示例

  ```shell
  curl -d 'dataId=nacos.example' \
    -d 'group=DEFAULT_GROUP' \
    -d 'namespaceId=public' \
    -d 'content=配置内容' \
    -d 'type=yaml' \
    -X POST 'http://127.0.0.1:8848/v3/console/cs/config'
  ```

* 返回示例

  ```json
  {
    "code": 0,
    "message": "success",
    "data": true
  }
  ```

<h2 id="1.3">删除配置</h2>

### 接口描述

删除指定的配置。

### 请求方式

`DELETE`

### 请求URL

`/v3/console/cs/config`

### 请求参数

| 参数名        | 类型     | 必填   | 参数描述                            |
| ------------- | -------- | ------ | ----------------------------------- |
| `namespaceId` | `String` | 否     | 命名空间，默认为`public`与 `''`相同 |
| `group`       | `String` | **是** | 配置分组名                          |
| `dataId`      | `String` | **是** | 配置名                              |
| `tag`         | `String` | 否     | 配置标签                            |

### 返回数据

| 参数名 | 参数类型  | 描述         |
| ------ | --------- | ------------ |
| `data` | `boolean` | 是否执行成功 |

### 示例

* 请求示例

  ```shell
  curl -X DELETE 'http://127.0.0.1:8848/v3/console/cs/config?dataId=nacos.example&group=DEFAULT_GROUP&namespaceId=public'
  ```

* 返回示例

  ```json
  {
    "code": 0,
    "message": "success",
    "data": true
  }
  ```

<h2 id="1.4">批量删除配置</h2>

### 接口描述

批量删除指定的配置。

### 请求方式

`DELETE`

### 请求URL

`/v3/console/cs/config/batchDelete`

### 请求参数

| 参数名 | 类型     | 必填   | 参数描述   |
| ------ | -------- | ------ | ---------- |
| `ids`  | `Long[]` | **是** | 配置ID列表 |

### 返回数据

| 参数名 | 参数类型  | 描述         |
| ------ | --------- | ------------ |
| `data` | `boolean` | 是否执行成功 |

### 示例

* 请求示例

  ```shell
  curl -X DELETE 'http://127.0.0.1:8848/v3/console/cs/config/batchDelete?ids=1&ids=2&ids=3'
  ```

* 返回示例

  ```json
  {
    "code": 0,
    "message": "success",
    "data": true
  }
  ```

<h2 id="1.5">查询配置列表</h2>

### 接口描述

获取配置列表信息。

### 请求方式

`GET`

### 请求URL

`/v3/console/cs/config/list`

### 请求参数

| 参数名        | 类型     | 必填   | 参数描述                            |
| ------------- | -------- | ------ | ----------------------------------- |
| `namespaceId` | `String` | 否     | 命名空间，默认为`public`与 `''`相同 |
| `group`       | `String` | 否     | 配置分组名                          |
| `dataId`      | `String` | 否     | 配置名                              |
| `config_tags` | `String` | 否     | 配置标签，多个标签用逗号分隔        |
| `appName`     | `String` | 否     | 应用名                              |
| `pageNo`      | `int`    | **是** | 页码，从1开始                       |
| `pageSize`    | `int`    | **是** | 每页显示数量，最大值为500           |

### 返回数据

| 参数名                | 参数类型   | 描述说明                                  |
| --------------------- | ---------- | ----------------------------------------- |
| `data`                | `Object`   | 分页查询结果                              |
| `data.totalCount`     | `int`      | 总数                                      |
| `data.pageNumber`     | `int`      | 当前页                                    |
| `data.pagesAvailable` | `int`      | 总页数                                    |
| `data.pageItems`      | `Object[]` | 配置项列表，参见[配置项信息](#ConfigInfo) |

<h3 id="ConfigInfo">配置项信息</h3>

| 参数名             | 参数类型 | 描述         |
| ------------------ | -------- | ------------ |
| `id`               | `Long`   | 配置ID       |
| `dataId`           | `String` | 配置名       |
| `group`            | `String` | 配置分组     |
| `content`          | `String` | 配置内容     |
| `md5`              | `String` | 配置内容MD5  |
| `type`             | `String` | 配置类型     |
| `appName`          | `String` | 应用名       |
| `desc`             | `String` | 配置描述     |
| `tags`             | `String` | 配置标签     |
| `encryptedDataKey` | `String` | 加密数据密钥 |
| `lastModifiedTime` | `String` | 上次修改时间 |

### 示例

* 请求示例

  ```shell
  curl -X GET 'http://127.0.0.1:8848/v3/console/cs/config/list?pageNo=1&pageSize=10&namespaceId=public'
  ```

* 返回示例

  ```json
  {
    "code": 0,
    "message": "success",
    "data": {
      "totalCount": 1,
      "pageNumber": 1,
      "pagesAvailable": 1,
      "pageItems": [
        {
          "id": 1,
          "dataId": "nacos.example",
          "group": "DEFAULT_GROUP",
          "content": "配置内容",
          "md5": "9f67e6977b100e00cab385a75597db58",
          "type": "yaml",
          "appName": "示例应用",
          "desc": "配置描述",
          "tags": "tag1,tag2",
          "encryptedDataKey": null,
          "lastModifiedTime": "2023-12-05T01:48:03.380+0000"
        }
      ]
    }
  }
  ```

<h2 id="1.6">按配置内容搜索配置列表</h2>

### 接口描述

根据配置内容搜索配置列表。

### 请求方式

`GET`

### 请求URL

`/v3/console/cs/config/searchDetail`

### 请求参数

| 参数名          | 类型     | 必填   | 参数描述                            |
| --------------- | -------- | ------ | ----------------------------------- |
| `namespaceId`   | `String` | 否     | 命名空间，默认为`public`与 `''`相同 |
| `group`         | `String` | 否     | 配置分组名                          |
| `dataId`        | `String` | 否     | 配置名                              |
| `config_tags`   | `String` | 否     | 配置标签，多个标签用逗号分隔        |
| `appName`       | `String` | 否     | 应用名                              |
| `config_detail` | `String` | **是** | 配置内容关键字                      |
| `search`        | `String` | 否     | 搜索类型，默认`blur`                |
| `pageNo`        | `int`    | **是** | 页码，从1开始                       |
| `pageSize`      | `int`    | **是** | 每页显示数量，最大值为500           |

### 返回数据

同[查询配置列表](#1.5-查询配置列表)的返回数据。

### 示例

* 请求示例

  ```shell
  curl -X GET 'http://127.0.0.1:8848/v3/console/cs/config/searchDetail?config_detail=关键字&pageNo=1&pageSize=10&namespaceId=public'
  ```

* 返回示例

  ```json
  {
    "code": 0,
    "message": "success",
    "data": {
      "totalCount": 1,
      "pageNumber": 1,
      "pagesAvailable": 1,
      "pageItems": [
        {
          "id": 1,
          "dataId": "nacos.example",
          "group": "DEFAULT_GROUP",
          "content": "配置内容包含关键字",
          "md5": "9f67e6977b100e00cab385a75597db58",
          "type": "yaml",
          "appName": "示例应用",
          "desc": "配置描述",
          "tags": "tag1,tag2",
          "encryptedDataKey": null,
          "lastModifiedTime": "2023-12-05T01:48:03.380+0000"
        }
      ]
    }
  }
  ```

<h2 id="1.7">查询配置监听客户端信息</h2>

### 接口描述

查询对指定配置订阅的客户端信息。

### 请求方式

`GET`

### 请求URL

`/v3/console/cs/config/listener`

### 请求参数

| 参数名        | 类型     | 必填   | 参数描述                   |
| ------------- | -------- | ------ | -------------------------- |
| `namespaceId` | `String` | 否     | 命名空间ID，默认为`public` |
| `group`       | `String` | **是** | 配置分组名                 |
| `dataId`      | `String` | **是** | 配置名                     |
| `sampleTime`  | `int`    | 否     | 采样时间，默认为`1`        |

### 返回数据

| 参数名 | 参数类型                  | 描述             |
| ------ | ------------------------- | ---------------- |
| `data` | `GroupkeyListenserStatus` | 监听状态信息对象 |

`GroupkeyListenserStatus`对象包含以下字段：

| 字段名                    | 类型                        | 描述           |
| ------------------------- | --------------------------- | -------------- |
| `groupKey`                | `String`                    | 配置的唯一键   |
| `listenersGroupkeyStatus` | `List<Map<String, Object>>` | 监听客户端列表 |

### 示例

* 请求示例

  ```shell
  curl -X GET 'http://127.0.0.1:8848/v3/console/cs/config/listener?dataId=nacos.example&group=DEFAULT_GROUP&namespaceId=public'
  ```

* 返回示例

  ```json
  {
    "code": 0,
    "message": "success",
    "data": {
      "groupKey": "nacos.example+DEFAULT_GROUP+public",
      "listenersGroupkeyStatus": [
        {
          "remoteIp": "127.0.0.1",
          "proc": "app1",
          "port": "8080",
          "appName": "testApp"
        }
      ]
    }
  }
  ```

<h2 id="1.8">查询订阅客户端信息（根据IP）</h2>

### 接口描述

根据客户端IP查询订阅的配置信息。

### 请求方式

`GET`

### 请求URL

`/v3/console/cs/config/listener/ip`

### 请求参数

| 参数名        | 类型      | 必填   | 参数描述                        |
| ------------- | --------- | ------ | ------------------------------- |
| `ip`          | `String`  | **是** | 客户端IP地址                    |
| `all`         | `boolean` | 否     | 是否查询所有配置，默认为`false` |
| `namespaceId` | `String`  | 否     | 命名空间ID，默认为`public`      |
| `sampleTime`  | `int`     | 否     | 采样时间，默认为`1`             |

### 返回数据

同[查询配置监听客户端信息](#1.7-查询配置监听客户端信息)的返回数据。

### 示例

* 请求示例

  ```shell
  curl -X GET 'http://127.0.0.1:8848/v3/console/cs/config/listener/ip?ip=127.0.0.1&namespaceId=public'
  ```

* 返回示例

  ```json
  {
    "code": 0,
    "message": "success",
    "data": {
      "groupKey": "",
      "listenersGroupkeyStatus": [
        {
          "groupKey": "nacos.example+DEFAULT_GROUP+public",
          "remoteIp": "127.0.0.1",
          "proc": "app1",
          "port": "8080",
          "appName": "testApp"
        }
      ]
    }
  }
  ```

<h2 id="1.9">导出配置</h2>

### 接口描述

导出指定的配置。

### 请求方式

`GET`

### 请求URL

`/v3/console/cs/config/export`

### 请求参数

| 参数名        | 类型     | 必填 | 参数描述                   |
| ------------- | -------- | ---- | -------------------------- |
| `namespaceId` | `String` | 否   | 命名空间ID，默认为`public` |
| `group`       | `String` | 否   | 配置分组名                 |
| `dataId`      | `String` | 否   | 配置名                     |
| `appName`     | `String` | 否   | 应用名                     |
| `ids`         | `Long[]` | 否   | 配置ID列表                 |

### 返回数据

以文件形式返回导出的配置数据。

### 示例

* 请求示例

  ```shell
  curl -X GET 'http://127.0.0.1:8848/v3/console/cs/config/export?namespaceId=public'
  ```

* 返回示例

  下载包含配置数据的文件。

<h2 id="1.10">导入配置</h2>

### 接口描述

导入并发布配置。

### 请求方式

`POST`

`Content-Type: multipart/form-data`

### 请求URL

`/v3/console/cs/config/import`

### 请求Body

| 参数名        | 类型               | 必填   | 参数描述                    |
| ------------- | ------------------ | ------ | --------------------------- |
| `namespaceId` | `String`           | 否     | 命名空间ID，默认为`public`  |
| `src_user`    | `String`           | 否     | 源用户                      |
| `policy`      | `SameConfigPolicy` | 否     | 配置冲突策略，默认为`ABORT` |
| `file`        | `MultipartFile`    | **是** | 包含配置数据的文件          |

`SameConfigPolicy`策略可选值：

- `ABORT`：中止操作
- `OVERWRITE`：覆盖已有配置
- `SKIP`：跳过已有配置

### 返回数据

| 参数名 | 参数类型              | 描述     |
| ------ | --------------------- | -------- |
| `data` | `Map<String, Object>` | 导入结果 |

### 示例

* 请求示例

  ```shell
  curl -F 'file=@configs.zip' \
    -F 'namespaceId=public' \
    -X POST 'http://127.0.0.1:8848/v3/console/cs/config/import'
  ```

* 返回示例

  ```json
  {
    "code": 0,
    "message": "success",
    "data": {
      "successCount": 10,
      "skipCount": 0,
      "failCount": 0,
      "failData": []
    }
  }
  ```

<h2 id="1.11">克隆配置</h2>

### 接口描述

克隆配置到指定命名空间。

### 请求方式

`POST`

`Content-Type: application/json`

### 请求URL

`/v3/console/cs/config/clone`

### 请求参数

| 参数名            | 类型                             | 必填   | 参数描述                    |
| ----------------- | -------------------------------- | ------ | --------------------------- |
| `namespaceId`     | `String`                         | **是** | 目标命名空间ID              |
| `src_user`        | `String`                         | 否     | 源用户                      |
| `policy`          | `SameConfigPolicy`               | 否     | 配置冲突策略，默认为`ABORT` |
| `configBeansList` | `SameNamespaceCloneConfigBean[]` | **是** | 要克隆的配置列表            |

`SameNamespaceCloneConfigBean`对象包含以下字段：

| 参数名 | 类型   | 描述   |
| ------ | ------ | ------ |
| `id`   | `Long` | 配置ID |

### 返回数据

| 参数名 | 参数类型              | 描述     |
| ------ | --------------------- | -------- |
| `data` | `Map<String, Object>` | 克隆结果 |

### 示例

* 请求示例

  ```shell
  curl -H "Content-Type: application/json" \
    -d '{"namespaceId":"target_namespace","configBeansList":[{"id":1},{"id":2}]}' \
    -X POST 'http://127.0.0.1:8848/v3/console/cs/config/clone'
  ```

* 返回示例

  ```json
  {
    "code": 0,
    "message": "success",
    "data": {
      "successCount": 2,
      "skipCount": 0,
      "failCount": 0,
      "failData": []
    }
  }
  ```

---

