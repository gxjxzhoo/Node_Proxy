# Node_Proxy

把节点链接（vless / vmess / trojan / ss / socks5 / http）用 sing-box 转成本地 `127.0.0.1` 的 http+socks 混合代理，供后续步骤（浏览器、requests、curl）使用。启动后会自检出口 IP，失败直接报错。

节点链接只从调用方的 Secret / env 传入，本仓库不保存任何节点。

## 用法

```yaml
- id: proxy
  uses: gxjxzhoo/Node_Proxy@main
  with:
    node-link: ${{ env.NODE_LINK || secrets.NODE_LINK }}
    sing-box-sha256: ${{ env.SING_BOX_SHA256 || secrets.SING_BOX_SHA256 }}
    # export-proxy-env: "true"   # 需要时把 HTTP(S)_PROXY 导出给后续所有步骤

- run: curl -x "${{ steps.proxy.outputs.proxy-url }}" https://www.cloudflare.com/cdn-cgi/trace
```

## 输出

- `proxy-url`：如 `http://127.0.0.1:12345`（同时写入环境变量 `NODE_PROXY_URL`）；留空节点时为空、直连
- `exit-ip`：脱敏出口 IP
- `mode`：`代理` 或 `直连`
