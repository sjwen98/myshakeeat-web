# myshakeeat-web

myShakeEat 的官网 + 法务页 + Android App Links 配置。静态站,部署到 Vercel(仿
myswitchboard-web)。

## 内容

- `index.html` —— 落地页
- `open.html` —— 分享落地页。app 分享的链接就是 `/open.html`,装了 app 的人点了
  直接开 app;没装的落到这页,有去 Google Play 的按钮。
- `privacy.html` / `terms.html` / `data-deletion.html` —— Play 上架要的法务页
- `.well-known/assetlinks.json` —— Android App Links(证明 `myshakeeat.vercel.app`
  归 `com.shakeeat.my_shake_eat` 这个 app)。app 端只接管 `/open.html` 这一个路径
  (见 AndroidManifest 的 `pathPrefix="/open.html"`),所以隐私政策等其它页面照常
  在浏览器打开,不会被 app 劫持。
- `app-ads.txt` / `ads.txt` —— AdMob 认证(publisher pub-4551036589300765)
- `vercel.json` —— 确保 assetlinks.json 以 application/json 提供

## 部署(Vercel)

1. push 到 GitHub(如 `sjwen98/myshakeeat-web`)。
2. 在 Vercel 里 Import 这个 repo,项目名设成 `myshakeeat`,得到域名
   `https://myshakeeat.vercel.app`。
3. 验证:浏览器打开 `https://myshakeeat.vercel.app/.well-known/assetlinks.json`
   应返回 JSON(Content-Type: application/json)。

## App Links 指纹(assetlinks.json 里的 sha256_cert_fingerprints)

- 已放:**debug keystore** 的 SHA-256(本机开发调试用)。
- 上架前还要补两个:
  - **release keystore** 的 SHA-256(你自己建的正式签名)。
  - **Play App Signing** 的 SHA-256(上传到 Play Console 后,在 App integrity 页面拿)。
- 拿到后加进 `sha256_cert_fingerprints` 数组,重新部署即可。

拿指纹的命令:
```
keytool -list -v -alias <alias> -keystore <keystore> | grep SHA256
```
