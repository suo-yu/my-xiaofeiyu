# 自有构建说明（0.3.0）

这个仓库是 `Mieksido/surfpet` 的一个 fork，用来解决「改完的代码被官方更新覆盖」的问题。
上游是 MIT，fork 保留原作者署名与 LICENSE。

## 和上游的差别（三个提交）

| 提交 | 改了什么 | 落在哪 |
| --- | --- | --- |
| `feat(chat): 按语气自动播动作` | 每句话弹气泡时挑一个动作：宿主（SurfCore）给了 `action` 就用它，没有就按文本关键词猜；猜不出来（长句/平淡）不动。同一动作不连续重复 | `src/main/chat-host.ts`、`src/shared/chat.ts` |
| `feat(tray): 开机自启开关` | 右键菜单和托盘菜单各加一项「开机自启」，勾选状态读真实状态 | `src/main/main.ts` |
| `chore(build): 自有构建 0.3.0` | 版本号自管；5 套皮肤进仓（官方仓库 gitignore 掉了），皮肤右键菜单只留窗口操作 | `package.json`、`skins/` |

配套（不在本仓库）：`~/.dsh/surfcore/pet/` 下的 `start-pet.ps1` / `start-pet.vbs` 是启动器，
开机自启的注册表项指向它；它会在需要时无窗口起 `dsh --profile pet`。

## 构建 / 打包

```powershell
npm install                       # 首次；如果不想下 Electron 二进制，先 $env:ELECTRON_SKIP_BINARY_DOWNLOAD=1
npm run build                     # → dist/
npm run selftest                  # 可选：真跑一遍（需要本机 electron 路径能解析）
npm run pack                      # → artifacts/pet-pack.zip
```

用现成 Electron 跑自检（省得再下 100MB）：

```powershell
& "$env:USERPROFILE\.dsh\surfcore\runtime\electron-v45.0.0\electron.exe" . --no-sandbox --selftest
```

## 装到本机

`package.json` / `dist/` / `skins/` / `assets/` 覆盖到 `~/.dsh/surfcore/pet/app/`，
**保留 `app/data/`**（聊天记录 + Chromium profile）。覆盖前先备份，回滚就是把备份拷回去。

## 跟进上游

```powershell
git fetch upstream            # 先 git remote add upstream https://github.com/Mieksido/surfpet.git
git merge upstream/main       # 有冲突就按三个提交的意图手工合
npm run build && npm run pack
```

## 注意

- **别在 SurfCore 设置里点桌宠的「立即更新」**。那会把 `app/` 换回官方包，自有构建连同这些补丁一起没了。
  当前版本 0.3.0 高于官方 0.2.3，检查更新会显示「已是最新」；等官方发到 0.4+ 时会提示有新版，届时先合并再更新。
- `skins/` 进了 git（15.7MB）。上游新版本如果改了皮肤资源，合并时留意别用官方的覆盖掉我们的菜单精简。
- 历史补丁脚本 `~/.dsh/surfcore/pet/backups/reapply-*.mjs` 是「直接改 dist」时代的产物，自有构建之后它们**已经过时**，不要再用。
