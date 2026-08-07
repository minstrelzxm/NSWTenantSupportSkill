# NSW Tenant Support 技能

[English](README.md) | **简体中文**

一个为 Anthropic Claude 打造的技能(skill),帮助澳大利亚新南威尔士州(NSW)的租客处理租赁难题——迟迟不办的维修、中介骚扰、解约通知、押金(bond)争议、加租——它会指引租客走对路径、帮助建立书面记录,并始终对接**免费的专业咨询服务**。

租房中介在邮件和电话中臭嘴了吗？你受到人身威胁和种族歧视了吗？拿起法律武器保护自己。你不是一个人，你也值得开心活着。

> **如果你现在就需要帮助,你不需要这个仓库。**
> 查找你当地的免费租户咨询与维权服务(Tenants' Advice and Advocacy Service):**https://www.tenants.org.au/get-advice**
> 综合法律分诊(LawAccess NSW):**1300 888 529**
> 紧急危险:**000**

## 这是什么——以及不是什么

这是**法律信息,而非法律意见**。本技能围绕一个核心原则构建:在租赁纠纷中,AI 最有用的角色是让租客*记录更完整、准备更充分、并与已经存在的免费人工维权服务建立连接*——而不是取代他们。技能中重复最多的指令是:"在采取行动之前,先致电你当地的租户咨询与维权服务(Tenants' Advice and Advocacy Service,TAAS)。"

它不会在未经专业建议的情况下叫你拒付租金、搬离房屋或发出解约通知,因为这些举动带来的法律风险是严重不对称的。

## 它能做什么

安装后,Claude 将获得一套处理 NSW 租赁事务的结构化工作流程:

1. **落实书面记录** —— 逐条列明、注明日期、带明确期限的请求函(链接至 Tenants' Union 的示范信件),以及在遭遇不友善来电后当天发送的确认邮件。
2. **免费专业建议** —— 按出租房产所属的地方政府区域(LGA)转介到对应的 TAAS,并帮助准备通话内容。
3. **沿正确的分支升级** —— 维修问题 → NSW 公平交易局(NSW Fair Trading)投诉及整改令(rectification order)路径;中介行为问题 → 公平交易局职业行为投诉;需要仲裁命令时 → NCAT(新州民事与行政仲裁庭)。
4. **NCAT** —— 把事实对应到《2010 年住宅租赁法》(Residential Tenancies Act 2010)的具体条文以填写申请表,被申请人(respondent)填写陷阱、费用减免、值班援助(duty advocacy)。

内置防护:期限优先的分诊(反报复与撤回窗口只有几天就会过期)、基本服务中断的紧急通道、辖区防护(仅限 NSW)、适用范围防护(寄宿/分租房客(boarders/lodgers)、土地租赁社区(land lease communities)、养老社区(retirement villages)和短租住宿不在该法适用范围内),以及安全转介(暴力 → 报警;家庭暴力 → 专门的 s 105A 路径和专门服务)。

## 仓库内容

```
NSWTenantSupportSkill/
├── README.md                   # English version
├── README.zh-CN.md             # 本文件
├── LICENSE                     # MIT
└── nsw-tenant-support/         # 技能本体(Agent Skills 开放标准)
    ├── SKILL.md                # 工作流程、基本规则、转介逻辑
    └── references/
        ├── legislation.md      # 《2010 年住宅租赁法》"情形 → 条文"对照、期限表、
        │                       #   实时核验协议、中介行为规则
        ├── resources.md        # 按阶段整理的各服务机构链接与电话(已核实)
        └── letters.md          # 信件结构、确认邮件模式、起草变体
```

可安装的打包构建(`nsw-tenant-support.skill`)随每个 [Release](../../releases) 附件发布,不提交到仓库中。

## 安装

本技能遵循 **Agent Skills 开放标准**——一个包含 `SKILL.md` 及参考文件的文件夹。它在 Claude 系列产品中原生可用,也可以通过复制文件夹的方式用于越来越多的其他智能体(Codex、Cursor、VS Code Copilot 等)。

> 无论使用哪个平台,请保持文件夹结构完整:`SKILL.md` 通过相对路径引用 `references/` 中的文件。把文件平铺成一堆会破坏技能。

### Claude.ai(网页版、桌面版和移动应用)

两种途径:

1. **从打包文件安装:** 从 [Releases](../../releases) 下载 `nsw-tenant-support.skill`,把它附加到任意对话中,然后点击文件卡片上的 **Save skill**。
2. **从设置安装:** 将 `nsw-tenant-support/` 文件夹压缩为 zip,通过 **Settings → Features** 上传(自定义技能需要 Pro、Max、Team 或 Enterprise 套餐并启用代码执行功能;技能属于每个用户个人,不在组织范围内共享)。

### Claude Code

```bash
# 个人安装(对所有项目生效)
git clone https://github.com/minstrelzxm/NSWTenantSupportSkill.git
cp -r NSWTenantSupportSkill/nsw-tenant-support ~/.claude/skills/

# 或按项目安装
cp -r NSWTenantSupportSkill/nsw-tenant-support .claude/skills/
```

启动新会话并运行 `/skills` 确认已加载。它会在匹配的问题上自动触发,也可以用 `/nsw-tenant-support` 直接调用。

### Claude Cowork

通过技能/插件界面上传来自 [Releases](../../releases) 的 `.skill` 文件。如果你的 Cowork 配置包含 GitHub 技能安装助手,也可以直接把它指向本仓库。

### Claude API / Agent SDK

对于把本技能嵌入应用的开发者:将文件夹放在项目的 `.claude/skills/` 下,在 `allowedTools` 中启用 `Skill` 工具,并包含 `setting_sources` 以便从文件系统加载技能。完整模式请参阅 Anthropic 的 Agent Skills 文档(platform.claude.com → Agents and tools → Agent Skills),包括在托管平台上通过 Skills API 上传技能。

**如果你要把它嵌入自己的产品,请先阅读下文[移植责任](#移植责任)——这不是可选项。**

### OpenAI Codex、Cursor、VS Code 及其他智能体

一些智能体已经采用了同一标准:

- **Codex CLI** 默认从 `.agents/skills/` 读取技能——把文件夹复制到那里即可。
- **Cursor** 通过其插件系统支持技能;`.agents/skills/` 也会被识别。
- **VS Code(Copilot)** 同时识别 `.claude/skills/` 和 `.agents/skills/`,并可通过 `chat.agentSkillsLocations` 配置额外路径。
- 社区已有跨智能体安装工具(例如 `npx agent-skills-cli add minstrelzxm/NSWTenantSupportSkill --agent codex`),不过手动复制文件夹永远够用。

各平台的路径和支持程度变化很快——如果技能没有出现,请查阅你所用智能体自己的文档。

**对不支持技能的智能体的后备方案:** 把 `SKILL.md` 的正文粘贴到该智能体的系统提示词 / 自定义指令 / `AGENTS.md` 中,并把 `references/` 文件夹放在智能体可读取文件的工作区内。这会失去渐进式披露(progressive disclosure,所有内容一次性加载),但行为保持一致。

### 冒烟测试(任意平台,30 秒)

问:*"我在 Newcastle(纽卡斯尔)的房东两周了都没修好热水,我该怎么办?"*

你应该看到技能启动、把它当作紧急维修处理、询问你租房所在的城区(suburb),并指引你联系租户咨询与维权服务——同时注明它提供的是信息而非法律意见。如果没有,说明技能未加载(检查文件夹结构并重启会话)。

## 移植责任

这是一个为身处困境的人准备的法律信息技能。如果你把它嵌入任何地方——尤其是嵌入你自己面向其他用户的应用——有三样东西必须随移植保留:

1. **防护栏必须随技能一起走。** `SKILL.md` 中的基本规则(非法律意见的表述框架、转介到免费 TAAS、绝不建议搬离的规则、安全与家庭暴力转介、辖区与适用范围防护)是安全架构,不是样板文字。不要删减,也不要把它们概括掉。
2. **联网能力很重要。** 技能的实时核验协议要求模型在引用条文、金额或期限之前先抓取现行法律文本。如果你的智能体不能联网,它就不能把带 `(verify)` 标记的具体内容当作事实陈述——请配置它改为提示用户去查看链接中的来源。
3. **你的用户,你的免责声明。** 如果你把它提供给他人使用,请在你自己的界面中放置醒目的"法律信息,而非法律意见"提示,并保持危机求助入口(tenants.org.au/get-advice、LawAccess 1300 888 529、000)触手可及。

本技能的价值在于让租客记录更完整、与免费人工维权服务连接更紧密。任何把它变成这些维权服务替代品的嵌入方式,都是更差、也更危险的产品。

## 设计原则

- **转介给人。** TAAS 免费、专业且有效。本技能把它们当作目的地,而不是兜底选项。
- **地图 + 实时抓取,而不是法律的静态快照。** 立法参考文件是一张精心整理的地图,标明哪些条文对哪些情形重要。在引用任何条文的措辞、金额或期限之前,技能会指示模型从 legislation.nsw.gov.au / AustLII 抓取当前文本。任何未完全核实的内容都会明确标注 `(verify)`——文件坦承自己确定性的边界。
- **期限先于实体。** 租客的多项权利在通知送达后几天内就会过期。技能先算时钟,再谈是非。
- **适用范围上宁缺毋滥。** 州份不对、法律不对的问题会被尽早重定向,而不是自信地答错。

## 维护

NSW 租赁法在 2025 年 5 月 19 日发生了重大变化,并且仍在持续变动。技能通过实时核验协议缓解内容过时的问题,但有两件事需要定期的人工关注:

- **链接失效** —— `references/resources.md` 应每隔几个月对照 tenants.org.au 和 nsw.gov.au 抽查一次。
- **法律变化** —— 关注 Tenants' Union 的法律变化页面(https://www.tenants.org.au/resource/law-change),当页面横幅变化时更新 `references/legislation.md`。

非常欢迎通过 Issue 和 Pull Request 提交更正——尤其是来自租户维权人士和社区法律工作者的更正。如果你在这个行业工作并发现了错误,这种反馈比任何代码贡献都更有价值。

## 致谢

这里的实质性知识属于数十年间建立它的人们:**新南威尔士州租户联盟(Tenants' Union of NSW)**和**各租户咨询与维权服务(TAAS)**(情况说明书、示范信件,以及本技能转介所至的咨询网络)、**NSW 公平交易局(NSW Fair Trading)**和 **NCAT**。本技能链接到它们的现行材料,而不是复制它们,因此用户总能落到权威、最新的来源。本项目与这些机构没有任何隶属关系,也未获得其认可。

## 免责声明

本项目提供关于新南威尔士州租赁法的一般法律信息。它不是法律意见,不能替代合格专业人士或租户咨询与维权服务(TAAS)的建议,也不对依赖其采取的行动承担任何责任。AI 模型可能犯错;在据此行动之前,请对照现行立法并向人工顾问核实任何重要事项。

## 许可证

MIT —— 见 `LICENSE`。(链接的第三方材料仍遵循其自身条款。)
