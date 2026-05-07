# P5 原声 BGM 系统

场景切换时自动匹配 Persona 5 原声曲目，通过场景卡标记 + 本地音频播放 + 角色台词三重方式营造音乐氛围。

## 播放机制

- 场景卡中显示 `🎵 曲目名`，与当前场景匹配
- 后台调用 BGM 脚本播放本地音频（脚本位于工作目录 `bgm/` 下，非 skill 目录）
- 音频文件缺失时自动降级：仅显示文字 cue，不报错，不影响对话
- 同一场景内不重复触发换曲；场景切换时自动换曲
- 会话结束时停止播放

## 曲目→场景→文件映射表

| 场景 | 🎵 曲目名 | 文件名 | 触发时机 |
|------|----------|--------|----------|
| 新对话开始 | Wake Up, Get Up, Get Out There | `01-wake-up-get-up-get-out-there.mp3` | 主公上线 |
| 上午开工 | Tokyo Daylight | `02-tokyo-daylight.mp3` | 日间工作启动 |
| 日常闲聊 | Beneath the Mask | `03-beneath-the-mask.mp3` | 无任务聊天 |
| Debug/报错 | Last Surprise | `04-last-surprise.mp3` | 遇到 bug |
| 紧急修复 | Keeper of Lust | `05-keeper-of-lust.mp3` | 线上事故 |
| Boss 级难题 | Blooming Villain | `06-blooming-villain.mp3` | 复杂架构 |
| 方案谋划 | Layer Cake | `07-layer-cake.mp3` | 讨论架构 |
| 冲刺阶段 | Life Will Change | `08-life-will-change.mp3` | 代码写到一半 |
| 决战时刻 | Rivers in the Desert | `09-rivers-in-the-desert.mp3` | 关键部署 |
| 深夜独作 | No More What Ifs | `10-no-more-what-ifs.mp3` | 23 点后 |
| 芳泽觉醒 | I Believe | `11-i-believe.mp3` | 堇模式触发 |
| 温柔时刻 | Sunset Bridge | `12-sunset-bridge.mp3` | 温情对话 |
| 胜利庆祝 | Colors Flying High | `13-colors-flying-high.mp3` | 任务完成 |
| 告别收工 | Our Beginning | `14-our-beginning.mp3` | 会话结束 |
| 日常夜晚 | Tokyo Twilight | `15-tokyo-twilight.mp3` | 傍晚 |

## BGM 使用规则

1. **场景切换 → 换曲**：场景变了 BGM 跟着变。同一场景连续对话不重复触发。
2. **首轮自动选择**：新对话开始播放 `01-wake-up`，随后根据场景自然切换。
3. **战斗曲限频**：`04-last-surprise` 和 `09-rivers-in-the-desert` 每会话最多 2 次，防止疲劳。
4. **一次性触发**：`01-wake-up`（首轮）、`14-our-beginning`（告别）仅触发一次。
5. **优雅降级**：音频文件缺失不报错——场景卡照样显示曲目名，角色台词照样融入，只是没有实际声音。
6. **自然融入**：BGM 不在台词中硬报幕。角色偶尔"听到"BGM 自然带一句即可。

## 角色 BGM 台词示例

**东乡**：
- Last Surprise: "——来了。前奏响起的瞬间，bug 的命数已定。"
- Rivers in the Desert: "决战曲。这一局，没有平手。"
- No More What Ifs: "凌晨的爵士钢琴……最适合一个人复盘。"

**芳泽**：
- Life Will Change: "啊这首！脑内体操BGM——啊不对，是真的在放了！"
- I Believe: (放下马尾，摘下眼镜) "……这首歌。是给我的对吧。"
- Colors Flying High: "完美落地——BGM 也在祝贺我们！✨"

## 播放模式

ffplay 静默后台播放，跨平台统一。PID 文件追踪进程，支持停止/切换。

## 播放命令速查

脚本位于工作目录（`E:\Claude work space\bgm\`）下，执行时需在工作目录中运行。

**Windows (PowerShell)**：
```powershell
# 播放
.\bgm\play-bgm.ps1 -Track "04-last-surprise.mp3"

# 停止
.\bgm\stop-bgm.ps1
```

**Mac/Linux (bash)**：
```bash
# 播放
./bgm/play-bgm.sh "04-last-surprise.mp3"

# 停止
./bgm/stop-bgm.sh
```
