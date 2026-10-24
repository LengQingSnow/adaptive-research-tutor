# 轻量进度与恢复格式

## 位置、授权与文件所有者

优先复用项目已有学习资料结构；没有约定时采用 `docs/learning/`。先读取现有内容再合并，不覆盖用户笔记，也不搬移或重建已有课程。

- `progress.md`：当前点、子话题、未解决问题、返回栈、讲解依据、主任务观察位置和恢复下一步。导师维护。
- `knowledge-map.md`：重要知识点、依赖与队列。导师维护；短课程可直接合并进 progress.md，毋须创建空文件。
- `checkpoints.md`：主开发启用集成后的里程碑事实。主开发写、导师读，格式见 [context-and-checkpoints.md](context-and-checkpoints.md)。没有集成不必创建。
- `notes/<concept-id>.md`：按需保存重要讲解或用户笔记，避免复制源码。不是每轮必建。

同一项目若有多个学习会话并发写入，使用项目已有分区；无约定时各自写 `docs/learning/sessions/<session-id>/progress.md` 和同目录知识图谱，共享检查点保持只读。新恢复会话应先认领/确认现有 session ID 的写入者；若旧会话仍在写，使用新 session 目录引用旧记录，不抢写。

用户请求保存、恢复并继续维护记录，或已有项目授权时，在授权范围内增量写入；许可未明则使用会话内记录，继续教学，不擅自建立学习目录。目录可写本身不等于用户允许存储；请求保存也不能越过环境权限。

只写明确由本导师会话管理的区域，可使用 `<!-- adaptive-research-tutor:<session-id>:begin -->` 与对应 `:end` 标记。没有标记的既有用户笔记先保留，采用新小节或独立文件；不整文件覆盖。写入前重新读取，将新记录合并到现有内容；发现同时修改或所有者不清时保留冲突副本/新会话文件并说明，避免覆盖。跨文件保存不假装具有事务能力；采用一致的 `revision` 标识，恢复时发现不一致则先保留疑问并核对，不推定完成。

## 推荐格式

Markdown 说明加一个 YAML 数据块即可；已有格式能无损表达这些信息时不迁移。以下是完整的教学格式示例，并非真实项目事实：

```yaml
schema_version: 1
session_id: sensor-study
revision: 3
project_ref: .
main_task_ref: sensor-gateway
updated_at: "2026-10-23T10:00:00+08:00"
mode: paused
persistence: files
main_observed:
  checkpoint_id: cp-0002
  version: {kind: snapshot, ref: gateway-v2, dirty: unknown}
  scope: ["src/gateway.py::window_mean"]
lesson_basis:
  checkpoint_id: cp-0001
  version: {kind: snapshot, ref: gateway-v1, dirty: unknown}
  note: "先理解旧版分批窗口，再比较滑动窗口；旧版源码未必仍可读"
active:
  concept_id: window-latency
  subtopic: "为何第一个完整输出要等待三条读数"
  awaiting: feedback
  unresolved_questions: ["三条读数是否等于等待三秒？"]
return_stack: []
queue: [mean-noise, window-latency, sliding-window]
learner_model:
  stated: []
  observed: []
next_action: "在 window-latency 用到达时刻 0、1、2 秒说明首个完整窗口；不先跳到滑动窗口"
concepts:
  - id: mean-noise
    name: "算术平均与降噪"
    domains: ["统计", "信号处理"]
    task: "解释传感器读数处理"
    prerequisites: []
    importance: high
    evidence:
      - path: src/gateway.py
        anchor: window_mean
        version: gateway-v1
        kind: checkpoint-report
    status: understood
    open_questions: []
    understanding_evidence: "用户：懂了，继续"
  - id: window-latency
    name: "分批窗口与时延"
    domains: ["流式处理"]
    task: "解释何时产生处理结果"
    prerequisites: [mean-noise]
    importance: high
    evidence:
      - path: src/gateway.py
        anchor: window_mean
        version: gateway-v1
        kind: checkpoint-report
    status: confused
    open_questions: ["三条读数是否等于等待三秒？"]
    understanding_evidence: null
  - id: sliding-window
    name: "滑动窗口与重叠"
    domains: ["流式处理"]
    task: "理解新版本每次读数更新"
    prerequisites: [window-latency]
    importance: medium
    evidence:
      - path: src/gateway.py
        anchor: window_mean
        version: gateway-v2
        kind: checkpoint-report
    status: pending
    open_questions: []
    understanding_evidence: null
```

`mode` 为 active 或 paused；`persistence` 为 files 或 session-only。单点临时答疑无需序列化整套结构。

公开版本的 `learner_model` 默认留空，不包含作者或其他学习者的背景。用户可在教学聊天中自愿填写自己的已掌握知识、希望补充的基础和讲解偏好；`stated` 仅记录当前用户明确提供的信息，`observed` 仅记录当前教学会话中的理解反馈，不从示例或其他人的记录复制。只有获得保存授权后才写入当前项目的学习记录，不把个人资料写回公共 Skill 或仓库。

`concepts` 较长时移动到 knowledge-map.md，progress.md 用 `knowledge_map: knowledge-map.md` 引用并共享 revision。两处若同时记录 active 未解决问题与对应 concept.open_questions，应一起更新；不一致时保留两边问题，不能据缺失推断问题已解决。

`evidence.kind` 区分实际读到的 source、tool-output、document、checkpoint-report、inference、hypothesis。路径只在真实可见或由用户/检查点明确报告时记录；后者标为报告来源，不伪装成已读源码。项目路径优先相对路径，缺失值用 null/unknown，不使用看似真实的占位提交。

## 保存时机与不变量

在用户确认理解、显式跳转、暂停、同步新资料或要求保存时做简短增量；已启用持久化时，重要新卡点也保存。不会每句对话写文件。

- ID 稳定且唯一，队列和前置 ID 能解析。依赖有环或缺失时核对局部关系，不默默丢弃课程；用户要求跳转仍可前进并说明基础缺口。
- `active.concept_id` 必须能解析；缺少时先定位原 ID/锚点，不从第一个点重新开始。
- `understood` 需要可追溯的用户明确确认。跳过时保留原状态和疑问，不能标记完成。新版本知识需要回顾时保留原确认和版本，记录回顾原因。
- 当前讲解依据、最新主任务观察和实际验证范围分别保留；未提交修改不可只用 HEAD 代表。
- 只保存与学习相关的背景、问题和必要证据锚点，不存凭据、不必要个人信息或敏感数据，不整段复制工程文件。
- 写失败时明确本次未落盘，给简短恢复摘要；继续教学和主任务，不反复重试或擅自更改权限。

## 恢复

1. 按用户给定位置或项目约定查找 progress.md；有多个课程时结合项目和 session ID 定位，无法区分时只问选择哪个，不混合用户模型和记录。
2. 读取当前点、子话题、疑问、返回栈、教学依据、队列及必要的关联知识图谱；核对版本和记录一致性。读到文件才能声称“已恢复保存进度”。
3. 比较最近可访问检查点及当前点实现。把变化增量加入队列，不抢占暂停位置。旧内容不可复现时指出缺口，保留概念位置。
4. 用一两句说明“上次停在 B 的什么问题，现在从这里继续”，接着解释 B。不重讲已确认的 A，不要求重新测验。设置 active 课程时不改变知识点理解状态。
5. 只在用户授权的目标维护新进度。资料不可访问时说明无法恢复磁盘记录，可依据用户提供的恢复摘要继续，不能假称已读旧会话。
