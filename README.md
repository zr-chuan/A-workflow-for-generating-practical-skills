# A-workflow-for-generating-practical-skills
分享一个基于AutoSecill项目的，实现较为完整的从零生成并优化至可实际应用的skills流程
## 目标是实现Agent工作生成skill的一个闭环：从执行轨迹中提炼“技能”（Skill），存入技能库，并在新任务中检索、应用、验证和迭代。
## 一、从零生成skill
对于任意类型的一个Agent,可以部署一个根据其工作流生成基础skill的agent，这里我们选择OpenSpace来完成这个功能。

1、在kali环境安装OpenSpace：

```bash
git clone https://github.com/HKUDS/OpenSpace.git
cd OpenSpace
pip install -e .
openspace-mcp --help   # 验证安装成功
```

2、在你使用的Agent的MCP配置中注册OpenSpace，具体看Agent的要求。

3、复制引导技能到 Agent 的技能目录。

OpenSpace 需要两个引导技能来“教”你的 Agent 何时调用它：

```bash
cp -r OpenSpace/openspace/host_skills/delegate-task/ /path/to/your/agent/skills/
cp -r OpenSpace/openspace/host_skills/skill-discovery/ /path/to/your/agent/skills/
```

delegate-task：让 Agent 知道可以把复杂任务委托给 OpenSpace。

skill-discovery：让 Agent 能从云端搜索和下载社区技能。

4、生成原始skill
写入位置：对于 CAPTURED 生成的全新技能，OpenSpace 会将其直接写入你配置的宿主 Agent 的技能目录

## 二、skill功能评估
1、工具安装
选择agent-insight作为skill评估平台。
一键安装：
```bash
npx agent-insight install
```

2、接入OpenCode

agent-skill支持OpenCode等多种模型。
先在终端打开OpenCode。
在另一个终端输入npx agent-insight start启动agent-skill，获取看板地址，输入浏览器打开看板，登录后点击客户端安装
<img width="2664" height="1280" alt="9a3c917c4b63900901368c7eebb369b6" src="https://github.com/user-attachments/assets/3dddf6fe-f33c-4552-8f6a-a8ce6f6ed598" />
选择 OpenCode，复制它生成的那条 curl 命令，而后在运行 OpenCode 的终端里，粘贴并执行刚才复制的命令。它会引导你完成插件安装和 API Key 配置。
对于功能的配置，根据需求选择即可。

3、导入skill

在skill管理中心处可导入本地skill，同时可以监测管理agent的运行情况。

4、评估

在看板中找到评估的界面，可以对skill的各项指标进行评估，目的适配性，结构规范性，工程健壮性等。

## 三、skill的进化与完善

agent-insight同时兼有完善进化skill的功能。对于导入的skill，可以直接发现问题并ai智能进化。优化后会生成一个候选版本，在一个新网页上显示Diff，而后选择保留或重新优化即可。


## 常见问题
opencode server did not become ready within 30s报错：
<img width="664" height="738" alt="图片" src="https://github.com/user-attachments/assets/bcbf37c4-d120-4806-8d21-e6295c530eb2" />

若无法通过常规方式解决，可直接跳过看板，在OpenCode终端优化：
```bash
opencode run "/si-optimizer 目标文件路径"
```
可实现相同效果

## 总结
至此就完成了一个从skill的生成到优化落地流程的雏形，适用于多种agent，且几乎没有适配性的限制。

[agent-insight详情](https://gitcode.com/openeuler/agent-insight)


## 免责声明
该方法仅作为个人学习交流使用。
