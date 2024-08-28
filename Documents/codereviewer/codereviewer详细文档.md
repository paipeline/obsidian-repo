

流程图: 
![[Pasted image 20240826114629.png]]
### 1. 配置文件

- **描述**: 创建或更新配置文件，包含必要的参数如GitLab API密钥、模型路径、服务端口、数据库连接信息等。
- **任务**: 确保配置文件中所有字段均已正确填写，特别是敏感信息如API密钥应被安全地存储。
ENV 变量
1. API_PATH: gitlab的url加上当前project id
	- project_id可以做在项目主页上的这个地方找到
	- ![[Pasted image 20240826094154.png]]
2. PRIVATE_TOKEN: GitLab API token, 在设置-> Access tokens -> create 里设置.
3. OPENAI_API_KEY: dev端可以用openai api进行测试agent.
4. MODEL_NAME: 大模型全名
5. LITELLM_PROXY_URL: 端口默认为4040, 这个端口可供Load balance API requests
6. REDIS_HOST - REDIS_POST - REDIS_PASSWORD : 设置本地redis
	![[Pasted image 20240826114736.png]]
### 如何更换大模型: 
1. 以防8月31号前没能部署完本地模型, 可以使用litellm内的国内大模型测试agent
2. litellm是一个把市面上的大模型统一以openai的I/O格式输出的框架. 如果需要更换模型, 可根据需要更换 ENV MODEL_NAME = `gpt-4o-mini` 即可.


### 2. GitLab权限

- 设置gitlab API 以及 webhook需要project **owner**的权限
- 测试阶段, 建议拷贝下代码到个人的仓库内, 这样就获得到owner权限
### 3. GitLab Webhook设置
- 在owner权限的前提下, 打开设置 -> webhooks-> 勾选 push event, comment event, Emoji event. 
- URL部分确保URL与本地地址一致以及api端口选为webhook/gitlab. 
- 判断event type [emoji, note hook, push hook]会在listener中实现.
		![[Pasted image 20240826100127.png]]
- 如果需要内网穿透请下载ngrok后输入对应的URL
	- ![[Pasted image 20240826100205.png]]

### 4. 内网穿透(optional)
- **描述**: 如果服务部署在本地或内网中，可能需要使用工具（如ngrok）进行内网穿透，以便外部的GitLab可以访问服务。
- **任务**: 配置内网穿透工具，获取公开可访问的URL，并将其配置到GitLab的webhook设置中。

### 5. Python和环境配置
1. 方案1
	- 安装程序运行所需的Python依赖库.
	- **任务**: 使用`pip install -r requirements.txt`来安装所有必要的依赖库。
2. 方案2
	- 跑dockerfile
	- ![[Pasted image 20240826114917.png]]
#### **如何启动项目:** 
- 内网穿透(optional), 确保port number与主程序对其
1. 将服务器地址输入进gitlab webhook setting
2. 设置好webhook后运行: `uvicorn app.main:app --host 127.0.0.1 --port ·port number here· --reload`  
3. 启动redis
4. 启动mongodb
5. 启动api代理: `litellm --model gpt-4o-mini`
6. 测试webhook
	![[Pasted image 20240828093904.png]]
	如果有response 200说明运行正常, <webhook测试用例因没有与任何项目绑定可能不会触发llm或报错, 属于正常现象>
5. 真实用例测试: push, comment 和 feedback

### 6. API接口

- POST /webhook/gitlab - 触发条件: 当webhook检测到comment, push或 emoji事件被触发后, 会将当前的event通过此接口发送到服务器内做处理 - 详细格式在 event types.md
	![[Pasted image 20240826115012.png]]

| **HTTP Status Code** | **Description**       | **Scenario**                                                                                                   |
| -------------------- | --------------------- | -------------------------------------------------------------------------------------------------------------- |
| 200                  | OK                    | The webhook was received successfully and processed without issues.                                            |
| 400                  | Bad Request           | The request was malformed or missing required data, such as an invalid or missing payload.                     |
| 401                  | Unauthorized          | The webhook does not have the proper authorization, likely due to a missing or invalid token.                  |
| 403                  | Forbidden             | The webhook is not allowed to perform the action, possibly due to insufficient permissions.                    |
| 404                  | Not Found             | The endpoint does not exist, or the specified resource was not found.                                          |
| 500                  | Internal Server Error | An error occurred on the server while processing the webhook, potentially due to a bug or misconfiguration.    |
| 502                  | Bad Gateway           | The server received an invalid response from an upstream server, possibly indicating a network or proxy issue. |
| 503                  | Service Unavailable   | The server is temporarily unable to handle the request, typically due to maintenance or overload.              |
|                      |                       |                                                                                                                |

# GitLab Webhook Listener

## 概述

