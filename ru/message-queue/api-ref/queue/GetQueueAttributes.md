# GetQueueAttributes

Метод для получения атрибутов указанной очереди.

Если в конце названия очереди стоит суффикс `.fifo` — запрашиваемая очередь имеет [тип FIFO](../../concepts/queue.md#fifo-queues).

## Запрос {#request}

### Параметры запроса {#request-parameters}

Параметр | Тип | Обязательный параметр | Описание
----- | ----- | ----- | -----
`QueueUrl`  | **string** | Да | URL очереди.
`Attributes.N.*` | [список атрибутов](#attributes) | Нет | Список атрибутов очереди.

#### Атрибуты {#attributes}

Атрибуты очереди. Атрибуты передаются в виде списка. Подробнее о передаче списочных параметров см. [Использование API](../index.md#array-parameters).

```
Attribute.N.Name (атрибут)
Attribute.N.Value (значение атрибута)
```

Атрибут | Описание
----- | -----
`All` | Возвращает все атрибуты очереди.
`ApproximateNumberOfMessages` | Возвращает ориентировочное количество сообщений в очереди, которые готовы к получению.
`ApproximateNumberOfMessagesDelayed` | Возвращает ориентировочное количество отложенных сообщений в очереди, которые еще не готовы к приему.
`ApproximateNumberOfMessagesNotVisible` | Возвращает ориентировочное количество сообщений, находящихся в процессе передачи — они уже отправлены получателю, но еще не удалены из очереди или у них не закончился [таймаут видимости](../../concepts/visibility-timeout).
`CreatedTimestamp` | Возвращает таймстемп создания очереди в секундах (epoch time).
`DelaySeconds` | Время в секундах, на которое сообщения будут скрыты после отправки.
`LastModifiedTimestamp` | Возвращает время последнего изменения очереди в секундах (epoch time).
`MaximumMessageSize` | Возвращает максимальный размер сообщения в байтах.
`MessageRetentionPeriod` | Возвращает срок хранения сообщений в секундах.
`ReceiveMessageWaitTimeSeconds` | Возвращает время ожидания для метода [ReceiveMessage](../message/ReceiveMessage) в секундах.
`VisibilityTimeout` | Возвращает [таймаут видимости](../../concepts/visibility-timeout.md) сообщений в очереди в секундах.

#### Атрибуты очередей FIFO {#fifo-path-parameters}

Атрибут | Описание
----- | -----
`FifoQueue` | Флаг, указывающий, что очередь является [FIFO-очередью](../../concepts/queue.md#fifo-queues). Чтобы определить, является ли очередь FIFO-очередью, убедитесь, что в конце названия очереди стоит суффикс `.fifo`.
`ContentBasedDeduplication` | Флаг, указывающий, что в очереди включена [дедупликация по содержимому сообщения](../../concepts/deduplication.md#content-based-deduplication).

#### Неподдерживаемые атрибуты

Атрибут | Тип | Описание
----- | ----- | -----
`RedrivePolicy` | **string** | Атрибут не поддерживается в Yandex Message Queue.
`KmsMasterKeyId` | **string** | Атрибут не поддерживается в Yandex Message Queue.
`KmsDataKeyReusePeriodSeconds` | **string** | Атрибут не поддерживается в Yandex Message Queue.


## Ответ {#response}

### Поля успешного ответа {#response-parameters}

Поле | Тип | Описание
----- | ----- | -----
`Attributes.N.*` | **array** | Массив атрибутов очереди.

### Ошибки GetQueueAttributes {#errors}

Перечень общих для всех методов ошибок смотрите в разделе [[!TITLE]](../common-errors.md).

Код HTTP | Идентификатор ошибки | Описание
----- | ----- | -----
400 | `InvalidAttributeName` | Неверное имя атрибута.

## Пример запроса {#request-example}

```
Action=GetQueueAttributes
&Version=2012-11-05
&QueueUrl=http//sqs.yandex.net:8771/yc-docs/sample-queue-2
&AttributeName.1=All
```

Подробнее о формировании запросов см. в разделе [Общий вид запросов к API](../index.md#api-request).

## Пример ответа {#response-example}

```xml
<GetQueueAttributesResponse>
    <GetQueueAttributesResult>
        <Attribute>
            <Name>ApproximateNumberOfMessages</Name>
            <Value>0</Value>
        </Attribute>
        <Attribute>
            <Name>ApproximateNumberOfMessagesDelayed</Name>
            <Value>0</Value>
        </Attribute>
        <Attribute>
            <Name>ApproximateNumberOfMessagesNotVisible</Name>
            <Value>0</Value>
        </Attribute>
        <Attribute>
            <Name>CreatedTimestamp</Name>
            <Value>1548345183</Value>
        </Attribute>
        <Attribute>
            <Name>DelaySeconds</Name>
            <Value>0</Value>
        </Attribute>
        <Attribute>
            <Name>MaximumMessageSize</Name>
            <Value>262144</Value>
        </Attribute>
        <Attribute>
            <Name>MessageRetentionPeriod</Name>
            <Value>345600</Value>
        </Attribute>
        <Attribute>
            <Name>ReceiveMessageWaitTimeSeconds</Name>
            <Value>0</Value>
        </Attribute>
        <Attribute>
            <Name>VisibilityTimeout</Name>
            <Value>30</Value>
        </Attribute>
    </GetQueueAttributesResult>
    <ResponseMetadata>
        <RequestId>a69769e7-d07122f-147f09c0-e1f9d20-2e4fd5bae005ef4e6d639f7946b42bde</RequestId>
    </ResponseMetadata>
</GetQueueAttributesResponse>
```