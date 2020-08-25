# GD Share Translated Eng
**just pull it, please fix it yourself.**
Original source [iwestlin](https://github.com/iwestlin/gdshare)

## demo
[https://drive.google.com/drive/folders/1soZPZdN0beUTvnD8YbRXgOIeff7Dgwa2](https://drive.google.com/drive/folders/1soZPZdN0beUTvnD8YbRXgOIeff7Dgwa2)


## Introduction
This is a project inspired by goindex, suitable for deployment in cloudflare worker, and has the following features compared to the original version:
- Full search (including personal disks and all authorized team disks, you can click the link in the search results to jump to the corresponding google drive official website)
- Paging browsing (the number of files per page can be customized, and each page can be sorted according to file name and size)
- More beautiful UI (thanks to ant design)
- Anti-crawler, for all directories and files, only the administrator has the permission to read and download (the original goindex can set a read password for the directory by preventing .password in the directory, but it cannot restrict the download of a single file)
- It can generate a download link, which is convenient for third-party download tools to download. For streaming media files, you can use players such as potplayer to directly open and play (you can customize the validity period)
- A sharing link with extraction code can be generated to facilitate sharing to others for browsing and downloading (also supports custom validity period)

## changelog
### 2020-08-12
- Display a list of team disks on the homepage (up to 100 entries), click the corresponding name to enter
- Add the function of obtaining direct links in batches (you can choose by yourself, or get the direct links of all direct sub-files in the current directory with one click)
- Add arouse external player function (support IINA/PotPlayer/VLC/nPlayer/MXPlayer)
- Add a search function within a specified range (you can search on the directory list page. Due to Google API limitations, you can only search for direct sub-files of the current directory, and cannot search for the contents of recursive subdirectories. If the current directory is the root directory of the team, then Support whole disk search)
- The "Show Path" button can correctly identify the name of the team drive (it would return "Drive" before)
- Added failure retry mechanism to the back-end search interface

## tips
- This tool can also be used as goindex, just add the directory ID to `https://your.website.com/ls/`.
For example, to browse the content of the team disk, you can directly visit `https://your.website.com/ls/your team disk ID`
Browsing the root directory of the personal disk can directly visit `https://your.website.com/ls/root`

## Construction method
Open [template-eng.js](./template-eng.js) and modify the variables according to the prompts:
```javascript
const CONFIG = {
     PASSKEY: "this is your passkey", // Administrator web login key, please modify it yourself, as complicated as possible
     HASHKEY: "this is your hash key", // It is used to verify the generated download link and sharing link. Please modify it yourself, as complicated as possible. After the modification, the previously generated download and sharing links will be invalid
     RETRY_LIMIT: 5, // Sometimes an error will be reported when calling google drive api to read the directory, here is the maximum number of retries allowed
     PAGESIZE: 100, // The number of single-page objects in the read list, the official limit is 1000
     AUTH: {
         client_id: "insert_your_client_id", // These three items are your google account personal authorization information, the same as goindex
         client_secret: "insert_your_client_secret", // Same as above required
         refresh_token: "insert_your_refresh_token", // Same as above and required
         expires: 0,
         access_token: "" // optional
     }
}
```
After setting the variables, copy the entire `template.js` into the cloudflare worker (for specific steps, please refer to [https://www.jiyiblog.com/archives/031279.html](https://www.jiyiblog.com/ archives/031279.html)), complete.

---
## 介绍
这是一款受goindex启发而产生的项目，适合部署于cloudflare worker，相比原版有以下特性：

- 全盘搜索（包括个人盘和所有有权限的团队盘，可点击搜索结果中的链接跳转到对应的google drive官方网址）
- 分页浏览（可自定义每页文件数，每页可根据文件名和大小排序）
- 更美观的UI（致谢 ant design）
- 防爬虫，对于所有目录和文件，只有管理员才有读取和下载的权限（原版goindex可以通过在目录下防止 .password 来给目录设置读取密码，但无法限制单个文件的下载）
- 可以生成下载直链，方便第三方下载工具下载，对于流媒体文件，可以用potplayer等播放器直接打开进行播放（可以自定义有效期）
- 可以生成带有提取码的分享链接，方便分享给他人浏览和下载（同样支持自定义有效期）

## changelog
### 2020-08-12
- 在首页展示团队盘列表（最多100条），点击对应名称即可进入
- 添加批量获取直链功能（可自行选择，也可一键获取当前目录下所有直接子文件的直链）
- 添加唤起外部播放器功能（支持IINA/PotPlayer/VLC/nPlayer/MXPlayer）
- 添加指定范围搜索功能（可在目录列表页进行搜索，由于Google API的限制，只能搜索当前目录的直接子文件，无法搜索到递归子目录的内容。若当前目录为团队盘根目录，则支持整盘搜索）
- 「显示路径」按钮可正确识别团队盘名称（之前会返回"Drive"）
- 后端搜索接口添加失败重试机制


## tips
- 本工具亦可当作goindex使用，只需将目录ID添加到 `https://your.website.com/ls/` 后即可。
比如浏览团队盘内容可以直接访问 `https://your.website.com/ls/你的团队盘ID`
浏览个人盘根目录可以直接访问 `https://your.website.com/ls/root`

## 搭建方法
打开[template.js](./template.js)，根据提示修改变量：
```javascript
const CONFIG = {
    PASSKEY: "this is your passkey", // 管理员网页登录密钥，请自行修改，尽量复杂
    HASHKEY: "this is your hash key", // 用于校验生成的下载链接和分享链接，请自行修改，尽量复杂。修改后之前生成的下载和分享链接都会失效
    RETRY_LIMIT: 5, // 有时调用 google drive api 读取目录时会报错，这里设置最多允许重试的次数
    PAGESIZE: 100, // 读取列表的单页对象数，官方限制最大 1000
    AUTH: {
        client_id: "insert_your_client_id", // 这三项是你的google帐号个人授权信息，和goindex相同
        client_secret: "insert_your_client_secret", // 同上必填
        refresh_token: "insert_your_refresh_token", // 同上必填
        expires: 0,
        access_token: "" // 可不填
    }
}
```
变量设置完成后，将 `template.js` 整体复制到 cloudflare worker 中（具体步骤可参考[https://www.jiyiblog.com/archives/031279.html](https://www.jiyiblog.com/archives/031279.html)），完成。

## Credit to 
- Iwestlin for this [source](https://github.com/iwestlin/gdshare)
- Google Translated
- And you.