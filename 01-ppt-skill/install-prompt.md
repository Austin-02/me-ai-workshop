# Antigravity 安装 Prompt（V0.1，待实测）

复制以下内容交给 Antigravity：

请安装或更新 aidea-ppt-me，来源 https://github.com/stephenzzf/skill_aidea-ppt-mebrand，main 分支。已审查 commit 为 ef7ba76c015b3e40b5b92102a394f0790ee250ad（v0.3.0）。优先使用该已审查版本；若 main 已更新，先报告差异及版本，不自行切换到未经审查的版本。

1. 先定位当前 Antigravity 实际使用的 Skill 目录和发现机制，确认其支持此 Skill 的格式；不要照搬 Codex 路径。
2. 阅读上游 README.md、SKILL.md、package.json、锁文件及依赖、安装和检查脚本；检查本地是否已有安装及自定义修改。
3. 首次安装在确认目录和权限后获取指定版本，按上游说明安装必要依赖（Git、Node.js >=20.9、Python 3，npm ci 及 Python 虚拟环境/requirements）；额外工具按实际任务需要检查，系统级安装先获得确认。
4. 现有 Git 安装先检查来源、工作区、分支；仅允许符合已审查版本的快进更新；源码同步先比较差异，将自定义内容备份至技能发现目录之外，再经确认同步。不得强制覆盖或重置本地修改。
5. 运行 npm run check（按需要设置 AIDEA_PPT_PYTHON），如实记录退出状态及缺失依赖；验证 Antigravity 发现与调用方式。
6. 报告实际安装路径、版本/commit、依赖、检查结果、Skill 识别情况、重载方法、未解决问题。遇到兼容性或权限问题先停止并说明，不能虚构执行成功。

安装检查不等于 PPT 生成测试；任何新 PPT（含测试稿）须先由学员确认逐页方案。
