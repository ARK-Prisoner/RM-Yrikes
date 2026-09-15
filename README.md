# 攒钻 · 抽卡计算器（Kivy / Android 版）

把原来的 tkinter 版「攒钻 / 抽卡计算器」改写成了 Kivy 应用，可打包成 Android `.apk`。

## 目录结构

```
gacha_kivy/
├── main.py                  # Kivy 应用主程序（含自制日期选择弹窗）
├── buildozer.spec           # APK 打包配置
├── fonts/
│   ├── NotoSansSC-Regular.otf   # 中文正体字体（OFL 开源授权）
│   └── NotoSansSC-Bold.otf      # 中文粗体字体
├── .github/workflows/build.yml  # GitHub Actions 云打包工作流
└── README.md
```

> 说明：Kivy 默认字体不含中文，Android 上会显示成方框，所以打包了一个「思源黑体（Noto Sans SC）」字体（SIL Open Font License，可自由分发）。

## 功能与原版一致

- 输入区：开始日期、最终日期、多久之后（天）、免费抽剩多少、抽几辆、保底多少抽、还剩多少钻石、已经抽了几发。
- 神秘常数区：月卡、月卡月初奖励、签到、每日任务、每周任务、每周任务额外、平均一周活动奖励。
- 结果区：总计新增钻石、换算抽数、到时候共有钻石、一共需要钻石、至少还需要等。
- 开始/结束日期用自制的年/月/日滚轮弹窗选择，并与「多久之后（天）」自动联动。

## 本地运行（Windows，用于快速预览）

```powershell
pip install kivy
python main.py
```

## 打包成 APK（GitHub Actions 云打包，推荐）

buildozer 无法直接在 Windows 上运行，所以采用 GitHub Actions 在线打包。

1. 在 GitHub 新建一个仓库，把 `gacha_kivy` 里的全部内容推到仓库根目录：

   ```powershell
   cd C:\Users\longk\Desktop\task2\gacha_kivy
   git init
   git add .
   git commit -m "kivy gacha calculator"
   git branch -M main
   git remote add origin https://github.com/<你的用户名>/<仓库名>.git
   git push -u origin main
   ```

2. 打开仓库的 **Actions** 页签 → 选择 **Build Android APK** 工作流 → 点击 **Run workflow** 手动触发（或推送一个 `v*` 标签自动触发）。

3. 等构建完成后（首次约 10~20 分钟），在该次运行的 **Artifacts** 里下载 `gacha-calc-apk`，解压得到 `*.apk`，传到手机安装即可。

> 注意：`git push` 需要本机已登录 GitHub 账号（可用 `gh auth login` 或配置 SSH key）。

## 本地打包（可选，需 Linux / WSL2）

在有 Linux 环境的机器上：

```bash
sudo apt update
sudo apt install -y git zip unzip openjdk-17-jdk python3-pip autoconf libtool \
    pkg-config zlib1g-dev libncurses5-dev libncursesw5-dev libtinfo5 cmake libffi-dev libssl-dev
pip3 install --user buildozer cython
buildozer android debug
```

生成的 APK 位于 `bin/` 目录。

## 打包说明

- `buildozer.spec` 中 `requirements = python3,kivy==2.3.0`，无需额外插件，体积更小、兼容性更好。
- 目标架构 `arm64-v8a, armeabi-v7a`，覆盖绝大多数安卓手机。
- 如果构建报错，可先把 `title` 临时改成英文（如 `Gacha Calc`），因为个别 CI 环境对非 ASCII 应用名较敏感。
