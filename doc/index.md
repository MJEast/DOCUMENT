


## 请求IP

http://164.90.147.104:8091
&nbsp;

&nbsp;



### 所有错误码：

| 错误码 | 错误消息                                 | 说明                         |
| ------ | ---------------------------------------- | ---------------------------- |
| 0      | OK 或 SUCCESS                            | 请求成功或者处理完成         |
| 301    | MISSING PARAMETER  <br>INVALID PARAMETER | 缺少参数或者参数错误         |
| 410    | NOT FOUND                                | 请求的对象不存在             |
| 411    | REQUIRES_SPECIFIC_PLAN                   | 需要提供具体的PLAN参数       |
| 420    | XXX NOT FOUND                            | XXX不存在                    |
| 421    | PARENT_INVALID_TYPE                      | 先决条件(基础任务)类型不正确 |
| 430    | INVALID_LOGIN                            | 非法登录                     |
| 501    | GENERAL_ERROR                            | 服务端未知错误               |

&nbsp;

&nbsp;





## Login

成功登录后会返回一个token，发起其他请求时需要将此token附带在header的Authorization字段中。

```jsx
POST /auth/login
{
	"username":"abc",
	"password":"xxxxxxxxxxxxxx"
}
```
登录成功返回示例：
```json
{
    "errno": 0,
    "errmsg": "SUCCESS",
    "data": {
        "id": "64179e-af357c-18960-4c08b2c9-19e4c58e",  // 用户的唯一ID
        "name": null,
        "username": "abc",
        "phone": null,
        "email": "abc@email.com",
        "account_type": "consumer",
        "account_status": "unlimited",
        "last_paid": null,
        "plan_start": null,
        "plan_end": null,
        "last_plan": null,
        // 发起其他请求时需要附带此token在header中:
        "token": "eyJhbGciOiJ...K2Jscp8_k1XwE"
    }
}
```
登录失败返回示例:
```json
{
    "errno": 301,
    "errmsg": "missing username or password"
}
```
&nbsp;

&nbsp;






## Job Create
创建一个文生图任务
```jsx
POST /jobs/create
Header
Authorization: Bearer eyJhbGciOiJ...K2Jscp8_k1XwE
{
    "content":"Shanghai cyberpunk"
}
```

请求成功，返回示例：
``` json
{
  "errno": 0,
  "errmsg": "OK",
  "data": "xxxxxxxxxxxxxxx".    // 创建任务后返回一个hash值，查询此任务结果时会用到这个hash值
}

 ```

请求失败，返回示例：

``` json
未登录：
{
    "errno": 430,
    "error": "invalid login"
}
服务端错误:
{
  "errno": 501,
  "errmsg": "SERVICE_ERROR"
}

 ```
&nbsp;

&nbsp;






## Job Find
根据指定条件查找job
```jsx
POST /jobs/find
Header
Authorization: Bearer eyJhbGciOiJ...K2Jscp8_k1XwE
{

    "limit":10,             // 可选 
	"timestamp_smaller_than":12353533231, // 可选
    "service":["niji"],     // 可选: niji 或者 main
    "job_type":["upscale"], // 可选
	"prompt":"some text",   // 可选
    "folders":["some_folder_id"], // 可选
    "favourite":true, // 可选
}
```
请求成功返回示例:
```json
{
    "errno": 0,
    "errmsg": "SUCCESS",
    "data": {
        "res": [
            {
                "id": 32,
                "id_uuid": "a4fc49fd-c303-4157-819c-be14d2098173",
                "bot_id": null,
                "room_id": null,
                "room_topic": null,
                "sender_id": "79f357ce-a641-4560-b2c9-14c089e8e4c5",
                "sender_name": null,
                "sender_msg": null,
                "msg_date": null,
                "msg_content": "shanghai at dawn",
                "msg_display": "Shanghai at dawn",
                "timestamp": "1712040515164",
                "hash": "89ee3ce8bb4d2a056e38d6bd4a0ab5f2bf5ef258cda6aa49cb2928c99b33349d",   // job 任务的Hash值。查找任务状态时需要用到此Hash
                "status": 2000,
                "error": "altered prompt",
                "job_id": "f052f826-0568-45a1-8797-0a120c92aa42",
                "parent_id": null,
                "parent_hash": null,
                "tmp_msg": null,
                "svg_buffer": "<svg width=\"1024\" height=\"64\">\n\t\t\t  <style>\n\t\t\t  .title { fill: #eeeeee; font-family: Arial sans-serif; font-size: 32px; font-weight: 420;}\n\t\t\t  </style>\n\t\t\t  <text x=\"109\" y=\"45\" text-anchor=\"start\" class=\"title\">@null : 小船AI生成</text>\n\t\t\t</svg>",
                "usr_buffer": null,
                "img_buffer": "https://mjeast.sfo3.digitaloceanspaces.com/image/f052f826-0568-45a1-8797-0a120c92aa42.png",
                "img_format": "png",
                "img_length": 1257418,
                "img_height": 1152,
                "img_width": 1088,
                "img_md5": null,
                "tmp_str": null,
                "grid_num": null,
                "zoom_factor": null,
                "job_type": "main",
                "payment_type": "trial",
                "service": "main",
                "source": "app",
                "client": null,
                "recall_msg": null,
                "recall_prv": null,
                "pct_prog": null,
                "img_prog": null,
                "img_thumb": "https://mjeast.sfo3.digitaloceanspaces.com/image/f052f826-0568-45a1-8797-0a120c92aa42_thumb.png",
                "event_name": null,
                "original_url": "https://mjeast.sfo3.digitaloceanspaces.com/f052f826-0568-45a1-8797-0a120c92aa42_4.png",
                "guild_name": null,
                "guild_id": null,
                "msg_type": null,
                "seed_id": null,
                "image_message_id": null,
                "deleted": false,
                "description": null,
                "description_translation": null,
                "pan_direction": null,
                "command": null,
                "model_source": "internal",
                "created_at": "2024-04-02T06:48:35.165Z",
                "updated_at": "2024-04-02T06:49:30.370Z",
                "is_favourite": false,
                "favourite_count": 0,
                "folders": []
            },{...}
        ],
        "total": 2  // 符合搜索条件的结果总数。此数字应大于或等于当前返回的记录数量（res的记录个数）
    }
}
```
&nbsp;