该模块使用 FastAPI 来处理来自 GitLab 的 Webhook 事件。通过定义不同的 API 路由和事件处理器，系统能够识别和处理多种类型的 GitLab 事件，例如评论、推送和反馈。

## 环境配置

该模块支持两种环境：`development` 和 `production`。不同环境下的日志记录级别不同：

- **development**: 记录 `DEBUG` 级别的日志。
- **production**: 记录 `INFO` 级别的日志。

日志文件保存在 `logs` 目录中，并根据环境不同写入 `debug.log` 或 `info.log` 文件。

## FastAPI 路由

### `/webhook/gitlab` (POST)

处理 GitLab 的 Webhook 事件。

- **请求体**: JSON 格式的 GitLab 事件数据。
- **请求头**: 事件类型通过 `X-Gitlab-Event` 请求头传递。
#### 功能描述

1. **解析请求体**: 从请求中解析 JSON 格式的事件数据。
2. **记录日志**: 将接收到的事件数据和请求头信息记录到日志中。
3. **事件处理**:
    - **Note Hook**: 调用 `handle_comment_event(event)` 处理评论事件。
    - **Push Hook**: 调用 `handle_push_event(event)` 处理推送事件。
    - **Emoji Hook**: 调用 `handle_feedback_event(event)` 处理反馈事件。
4. **错误处理**: 如果事件类型不受支持，则返回 400 状态码，并在日志中记录错误。
	![[Pasted image 20240826115137.png]]
#### 返回值

- **成功响应**: 返回 200 状态码，表示事件处理成功。
    
    json
    
    复制代码
    
    `{     "status": "event processed",     "code": 200,     "message": "Success" }`
    
- **错误响应**: 如果事件类型不支持，返回 400 状态码。
    
    json
    
    复制代码
    
    `{     "status": "error",     "code": 400,     "message": "Unsupported event type: {event_type}" }`
    

## 事件处理函数

处理函数位于 `app.controller.webhook_controller` 模块中：

- **`handle_comment_event(event)`**: 处理评论（Note Hook）事件。
- **`handle_push_event(event)`**: 处理推送（Push Hook）事件。
- **`handle_feedback_event(event)`**: 处理反馈（Emoji Hook）事件。




## `CommentService` 类文档

### 概述

`CommentService` 是一个专门用于处理 GitLab Webhook 事件中与评论（通常称为笔记）相关的类。该类通过获取与事件相关的信息，如提交 ID、文件路径和评论在文件中的位置，然后使用这些信息生成上下文感知的回复，并将其发布回 GitLab。

### 初始化
- **参数**：
    
    - `event`：包含 GitLab Webhook 负载的字典。这包括项目、提交、评论等信息。
- **描述**：
    
    - 构造函数初始化与事件相关的各种属性，如 `project_id`、`commit_head`、`discussion_id`、`note_id` 和 `note_body`。它还提取 `position` 属性，以确定与评论相关的文件路径和行号。
### 方法

#### `_get_metadata()`

- **描述**：
    - 这是一个占位方法，旨在由子类实现。它用于检索与事件相关的元数据。

#### `_get_all_notes()`
- **返回值**：
    
    - 一个包含 `note`、`author_name` 和 `created_at` 的字典列表，每个字典代表一条评论。
- **描述**：
    
    - 该方法获取与事件中指定的文件和行相关的所有内联笔记（评论）。它处理从 GitLab API 获取的讨论，筛选出与指定路径和行匹配的笔记，并按创建时间排序。

#### `_detect_ai_keyword()`
- **返回值**：
    
    - 如果检测到 AI 相关的关键词，返回 `True`，否则返回 `False`。
- **描述**：
    
    - 该方法检查笔记正文是否包含与 AI 相关的关键词。如果检测到，它会将笔记内容分割，分离出 AI 请求与其他评论。

#### `_body_request_splitter()`
 - 从评论中分割出/ai
- **参数**：
    - `notes`：完整的笔记内容。
- **返回值**：
    
    - 包含先前笔记和最后一条笔记的元组。
- **描述**：
    
    - 该方法将笔记正文分割为单独的行，将最后一行（可能包含 AI 请求）与其余评论分开。

#### `get_diffcodeWrapper()`

- **返回值**：
    
    - 从 Redis 缓存或通过 API 获取的差异代码。
- **描述**：
    
    - 该方法检索与事件中指定的提交、文件和行相关的差异代码。它首先尝试从 Redis 缓存中获取差异代码；如果未找到，则从 GitLab API 获取差异代码并将其缓存。

#### `diffcode_formatter()`

- **参数**：
    
    - `diffcode`：原始的差异代码数据。
- **返回值**：
    
    - 格式化的字符串，概述了差异代码。
