# MIA pathway gene-family identification skills

个人科研 Skill 的源码、版本记录与迁移说明。用户指定的保存目标：[yz3394/MIA_pathway_genefamily_identification](https://github.com/yz3394/MIA_pathway_genefamily_identification)。当前包含 `genome-family-identification` 1.2.0。

## 内容与状态

```text
MIA_pathway_genefamily_identification/
├── README.md
├── AGENTS.md
├── CHANGELOG.md
├── MAINTENANCE.md
├── .gitignore
└── skills/
    └── genome-family-identification/
        ├── SKILL.md
        ├── agents/openai.yaml
        └── references/                 # 6 个配套参考文件
```

这里保存 Skill 的可迁移源码。当前 Skill 共 8 个文件；1.1.0 补充咖啡 GH1→SGD 的分层筛选，1.2.0 补充手动记录验证的 MDR→CAD/ADH 分支识别及 CDD/KO 证据边界。变化和验证范围见 [CHANGELOG.md](CHANGELOG.md)。

## 保存与版本管理

1. 在本机登录 GitHub，克隆此仓库，集中维护 `skills/` 中的源码。
2. 修改前同步远端并比较现有内容；通过相关验证后提交修改。网页上传也能保存文件，但本机仍需同步网页产生的新提交。
3. 稳定版本使用带 Skill 名称的标签，例如 `genome-family-identification-v1.0.0`；已有标签不移动或覆盖。
4. 验证远端存在对应文件、提交与标签后，再认为这次远程备份完成。每次改进都需要提交并推送，设置远端地址本身不会自动同步文件。

保留源码目录可逐行查看修改；版本标签定位特定提交，Release 可附更新说明并提供该版本源码下载。[GitHub 官方说明](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)

仓库维护可只使用 `main` 和短期修改分支。稳定后合并到 `main`、打标签并安装该版本；已经用于分析的标签不移动、不覆盖。1.0.0 保留为首次发布基线。

## 新电脑恢复

在新环境中登录有权访问目标仓库的 GitHub 账户，然后对 Codex 说：

> 使用 $skill-installer，从 yz3394/MIA_pathway_genefamily_identification 仓库安装 skills/genome-family-identification，版本指定为 genome-family-identification-v1.2.0。安装后检查完整性与 Skill 可见性。

也可以先把仓库克隆到本机，再把完整的 `skills/genome-family-identification/` 目录复制到 `~/.agents/skills/`。该目录是当前官方文档中的用户级发现位置，也支持符号链接。若 Skill 未显示，重启 Codex 后检查。[OpenAI 官方说明](https://learn.chatgpt.com/docs/build-skills)

当前旧电脑的实际文件位于 `~/.codex/skills/genome-family-identification/`，通过 `~/.agents/skills/genome-family-identification` 链接发现。迁移时保存、恢复真实文件，原电脑上的绝对路径链接不能直接用于新电脑。

如果目标位置已经有同名 Skill，先比较版本和本地修改，再备份并更换；安装器不会自动合并已有目录。只维护一个有效安装来源，避免两个不同内容的同名 Skill 同时被发现。

## 更新与回退

- 仓库中的 `skills/` 是集中维护的源码；本机安装目录是运行副本。首次备份与已安装的 1.0.0 内容相同；安装副本的后续更新需要单独完成。
- 修改和验证在源码中进行，发布完成后再更新运行副本。仅修改 GitHub 不会自动刷新已经安装的副本；仅修改本机副本也不会自动提交到 GitHub。
- 更新前保存运行副本和当前标签/提交号；更新后比较完整文件集与哈希，并做一次实际调用检查。
- 若新版出现问题，恢复上一个已验证标签对应的完整 Skill，再运行相关案例；保留新版分析结果及差异记录。
- 重要分析记录 Skill 名称、版本、Git commit，以及实际使用文件的哈希/快照。未提交的临时修改需明确标记。

具体改进流程与复核案例见 [MAINTENANCE.md](MAINTENANCE.md)。

## 分析环境与数据

这个仓库保存工作流及规则。BLAST+、HMMER、MAFFT、建树工具、Pfam/CDD/KO 数据库需要按实际任务配置；Skill 不能替代这些软件和数据库。每次分析保留环境导出、软件/数据库版本、下载来源、参数和输入哈希。有经过实测的环境配置后，再将可迁移的配置纳入对应 Skill，删除机器特定路径。

完整重现旧咖啡分析还需要另外保存原始输入、结果、命令和验证记录。本仓库保留咖啡案例的基因 ID、数量及结论摘要，但未包含原始分析数据。这些方法案例已由用户确认公开保存；今后新增案例时继续核实其披露范围。

其他自己编写的 Skill 可按同样方式加入 `skills/`。预装或第三方 Skill 优先记录来源及版本，保留其许可证；不把整个个人配置目录作为 Skill 源码上传。GitHub 之外可额外保留一次发行版 ZIP，完整 Git 历史可另做仓库镜像备份。[GitHub 备份说明](https://docs.github.com/en/repositories/archiving-a-github-repository/backing-up-a-repository)
