---
title: 如何设计更好的APIs中文版
date: 2022-04-12 17:57:22
tags: [架构]
categories: [架构]
---
# 如何设计更好的APIs

[原文链接](https://r.bluethl.net/how-to-design-better-apis)

api很棒，但它们也非常难设计。在从头创建API时，需要正确地获取许多细节。从基本的安全考虑到使用正确的HTTP方法，实现身份验证，决定应该接受和返回哪些请求和响应，……这样的例子不胜枚举。

在这篇文章中，我尽我最大的努力压缩我所知道的所有关于什么是好的API的东西。一个你的消费者会喜欢使用的API。所有技巧都与语言无关，因此它们适用于任何框架或技术。

## 1. 一致性
我知道这听起来很合理，但这很难做到正确。我所知道的最好的api都是可预测的。当使用者使用并理解一个端点时，他们可以期望另一个端点以同样的方式工作。这对于整个API来说非常重要，也是判断API是否设计良好、是否适合使用的关键指标之一。

* 对字段、资源和参数使用相同的大小写(我更喜欢使用snake_case)
* 使用复数或单数资源名(我更喜欢复数)

    * /users/{id}, /orders/{id} or /user/{id}, /order/{id}

* 对所有端点使用相同的身份验证和授权方法
* 在整个API中使用相同的HTTP头
    * 例如 `API-key`用于传递API键
* 根据响应的类型使用相同的HTTP状态码
    * 例如，当找不到资源时，会出现404
* 对相同类型的操作使用相同的HTTP方法
    * 例如删除资源时使用DELETE

## 2. 使用ISO 8601 UTC日期
在处理日期和时间时，api应该总是返回ISO 8601格式的字符串。在特定时区显示日期通常是客户端应用程序关心的问题。
````json
{
    "published_at": "2022-03-03T21:59:08Z"
}
````


## 3.对公共端点进行例外处理

默认情况下，每个端点都需要授权。大多数端点都需要调用经过身份验证的用户，因此将此设置为缺省值是有意义的。如果需要公开调用某个端点，则显式地将该端点设置为允许未经授权的请求。

## 4. 提供一个健康检查端点
提供一个端点(例如GET健康状况)来确定服务是否健康。这个端点可以被负载均衡器等其他应用程序调用，以便在服务中断时采取行动。

## 5. API的版本
确保对API进行版本控制，并在每次请求时传递该版本，这样消费者就不会受到对另一个版本的任何更改的影响。API版本可以通过HTTP头或querypath参数传递。即使是API的第一个版本(1.0)也应该显式地进行版本控制。

Some examples:

* https://api.averagecompany.com/v1/health
* https://api.averagecompany.com/health?api_version=1.0

## 6. 接受API密钥验证
如果一个API需要由第三方调用，那么允许通过API密钥进行身份验证是有意义的。API键应该使用自定义的HTTP头(例如API - key)传递。它们应该有一个过期日期，并且必须能够撤销活动密钥，以便在它们被破坏时能够失效。避免将API键检查到源代码控制中(改为使用环境变量)。

## 7. 使用合理的HTTP状态码
使用常规的HTTP状态码来指示请求的成功或失败。不要使用太多，在整个API中使用相同的状态码来实现相同的结果。一些例子:

* 200 for general success
* 201 for successful creation
* 400 for bad requests from the client
* 401 for unauthorized requests
* 403 for missing permissions
* 404 for missing resources
* 429 for too many requests
* 5xx for internal errors (these should be avoided at all costs)

## 8. 使用合理的HTTP方法
HTTP方法有很多，但最重要的是:

* POST for creating resources
    * POST /users
* GET for reading resources (both single resources and collections)
    * GET /users
    * GET /users/{id}
* PATCH for applying partial updates to a resource
    * PATCH /users/{id}
* PUT for applying full updates to a resource (replaces the current resource)
    * PUT /users/{id}
* DELETE for deleting resources
    * DELETE /users/{id}

## 9. 使用不言自明、简单的名称
大多数端点都是面向资源的，应该这样命名。不要添加从别处可以推断出来的不必要的信息。这也适用于字段名。

✅ GOOD

* GET /users => Retrieve users
* DELETE /users/{id} => Delete a user
* POST /users/{id}/notifications => Create a notification for a specific user
    * user.first_name
    * order.number

❌ BAD

* GET /getUser
* POST /updateUser
* POST /notification/user
* order.ordernumber
* user.firstName

## 10. 使用标准化错误反应
除了使用HTTP状态码来指示请求的结果(成功或错误)外，在返回错误时，始终使用一个标准化的错误响应，其中包含出错原因的更详细信息。消费者总是可以期待相同的结构，并采取相应的行动。

````json
// Request => GET /users/4TL011ax

// Response <= 404 Not Found
{
    "code": "user/not_found",
    "message": "A user with the ID 4TL011ax could not be found."
}

````

````json
// Request => POST /users
{
    "name": "John Doe"
}

// Response <= 400 Bad Request
{
    "code": "user/email_required",
    "message": "The parameter [email] is required."
}

````

## 11. 在POST时返回创建的资源
使用POST请求创建资源后，最好返回已创建的资源。这是最重要的，因为返回的、创建的资源将反映基础数据源的当前状态，并将包含更近期的信息(例如生成的ID)。

````json
// Request: POST /users
{
    "email": "jdoe@averagecompany.com",
    "name": "John Doe"
}

// Response
{
    "id": "T9hoBuuTL4",
    "email": "jdoe@averagecompany.com",
    "name": "John Doe"
}

````

## 12. 比起PUT，更喜欢PATCH
如前所述，PATCH请求应该对资源应用部分更新，而PUT则完全替换现有资源。围绕PATCH请求设计更新通常是个好主意，因为:

* 当使用PUT仅更新一个资源的字段子集时，仍然需要传递整个资源，这使得它的网络密集性更强，也更容易出错

* 允许任何字段不受任何限制地更新也是相当危险的
* 从我的经验来看，在实践中几乎没有任何用例能够让资源的完整更新变得有意义
* 假设一个订单资源有一个id和一个状态
* 允许消费者更新订单的状态是非常危险的
* 状态更改更有可能由另一个端点触发(如订单{id}履行)

## 13. 尽可能具体
如前一节所述，在设计端点、命名字段和决定接受哪些请求和响应时，最好尽可能具体。如果PATCH请求只接受两个字段(名称和描述)，则不会有错误使用它和损坏数据的危险。

## 14. 使用分页
对所有返回资源集合并使用相同响应结构的请求进行分页。使用page_number和page_size(或类似的)来控制要检索的数据块。

````json
// Response <= 200 OK 
{
  "page_number": 1,
  "page_size": 15,
  "count": 378,
  "data": [
    // resources
  ],
  "total_pages": 26,
  "has_previous_page": true,
  "has_next_page": true
}
````

## 15. 允许使用者使用名为expand(或类似)的查询参数加载相关数据。这对于避免往返和一次性加载特定操作所需的所有数据特别有用。

````json
// Response <= 200 OK
{
  "id": "T9hoBuuTL4",
  "email": "jdoe@averagecompany.com",
  "name": "John Doe",
  "orders": [
    {
      "id": "Hy3SSXU1PF",
      "items": [
        {
          "name": "API course"
        },
        {
          "name": "iPhone 13"
        }
      ]
    },
    {
      "id": "bx1zKmJLI6",
      "items": [
        {
          "name": "SaaS subscription"
        }
      ]
    }
  ]
}
````

