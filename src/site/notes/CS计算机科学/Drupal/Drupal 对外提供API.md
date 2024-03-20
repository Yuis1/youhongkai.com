---
{"dg-publish":true,"permalink":"/CS计算机科学/Drupal/Drupal 对外提供API/","created":"2024-03-19T15:09:10.702+08:00","updated":"2024-03-20T23:27:28.912+08:00"}
---


编程找Bug就像做侦探，思维得严丝合缝，不能放过一点蛛丝马迹。

---
## Drupal API对接方式

[JSON:API module | Core modules | Drupal Wiki guide on Drupal.org](https://www.drupal.org/docs/core-modules-and-themes/core-modules/jsonapi-module)

[JSON API模块 与核心的 REST 模块](https://www.drupal.org/docs/core-modules-and-themes/core-modules/jsonapi-module/jsonapi-vs-cores-rest-module)比较

Json API 不支持注册、登录等流程，只支持增删改查。

还是得启用 Restful API来支持。

这里演示了通过Restful进行 注册、登录等流程

[What JSON:API DOESN'T do | JSON:API module | Drupal Wiki guide on Drupal.org](https://www.drupal.org/docs/core-modules-and-themes/core-modules/jsonapi-module/what-jsonapi-doesnt-do)

[Register a User with Drupal 8 REST API | AreaType LLC](https://areatype.com/blog/register-user-drupal-8-rest-api)

## 通过Restful API注册的用户，默认是阻止状态

通过restful api注册的新用户，默认状态是 阻止，导致了无法通过账户激活链接来激活。

尝试了以下方案都无效：

- format=hal_json、 format=json
- 从/session/token 获取 Session Token，在注册接口header中增加 X-CSRF-Token
- 卸载各插件、清空缓存等

这是drupal的bug：[User created via /user/register?_format=json get blocked [#3055807] | Drupal.org](https://www.drupal.org/project/drupal/issues/3055807)

修复方案：通过Rules规则，让新用户立即启用。

- event: After Saving a new User  
- action: unBlock User ，数据选择器填：user

## 隐藏的坑

通过HTTP请求restful api，得到的新用户激活链接也是http。

登录表单流程优化模块，如 [Mail Login | Drupal.org](https://www.drupal.org/project/mail_login)   [Email Registration | Drupal.org](https://www.drupal.org/project/email_registration)  等，一般没有对Restful API调用进行覆写，对API不生效。

## 接口调用示例

以下是配置好的Postman文件，包括了 获取session、注册、登录、登录状态查询、注销、用户详情查询、用户更新、内容列表、内容详情等一系列接口配置，包括了要点和文档链接。

打开Postman导入即可使用。

```json
{
	"info": {
		"_postman_id": "ccab7818-ac4e-4a44-9cd8-7473e4dc8032",
		"name": "示例网站",
		"description": "## 接口文档\n\nrest：[OpenAPI Documentation | 示例网站 (example.com)](https://example.com/admin/config/services/openapi/swagger/rest)\n\njsonapi：[OpenAPI Documentation | 示例网站 (example.com)](https://example.com/admin/config/services/openapi/swagger/jsonapi)\n\n---\n\n## 进阶说明\n\nJSONAPI 文档： [JSON:API module | Core modules | Drupal Wiki guide on Drupal.org](https://www.drupal.org/docs/core-modules-and-themes/core-modules/jsonapi-module)\n\nRestful API 文档：[RESTful Web Services module | Core modules | Drupal Wiki guide on Drupal.org](https://www.drupal.org/docs/8/core/modules/rest)",
		"schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json",
		"_exporter_id": "13674700"
	},
	"item": [
		{
			"name": "00 获取Session Token",
			"request": {
				"method": "GET",
				"header": [],
				"url": {
					"raw": "https://example.com/session/token",
					"protocol": "https",
					"host": [
						"example",
						"com"
					],
					"path": [
						"session",
						"token"
					]
				},
				"description": "when performing **non-read-only requests**, that token is required.  \nSuch a token can be retrieved at `/session/token`."
			},
			"response": []
		},
		{
			"name": "用户注册_hal_json",
			"protocolProfileBehavior": {
				"disabledSystemHeaders": {}
			},
			"request": {
				"auth": {
					"type": "noauth"
				},
				"method": "POST",
				"header": [
					{
						"key": "Content-Type",
						"value": "application/hal+json",
						"type": "text"
					},
					{
						"key": "Accept",
						"value": "application/hal+json",
						"type": "text"
					},
					{
						"key": "X-CSRF-Token",
						"value": "h1oK8A0dx6LVY6IGiKDSMI-29Y8y2HaZcHgxSaSR_Bg",
						"type": "text"
					}
				],
				"body": {
					"mode": "raw",
					"raw": "{\n    \"_links\": {\n        \"type\": {\n            \"href\": \"https://example.com/rest/type/user/user\"\n        }\n    },\n    \"name\": [\n        {\n            \"value\": \"user\"\n        }\n    ],\n    \"mail\": [\n        {\n            \"value\": \"93054555@qq.com\"\n        }\n    ]\n}"
				},
				"url": {
					"raw": "https://example.com/user/register?_format=hal_json",
					"protocol": "https",
					"host": [
						"example",
						"com"
					],
					"path": [
						"user",
						"register"
					],
					"query": [
						{
							"key": "_format",
							"value": "hal_json"
						}
					]
				},
				"description": "### User registration\n\nIn order to allow users to register an account, the user_registration REST resource must be enabled (see the REST UI example below).\n\n``` php\ncurl \\\n--header \"Content-Type: application/json\" \\\n--header \"X-CSRF-Token: 57sTS-KS7UoYAWAPyzt0iJmo300CFct3jdKyWM-UiiQ\" \\\n--request POST \"https://drupal.d9/user/register?_format=json\" \\\n--data '{\"name\": {\"value\": \"thename123\"}, \"pass\": {\"value\": \"thepass\"}, \"mail\": {\"value\": \"someone@example.com\"}}'\n\n ```\n\nUse the session token value with the X-CSRF-Token header. A successful response should contain some user field values, including the UUID of the newly created user:\n\n``` php\n{\n   \"uuid\" : [ { \"value\" : \"3e75b757-831e-4bf7-bbb6-25b8c50c7ac0\" } ]\n}\n\n ```\n\nAlso note that the response doesn't have \"Set-Cookie\" headers, even if visitors are allowed to create an account by themselves. So, if no approval or confirmation required, you can log a user in after successful registration using same name and pass values."
			},
			"response": []
		},
		{
			"name": "用户注册_json",
			"protocolProfileBehavior": {
				"disabledSystemHeaders": {}
			},
			"request": {
				"auth": {
					"type": "noauth"
				},
				"method": "POST",
				"header": [
					{
						"key": "Content-Type",
						"value": "application/json",
						"type": "text"
					},
					{
						"key": "Accept",
						"value": "application/json",
						"type": "text"
					},
					{
						"key": "X-CSRF-Token",
						"value": "7AHNAUDcTLWlAMZp-W-OOZ-zbuwznKbwzDmQNUy3PGw",
						"type": "text",
						"disabled": true
					}
				],
				"body": {
					"mode": "raw",
					"raw": "{\n  \"name\": [{\"value\": \"user1\"}],\n  \"mail\": [{\"value\": \"y.h.k@live.com\"}]\n}\n"
				},
				"url": {
					"raw": "https://example.com/user/register?_format=json",
					"protocol": "https",
					"host": [
						"example",
						"com"
					],
					"path": [
						"user",
						"register"
					],
					"query": [
						{
							"key": "_format",
							"value": "json"
						}
					]
				},
				"description": "### User registration\n\nIn order to allow users to register an account, the user_registration REST resource must be enabled (see the REST UI example below).\n\n``` php\ncurl \\\n--header \"Content-Type: application/json\" \\\n--header \"X-CSRF-Token: 57sTS-KS7UoYAWAPyzt0iJmo300CFct3jdKyWM-UiiQ\" \\\n--request POST \"https://drupal.d9/user/register?_format=json\" \\\n--data '{\"name\": {\"value\": \"thename123\"}, \"pass\": {\"value\": \"thepass\"}, \"mail\": {\"value\": \"someone@example.com\"}}'\n\n ```\n\nUse the session token value with the X-CSRF-Token header. A successful response should contain some user field values, including the UUID of the newly created user:\n\n``` php\n{\n   \"uuid\" : [ { \"value\" : \"3e75b757-831e-4bf7-bbb6-25b8c50c7ac0\" } ]\n}\n\n ```\n\nAlso note that the response doesn't have \"Set-Cookie\" headers, even if visitors are allowed to create an account by themselves. So, if no approval or confirmation required, you can log a user in after successful registration using same name and pass values."
			},
			"response": []
		},
		{
			"name": "用户登录",
			"protocolProfileBehavior": {
				"disabledSystemHeaders": {}
			},
			"request": {
				"auth": {
					"type": "noauth"
				},
				"method": "POST",
				"header": [
					{
						"key": "Content-Type",
						"value": "application/json",
						"type": "text"
					},
					{
						"key": "Accept",
						"value": "application/json",
						"type": "text",
						"disabled": true
					},
					{
						"key": "X-CSRF-Token",
						"value": "KnNanbA7KEd_hEQ09eiom5tUexxDQSZ2aL5Tv31yNts",
						"type": "text",
						"disabled": true
					}
				],
				"body": {
					"mode": "raw",
					"raw": "{\"name\":\"user1\", \"pass\":\"1\"}"
				},
				"url": {
					"raw": "https://example.com/user/login?_format=json",
					"protocol": "https",
					"host": [
						"example",
						"com"
					],
					"path": [
						"user",
						"login"
					],
					"query": [
						{
							"key": "_format",
							"value": "json"
						}
					]
				},
				"description": "### User login\n\n``` php\ncurl \\\n  --header \"Content-type: application/json\" \\\n  -c cookie.txt \\\n  --request POST \"http://drupal.d8/user/login?_format=json\" \\\n  --data '{\"name\":\"admin\", \"pass\":\"admin\"}'\n\n ```\n\nthe `-c cookie.txt` tells curl to save a cookie. Your response should look something like this:\n\n``` php\n{\n   \"csrf_token\" : \"57sTS-KS7UoYAWAPyzt0iJmo300CFct3jdKyWM-UiiQ\",\n   \"logout_token\" : \"zzRaD8ZgLT1TkG804mYpVVTyM-pgoDm4h9XZ9JHSoCw\",\n   \"current_user\" : {\n      \"roles\" : [\n         \"authenticated\",\n         \"administrator\"\n      ],\n      \"name\" : \"admin\",\n      \"uid\" : \"1\"\n   }\n}\n\n ```"
			},
			"response": []
		},
		{
			"name": "用户注销登录",
			"protocolProfileBehavior": {
				"disabledSystemHeaders": {}
			},
			"request": {
				"auth": {
					"type": "noauth"
				},
				"method": "POST",
				"header": [
					{
						"key": "Content-Type",
						"value": "application/json",
						"type": "text"
					},
					{
						"key": "Accept",
						"value": "application/json",
						"type": "text"
					},
					{
						"key": "csrf_token",
						"value": "48BY_Tahw8K6digDa1pSyfIVzJyLSIJzIgkCCl-aAMg",
						"type": "text",
						"disabled": true
					}
				],
				"url": {
					"raw": "https://example.com/user/logout?_format=json&token=AhwpLwXPe0IXZSqbrK9Cd_XzI5X6UkKPIYk8y5PsjOo",
					"protocol": "https",
					"host": [
						"example",
						"com"
					],
					"path": [
						"user",
						"logout"
					],
					"query": [
						{
							"key": "_format",
							"value": "json"
						},
						{
							"key": "token",
							"value": "AhwpLwXPe0IXZSqbrK9Cd_XzI5X6UkKPIYk8y5PsjOo",
							"description": "登录时给的 \"logout_token\""
						}
					]
				},
				"description": "### User logout\n\n``` php\ncurl \\\n  --header \"Content-type: application/json\" \\\n  -b cookie.txt \\\n  --request POST \"http://drupal.d8/user/logout?_format=json&token=zzRaD8ZgLT1TkG804mYpVVTyM-pgoDm4h9XZ9JHSoCw\"\n\n ```\n\nWill log the user authenticated by `cookie.txt` out. Use the logout_token value with the token query parameter.\n\nHeader 中的token，也即是登录时给的 \"logout_token\"\n\n如果没存 \"logout_token\" ，也可以通过清除cookies来注销。"
			},
			"response": []
		},
		{
			"name": "用户登录状态查询",
			"protocolProfileBehavior": {
				"disableBodyPruning": true,
				"disabledSystemHeaders": {}
			},
			"request": {
				"auth": {
					"type": "noauth"
				},
				"method": "GET",
				"header": [
					{
						"key": "Content-Type",
						"value": "application/json",
						"type": "text"
					},
					{
						"key": "Accept",
						"value": "application/json",
						"type": "text"
					},
					{
						"key": "X-CSRF-Token",
						"value": "7AHNAUDcTLWlAMZp-W-OOZ-zbuwznKbwzDmQNUy3PGw",
						"type": "text"
					}
				],
				"body": {
					"mode": "raw",
					"raw": ""
				},
				"url": {
					"raw": "https://example.com/user/login_status?_format=json",
					"protocol": "https",
					"host": [
						"example",
						"com"
					],
					"path": [
						"user",
						"login_status"
					],
					"query": [
						{
							"key": "_format",
							"value": "json"
						}
					]
				},
				"description": "### User status\n\n``` php\ncurl \\\n  --header \"Content-type: application/json\" \\\n  -b cookie.txt \\\n  --request GET \"http://drupal.d8/user/login_status?_format=json\"\n\n ```\n\n`-b cookie.txt` tells curl to _send_ (not save) the cookie from the last request. If you're logged in, this will return `1` in the response body in plain text format (not JSON), otherwise `0`."
			},
			"response": []
		},
		{
			"name": "用户详情查询",
			"protocolProfileBehavior": {
				"disabledSystemHeaders": {
					"accept": true
				}
			},
			"request": {
				"auth": {
					"type": "noauth"
				},
				"method": "GET",
				"header": [
					{
						"key": "Accept",
						"value": "accept: application/json",
						"type": "text"
					}
				],
				"url": {
					"raw": "https://example.com/user/27?_format=json",
					"protocol": "https",
					"host": [
						"example",
						"com"
					],
					"path": [
						"user",
						"27"
					],
					"query": [
						{
							"key": "_format",
							"value": "json"
						}
					]
				},
				"description": "/user/{user}\n\n{user} 是uid。\n\n认证：basic auth ，提供用户名、密码；或者该用户登录之后的cookies认证。\n\n权限控制：有用户管理权限的角色，可以查询所有用户的详情。普通用户，只能查询自己的用户详情。"
			},
			"response": []
		},
		{
			"name": "用户更新",
			"protocolProfileBehavior": {
				"disabledSystemHeaders": {
					"accept": true
				}
			},
			"request": {
				"auth": {
					"type": "noauth"
				},
				"method": "GET",
				"header": [
					{
						"key": "Accept",
						"value": "accept: application/json",
						"type": "text"
					}
				],
				"url": {
					"raw": "https://example.com/user/27?_format=json",
					"protocol": "https",
					"host": [
						"example",
						"com"
					],
					"path": [
						"user",
						"27"
					],
					"query": [
						{
							"key": "_format",
							"value": "json"
						}
					]
				},
				"description": "/user/{user}?_format=json\n\n{user} 是uid。\n\n认证：basic auth ，提供用户名、密码；或者该用户登录之后的cookies认证。 另外，还需要在 header提供 X-CSRF-Token 。值是登录接口提供的 csrf_token。\n\n权限控制：有用户管理权限的角色，可以更新所有用户的所有字段。普通用户，只能更新自己的用户的部分字段（用户名 不允许修改）。"
			},
			"response": []
		},
		{
			"name": "文章列表",
			"protocolProfileBehavior": {
				"disabledSystemHeaders": {
					"accept": true
				}
			},
			"request": {
				"method": "GET",
				"header": [
					{
						"key": "Accept",
						"value": "application/vnd.api+json",
						"type": "text"
					}
				],
				"url": {
					"raw": "https://example.com/jsonapi/node/article",
					"protocol": "https",
					"host": [
						"example",
						"com"
					],
					"path": [
						"jsonapi",
						"node",
						"article"
					]
				},
				"description": "认证：basic auth ，提供用户名、密码；或者该用户登录之后的cookies认证。\n\n权限控制：只有登录用户才有权限查询文章列表。"
			},
			"response": []
		},
		{
			"name": "文章详情查询",
			"request": {
				"method": "GET",
				"header": [],
				"url": {
					"raw": "http://example.com/node?_format=json",
					"protocol": "http",
					"host": [
						"example",
						"com"
					],
					"path": [
						"node"
					],
					"query": [
						{
							"key": "_format",
							"value": "json"
						}
					]
				},
				"description": "/jsonapi/node/article/{entity}\n\n{entity} 是通过列表查到的内容id\n\n认证：basic auth ，提供用户名、密码；或者该用户登录之后的cookies认证。"
			},
			"response": []
		}
	]
}
```