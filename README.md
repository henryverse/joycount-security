# JoyCount Auto Match — Sign-in Security Review

*Reviewed against JoyCount 3.0.2 · October 2026* · [简体中文](#简体中文)

Auto Match reads the list of games you own on PlayStation, Nintendo Switch and Steam so you can add them to your JoyCount collection. This page explains, in plain terms, what happens when you link an account, what JoyCount stores, and who it talks to.

**In short:** only two parties are ever involved — **you** and **the platform you sign in to**. JoyCount has no server of its own for Auto Match; your sign-in never passes through anyone else.

---

## Who JoyCount talks to

| Platform | Where requests go | Nothing else |
|---|---|---|
| PlayStation | Sony's account and PlayStation Network servers | ✓ |
| Nintendo Switch | Nintendo Account and Nintendo's own servers | ✓ |
| Steam | Steam's Web API and Steam's own image servers | ✓ |

- Every request is made directly from your device to the platform over HTTPS.
- There is no JoyCount account, no JoyCount server, no analytics and no relay in between.
- Game artwork is downloaded from the same platform the game came from, without sending any of your credentials.

## PlayStation and Nintendo: you sign in on the platform's own page

1. When you tap **Sign in**, iOS opens the platform's own sign-in page in a secure system sheet. JoyCount cannot read or type into that page.
2. **JoyCount never sees your password**, your two-step verification codes or your passkeys. You enter them only on Sony's or Nintendo's page.
3. The sheet uses a **private browsing session**: it does not reuse cookies from Safari or a previous sign-in, and nothing is left behind afterwards. This is also why linking a second account never signs you in to the first one by mistake.
4. The sign-in uses the industry-standard authorization flow with **PKCE** (a one-time secret generated on your device) and a random **state** value. JoyCount rejects any reply that does not carry the exact values it generated for that sign-in, so a link from another app or website cannot inject a login.
5. After you sign in, the platform gives JoyCount a token that only lets it read your profile name and your game list. JoyCount does not request permission to buy, message, change settings or access payment details.

> **Please note.** PlayStation and Nintendo do not offer a public library interface for apps like JoyCount. Auto Match uses the same sign-in your platform's own mobile app uses. It works today, but the platform may change or block it at any time. If it stops working, JoyCount shows an error and your collection is not affected.

## Steam: your own key, and a public profile link

Steam offers an official Web API, so no sign-in page is needed.

1. Create a Web API key on Steam's developer page (`steamcommunity.com/dev/apikey`) and paste it into JoyCount.
2. Paste your **Steam profile link** (for example `steamcommunity.com/id/yourname` or `steamcommunity.com/profiles/7656…`). JoyCount turns it into your account number using Steam's own API.
3. Your Steam password is never asked for and never entered anywhere in JoyCount.
4. The key is checked with Steam before it is saved. If Steam rejects it, nothing is stored.

> **Please note.** A Steam Web API key can read any **public** Steam profile, and Steam's API expects the key inside the request address. JoyCount only sends it to Steam over HTTPS, but treat the key like a password: do not share it. Your game list must be set to public in Steam's privacy settings for Auto Match to see it.

## What is stored, and where

| What | Where | Leaves your device? |
|---|---|---|
| PlayStation / Nintendo sign-in tokens | iOS Keychain | **No** |
| Steam Web API key and account number | iOS Keychain | **No** |
| Linked account names | JoyCount's local settings | No |
| Games you choose to import | Your JoyCount collection | Only through your own iCloud sync |

- Credentials are saved with the Keychain setting **"this device only"**: they are not copied to iCloud Keychain, not restored onto another device from a backup, and not shared with JoyCount's widgets.
- Credentials are **never** written into JoyCount's iCloud sync files or Excel backups. Signing in on a new device means signing in again.
- Your password is never stored, because JoyCount never has it.

## Unlinking and revoking access

- **In JoyCount:** open Auto Match → your account → **Unlink**. The stored token or key is deleted from the Keychain immediately. Games already in your collection are kept.
- **On PlayStation:** in your Sony account's security settings, sign out of all devices or change your password. This revokes the token JoyCount holds.
- **On Nintendo:** in your Nintendo Account's sign-in and security settings, sign out of all devices or change your password.
- **On Steam:** revoke the key on the same developer page where you created it. JoyCount stops working with it immediately.

## What this review checked

- The sign-in sheet runs in a private session and is the platform's own page.
- PKCE (S256) and a random `state` value are generated fresh for every sign-in and are verified on return.
- The sign-in reply is checked for the expected app address before anything is accepted; the error description sent by the server is never shown in the app.
- Tokens and keys are saved only to the Keychain with device-only access, and only after the platform has accepted them.
- Unlinking deletes the stored credential.
- No credential is included in sync data, backups, logs or artwork downloads.
- Every Auto Match network address belongs to the platform being used.

## Reporting a problem

If you think you have found a security issue in Auto Match, please open an issue in this repository. Please do not post your tokens, keys or passwords.

---

## 简体中文

*基于 JoyCount 3.0.2 审查 · 2026 年 10 月*

「自动匹配」会读取你在 PlayStation、Nintendo Switch 和 Steam 上拥有的游戏，方便你加进 JoyCount 的收藏。本页用通俗的方式说明：关联账号时发生了什么、JoyCount 保存了什么、它会和谁通信。

**一句话：** 整个过程只涉及两方 —— **你**，以及**你登录的那个游戏平台**。自动匹配没有 JoyCount 自己的服务器，你的登录不会经过任何其他人。

### JoyCount 会和谁通信

- PlayStation：只连接 Sony 的账号与 PlayStation Network 服务器。
- Nintendo Switch：只连接 Nintendo 账号与任天堂自己的服务器。
- Steam：只连接 Steam Web API 和 Steam 自己的图片服务器。
- 所有请求都从你的设备经 HTTPS 直接发往平台；中间没有 JoyCount 账号、服务器、统计分析或中转。
- 游戏封面从该游戏所属的平台下载，下载时不携带任何凭据。

### PlayStation 与 Nintendo：在平台自己的页面上登录

1. 点「登录」后，iOS 会在系统的安全登录面板中打开平台自己的登录页。JoyCount 无法读取或填写这个页面。
2. **JoyCount 永远看不到你的密码**、两步验证码或通行密钥，这些只会输入在 Sony 或任天堂的页面上。
3. 登录面板使用**无痕会话**：不会沿用 Safari 或上一次登录的 Cookie，结束后也不留下任何东西。所以关联第二个账号时，不会误登进第一个账号。
4. 登录采用行业标准的授权流程，带 **PKCE**（在你设备上生成的一次性密钥）和随机 **state**。返回的结果必须与本次登录生成的值完全一致，否则一律拒绝，其他 App 或网页无法伪造登录。
5. 登录后平台给 JoyCount 的令牌只能读取你的昵称和游戏列表。JoyCount 不会申请购买、发消息、改设置或查看支付信息的权限。

> **请注意：** PlayStation 和任天堂没有为 JoyCount 这类 App 提供公开的游戏库接口。自动匹配使用的是平台自家手机 App 的同一套登录方式，目前可用，但平台随时可能更改或停用。一旦失效，JoyCount 会显示错误，你的收藏不受影响。

### Steam：你自己的 Key 和公开的个人资料链接

1. 在 Steam 开发者页面（`steamcommunity.com/dev/apikey`）创建一个 Web API Key，粘贴到 JoyCount。
2. 粘贴你的 **Steam 个人资料链接**（例如 `steamcommunity.com/id/你的名字` 或 `steamcommunity.com/profiles/7656…`）。JoyCount 用 Steam 自己的接口把它换算成账号编号。
3. JoyCount 不会索要、也没有任何地方需要输入你的 Steam 密码。
4. 保存前会先向 Steam 校验 Key；Steam 拒绝时不会保存任何东西。

> **请注意：** Steam Web API Key 可以读取任何**公开**的 Steam 资料，而且 Steam 的接口要求把 Key 放在请求地址里。JoyCount 只会经 HTTPS 把它发给 Steam，但请像对待密码一样保管它，不要分享。另外需要在 Steam 隐私设置里把游戏详情设为公开，自动匹配才能看到。

### 保存了什么、保存在哪

- PlayStation / Nintendo 登录令牌：iOS 钥匙串，**不离开本机**。
- Steam Web API Key 与账号编号：iOS 钥匙串，**不离开本机**。
- 已关联账号的名称：JoyCount 本地设置。
- 你选择导入的游戏：你的 JoyCount 收藏，仅通过你自己的 iCloud 同步。
- 凭据以「**仅限此设备**」方式保存：不进 iCloud 钥匙串，不会通过备份恢复到其他设备，也不与 JoyCount 的小组件共享。
- 凭据**绝不会**写进 JoyCount 的 iCloud 同步文件或 Excel 备份。换新设备需要重新登录。
- JoyCount 从未拿到你的密码，所以也不会保存它。

### 取消关联与撤销授权

- **在 JoyCount 中：** 自动匹配 → 你的账号 → **取消关联**。保存的令牌或 Key 会立即从钥匙串删除，已加入收藏的游戏保留。
- **PlayStation：** 在 Sony 账号的安全设置中退出所有设备或修改密码，即可撤销 JoyCount 持有的令牌。
- **Nintendo：** 在 Nintendo 账号的登录与安全设置中退出所有设备或修改密码。
- **Steam：** 在创建 Key 的同一个开发者页面撤销它，JoyCount 会立刻无法再使用。

### 反馈问题

如果你认为自动匹配存在安全问题，请在本仓库提交 Issue。请不要贴出你的令牌、Key 或密码。
