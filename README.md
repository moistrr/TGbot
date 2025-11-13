🤖 Telegram 双向转发客服机器人 (Cloudflare Worker)

本项目基于 Cloudflare Worker 和 Key-Value (KV) 存储构建，旨在为 Telegram 机器人提供一套高效的私聊用户与管理员群组话题之间的双向通信中继系统。它具备强大的反垃圾和自动化功能，极大地提升了对话效率。

✨ 核心功能亮点

💬私聊 - 话题双向中继

用户私聊消息转发至管理员群组的独立话题，管理员在话题内回复即可中继给用户。

🏷️话题名动态管理

话题名根据用户昵称和 ID 自动生成并更新，方便识别和查找用户。

🛡️人机验证 (Captcha)

新用户需通过问答验证才能开始使用，有效阻止恶意机器人和垃圾信息。

✏️已编辑消息通知

用户修改消息时，自动在话题中通知管理员修改前后的内容。

❌一键屏蔽与解封

管理员可通过话题卡片上的内联按钮，一键屏蔽/解除屏蔽用户。

🗑️关键词自动屏蔽

可配置关键词列表和阈值，对发送垃圾内容的用户进行自动封禁。

⚙️关键词自动回复

对常见问题设置关键词自动回复，减轻管理员日常压力。

🖼️🔗内容类型精细过滤

精确控制转发的消息类型（如图片、链接、纯文本、频道转发），避免接收不必要的内容。


🛠️ 部署前准备

1. Telegram 设置

创建机器人：通过 @BotFather 创建新的机器人，并获取您的 Bot Token。

创建管理员群组：创建一个超级群组，并开启“话题模式/Forum”。

获取群组 ID：将机器人添加为该群组的管理员。群组 ID 通常以 -100 开头。


2. Cloudflare 环境

   1.1创建 Worker：在 Cloudflare 控制台创建一个新的 Worker 服务。  类型选择第四个 从 Hello World! 开始

   1.2到KV 创建的 KV 命名空间，命名为 TG_BOT_KV

3.环境变量配置 (Worker Variables)

1. 核心与存储配置（必填）

   1.1 变量名称  BOT_TOKEN         变量内容 ： Telegram 机器人 Token。

   1.2 变量名称  ADMIN_GROUP_ID    变量内容：管理员群组 ID (必须是话题模式)。例如：-1001234567890

   1.3 KV 绑定：绑定您创建的 KV 命名空间，命名为 TG_BOT_KV

   1.4 将 worker.js 中的代码复制到您的 Cloudflare Worker 编辑器中。

   1.5 在 Worker 设置中配置好所有的环境变量和 KV 绑定。
 
   1.6 点击 保存并部署。

4.启用机器人和通信验证

  设置 Webhook：使用以下 URL 调用 Telegram API，将您的 Worker URL 绑定到机器人。

  https://api.telegram.org/bot(https://api.telegram.org/bot)<您的BOT_TOKEN>/setWebhook?url=<您的Worker的URL>]

  示例：https://api.telegram.org/bot<YOUR_BOT_TOKEN>/setWebhook?url=<WORKER_URL>

  
🛠️**重要变量信息**

**1. 验证与欢迎信息（可选）**

变量名称：**VERIFICATION_ANSWER**   变量默认值：3   变量说明：人机验证的正确答案。建议将答案写在 Bot 简介中   

变量名称：**VERIFICATION_QUESTION** 变量默认值：    变量说明：发送给新用户的验证问题  

变量名称：**WELCOME_MESSAGE**       变量默认值：    变量说明：用户发送 /start 时收到的欢迎语。


**2. 关键词过滤与反垃圾配置（可选）**

变量名称：**KEYWORD_RESPONSES**    格式：关键词1|关键词2===回复内容\n关键词3===回复内容2    变量说明：自动回复规则

变量名称：**BLOCK_KEYWORDS**       格式：关键词1|关键词2 每一行或使用隔断                   变量说明：自动屏蔽关键词

变量名称：**BLOCK_THRESHOLD**      变量默认值：5      变量说明：关键词触发次数达到此阈值后，用户将被自动屏蔽

**3. 消息类型转发开关（可选）**

变量名称：**ENABLE_IMAGE_FORWARDING**   格式：默认为启用，设置为 false 即可禁用       变量说明：是否转发图片消息

变量名称：**ENABLE_LINK_FORWARDING**    格式：默认为启用，设置为 false 即可禁用       变量说明：是否转发链接消息

变量名称：**ENABLE_TEXT_FORWARDING**    格式：默认为启用，设置为 false 即可禁用       变量说明：是否转发文本消息

变量名称：**ENABLE_CHANNEL_FORWARDING** 格式：默认为启用，设置为 false 即可禁用       变量说明：是否转发频道消息



