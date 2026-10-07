# Stillwater · Hyprlock

一个受 GNOME 锁屏启发、适用于 niri 的简洁 Hyprlock 主题。主题名称 stillwater 为 GPT Astra 建议。

## 主题特性

- Adwaita Sans 字体，128 号粗体时钟，24 小时制。
- 英文日期，例如 `Wednesday, October 7`；时间和日期使用系统时区。
- 壁纸模糊并适度暗化，所有文字和输入区域均无阴影。

## 文件

| 文件 | 用途 |
| --- | --- |
| `hyprlock.conf` | 完整主题配置 |
| `niri-bind.kdl` | niri 快捷键示例，需要合并进现有配置 |

```
//hyprlock.conf
background {
    monitor =
    path = $HOME/.config/background // 修改此处为您的锁屏壁纸路径
    //...
}
```

## 环境要求

- Adwaita Sans 字体

可检查程序和字体：

```sh
hyprlock --version
fc-match 'Adwaita Sans'
```

`fc-match` 的结果应包含 `Adwaita Sans`；否则说明使用了替代字体。可以安装该字体，或修改配置顶部的 `$font` 为本机已有的无衬线字体。

## 安装

以下示例使用默认配置目录 `~/.config`。如果自行设置了 `XDG_CONFIG_HOME`，请相应调整安装路径。先进入本仓库根目录。

### 1. 备份并安装主题

```sh
mkdir -p ~/.config/hypr
if [ -f ~/.config/hypr/hyprlock.conf ]; then
    cp -p --backup=numbered ~/.config/hypr/hyprlock.conf ~/.config/hypr/hyprlock.conf.bak
fi
cp hyprlock.conf ~/.config/hypr/hyprlock.conf
```

### 2. 设置壁纸

主题默认读取：

```ini
background {
    path = $HOME/.config/background
    # 其余设置保留原样
}
```

如果该文件已经是你的桌面壁纸，无需操作。否则，在已安装的 `hyprlock.conf` 中找到这条 `path`，将其改为自己的图片路径，例如：

```ini
path = $HOME/Pictures/wallpaper.jpg
```

不要再添加第二个 `background` 块。建议使用浅蓝、海景、天空等色彩干净的壁纸；当前模糊和亮度参数是按海景壁纸调校的。

### 3. 接入 niri （设置锁屏快捷键） 

打开 `~/.config/niri/config.kdl`，在已有的 `binds { ... }` 内加入下面这段；如果已经存在 `Super+Alt+L`，替换原来的绑定：

```kdl
Super+Alt+L hotkey-overlay-title="Lock the Screen: hyprlock" {
    spawn "hyprlock" "--grace" "0";
}
```

`niri-bind.kdl` 提供了完整的示例结构，不能用它覆盖整份 niri 配置。`--grace 0` 表示锁屏后立即要求验证。

检查 niri 配置：

```sh
niri validate
```

niri 会自动加载有效的配置修改。该绑定负责手动锁屏，不会额外设置空闲或休眠自动锁屏。

## 调整外观

编辑已安装的 `~/.config/hypr/hyprlock.conf`：

| 设置 | 当前值 | 作用 |
| --- | --- | --- |
| 时钟 `label` 的 `font_size` | `128` | 时间字号 |
| 时钟 `label` 的 `position` | `0, 140` | 时间位置 |
| 日期 `label` 的 `font_size` | `17` | 日期字号 |
| `$font` | `Adwaita Sans` | 统一字体 |
| 背景 `brightness` | `0.72` | 背景亮度 |
| 背景 `blur_passes` / `blur_size` | `3` / `7` | 模糊参数 |
| 输入区 `dots_size` | `0.30` | 圆点相对于输入区高度的大小 |
| 输入区 `dots_spacing` | `0.30` | 圆点间距 |
| `inputFieldDots` 动画 | `1, 2, gentle` | 圆点输入过渡 |
| `inputFieldColors` 动画 | `1, 1, gentle` | 背景色块快速过渡 |
| `fail_timeout` | `2500` | 失败提示显示时长，单位毫秒 |

居中对齐时，`position` 的正 Y 值向上移动，负值向下移动。字号改变较多时，可以同时调整时间与日期的间距。

动画条目的格式为 `名称, 开关, 时长系数, 曲线`；第三个值越小，过渡越快。背景色块由 `swap_font_color = false` 配合无边框输入区实现；空闲状态仍完全透明。

`fail_timeout = 2500` 表示 **Try again 出现后显示多久**，不是输错密码后等待多久。

## 参考

- [Hyprlock 官方文档](https://wiki.hypr.land/hypr-ecosystem/user/hyprlock/)
- [niri 配置说明](https://niri-wm.github.io/niri/Configuration:-Introduction)