- **描述**：
    
    - 该方法将获取的差异代码格式化为易于理解的摘要，突出显示文件中的更改。

#### `comments_formatter()`

- **返回值**：
    
    - 格式化的字符串，概述了与指定文件和行相关的最近和历史评论。
- **描述**：
    
    - 该方法将最近和历史评论格式化为摘要，可用于审查更改的上下文和相关讨论。


#### *`get_context()`* ***deprecated***

- **返回值**：
    
    - 包含差异代码和评论摘要的合并字符串。
- **描述**：
    
    - 该方法通过结合差异代码摘要和评论摘要来生成完整的 AI 上下文。此上下文用于生成 AI 回复。



#### `generate_context_aware_response()`
- **参数**：
    
    - `diffcode`：格式化的差异代码。
    - `comments`：格式化的评论。
- **返回值**：
    - AI 生成的回复或生成失败时的错误信息。
- **描述**：
    - 该方法通过调用外部 AI 函数，根据提供的差异代码和评论生成上下文感知的回复。

#### `run()`

- **描述**：
    - `CommentService` 的主执行方法。它检测与 AI 相关的关键词，获取差异代码和评论，使用 AI 生成回复，并将回复发布回 GitLab。


## 使用示例

```python
if __name__ == "__main__":
    # 模拟一个 GitLab Webhook 事件数据
    event_data = {
        "project_id": 12345,
        "object_attributes": {
            "commit_id": "abc123",
            "discussion_id": "disc456",
            "id": 789,
            "note": "/ai 请帮我查看这段代码。",
            "position": {
                "new_path": "src/file.py",
                "old_path": "src/file.py",
                "new_line": 42,
                "old_line": 40,
                "base_sha": "def456",
                "start_sha": "ghi789",
                "head_sha": "jkl012"
            }
        }
    }

    # 初始化 CommentService
    comment_service = CommentService(event_data)

    # 执行 run 方法处理事件
    comment_service.run()

```



# `PushService` 类文档

## 概述

`PushService` 类用于处理来自 GitLab 的 `push` 事件 Webhook。该类会获取提交前后的差异代码，格式化这些差异，并生成上下文感知的 AI 建议，最终将这些建议通过评论的形式发布到 GitLab 中。

## 初始化

- **参数**：
  - `event`: 一个包含 GitLab Webhook 负载的字典，包含 `before` 和 `after` 字段，分别表示推送前后的commit提交哈希值。

- **描述**：
  - 构造函数初始化与 `push` 事件相关的属性，如 `commit_before` 和 `commit_after`，分别对应推送前后的提交哈希值。

## 方法

### `get_diffcodeWrapper()`

- **返回值**：
  - `lst_paths`: 文件路径列表。
  - `lst_diff`: 与这些路径相关的差异代码列表。

- **描述**：
  - 该方法调用差异代码获取函数 `diffcode.run`，通过提交哈希来获取推送操作中涉及的文件路径和相应的差异代码。

### `diffcode_formatter(diffcode, filename)`

- **参数**：
  - `diffcode`: 获取到的原始差异代码数据。
  - `filename`: 文件名或路径。

- **返回值**：
  - 格式化后的差异代码字符串。

- **描述**：
  - 该方法将差异代码格式化为易于理解的字符串，包含文件名和变更的详细信息。

### `generate_context_aware_response(diffcode)`

- **参数**：
  - `diffcode`: 格式化后的差异代码字符串。

- **返回值**：
  - AI 生成的建议或错误消息（如果生成失败）。

- **描述**：
  - 该方法调用外部 AI 函数 `ai_response_push`，根据提供的差异代码生成上下文感知的建议。

### `run()`

```python
def run(self):
    ...
```

- **描述**：
  - `PushService` 类的主执行方法。它首先调用 `get_diffcodeWrapper()` 方法获取差异代码和文件路径，然后调用 `diffcode_formatter()` 格式化差异代码，接着生成 AI 建议，并将这些建议通过 `post_ai_response_to_gitlab` 方法发布到 GitLab。

- **流程**：
  1. 调用 `get_diffcodeWrapper()` 获取差异代码和文件路径列表。
  2. 对于每个文件路径和相应的差异代码，使用 `diffcode_formatter()` 进行格式化。
  3. 调用 `generate_context_aware_response()` 生成 AI 建议。
  4. 使用 `post_ai_response_to_gitlab()` 将建议作为评论发布到 GitLab。

---

## 使用示例

```python
if __name__ == "__main__":
    event_data = {...}  # 示例 GitLab Webhook 事件数据
    push_service = PushService(event_data)
    push_service.run()
```

---



## **测试图例:**

Push后LLM输出测试
![[Pasted image 20240826115825.png]]

 Comment /ai ... 后LLM输出测试: 
 ![[Pasted image 20240826115841.png]]
 