&nbsp;







## Job Variation
提供一个基础图片，生成更多类似的图片。会返回一个四宫格图片
```js
POST /jobs/variation
Header
Authorization: Bearer eyJhbGciOiJ...K2Jscp8_k1XwE
{
	"sender_id": "on-ZY5H_G6kX2YI219vcnLlbB9AM", // 可选：登录后，发起请求附带token时，非必须
	"parent_id": "f052f826-0568-45a1-8797-0a120c92aa42",    // 基础图片的 job id
	"grid_num": 1   // 基础图片（四宫格中）的编号     左上：1  右上:2  左下：3   右下：4
}
```
&nbsp;

&nbsp;






## Job Upscale
放大一张图片，会返回一张更高分辨率的图
```jsx
POST /jobs/upscale
Header
Authorization: Bearer eyJhbGciOiJ...K2Jscp8_k1XwE
{
    "sender_id":"on-ZY5H_G6kX2YI219vcnLlbB9AM", // 可选：登录后，发起请求附带token时，非必须
    "parent_hash":"89ee3ce8bb4d2a056e38d6bd4a0ab5f2bf5ef258cda6aa49cb2928c99b33349d",	// 基础图片任务的hash值
	"grid_num":2	// 基础图片（四宫格中）的编号     左上：1  右上:2  左下：3   右下：4
}
```
&nbsp;

&nbsp;






## Job Blend
将多张图片合成为一张
```jsx
POST /jobs/blend
Header
Authorization: Bearer eyJhbGciOiJ...K2Jscp8_k1XwE
{
    "sender_id":"on-ZY5H_G6kX2YI219vcnLlbB9AM", // 可选：登录后，发起请求附带token时，非必须
	// 2 - 5 张图片的链接，拼接成一个字符串，用空格分隔多个链接
    "content":"https://mjeast.sfo3.digitaloceanspaces.com/f052f826-0568-45a1-8797aa42-0a120c92.png https://mjeast.sfo3.digitaloceanspaces.com/f052f826-050c9268-45a1-8797-0a12.png https://mjeast.sfo3.digitaloceanspaces.com/f052f826-0568-45a1-8797-0a120c92aa42_4.png"
}
```
请求成功返回结果示例：
```json
{
    "errno": 0,
    "errmsg": "OK",
    "data": "c1d647af4f85b30af69b88b5021c51afafe8ca1c74f120bbf4195687476bb674"	// 此 job blend 任务对应的hash值
}
```
&nbsp;

&nbsp;




## Job Remix
remix模式，可用来更改各变异图形之间的prompts、参数、模型版本、图形比例。通过此接口可以实现诸如修改光线、修改物体之类的效果
```jsx
POST /jobs/variation
Header
Authorization: Bearer eyJhbGciOiJ...K2Jscp8_k1XwE
{
    "sender_id":"on-ZY5H_G6kX2YI219vcnLlbB9AM", // 可选：登录后，发起请求附带token，非必须
    "parent_id":"f052f826-0568-45a1-8797-0a120c92aa42",		// 基础图片的job id
		"grid_num":1,		// 基础图片（四宫格中）的编号     左上：1  右上:2  左下：3   右下：4
		"content":"old prompt rewritten in a new style"		// 用户输入的内容
}
```





## Job Describe
获取图片的prompt
```jsx
POST /jobs/describe
Header
Authorization: Bearer eyJhbGciOiJ...K2Jscp8_k1XwE
{
    "sender_id":"on-ZY5H_G6kX2YI219vcnLlbB9AM", //optional with JWT
    "content":"https://mjeast.sfo3.digitaloceanspaces.com/f052f826-0568-45a1-8797-0a120c92aa42_4.png" // contains LINK to 1 IMAGE
}
```
请求成功时返回示例:
```json
{
    "errno": 0,
    "errmsg": "OK",
    "data": "1ab8e1937cf07a463e2e4b413057cdb7b981420ba61308f08a5b8bc6750c0dd4"	// 此 job describe 任务的 hash值
}
```





