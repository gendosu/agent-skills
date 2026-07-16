# Migration Guide: v2.x to v3.0.0

## 概要 (Overview)

v3.0.0では、リポジトリ構造を`agents/`と`commands/`から統一的な`skills/`アーキテクチャに移行しました。この変更により、[Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)オープン標準に完全準拠し、よりシンプルで一貫性のあるプラグイン構造を実現しています。

This major version migrates the repository structure from separate `agents/` and `commands/` directories to a unified `skills/` architecture. This change ensures full compliance with the [Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) open standard and provides a simpler, more consistent plugin structure.

---

## Breaking Changes

### 1. Repository Structure Changes

**v2.x構造 (v2.x structure)**:
```
agent-skills/
├── .claude-plugin/
│   └── plugin.json          # v2.2.0
├── agents/
│   └── project-manager.md
├── commands/
│   ├── commit.md
│   ├── micro-commit.md
│   ├── pull-request.md
│   ├── todo-task-planning.md
│   └── todo-task-run.md
└── skills/
    ├── key-guidelines/
    └── todo-output-template/
```

**v3.0.0構造 (v3.0.0 structure)**:
```
agent-skills/
├── .claude-plugin/
│   └── plugin.json          # v3.0.0
└── skills/
    ├── commit/
    ├── key-guidelines/
    ├── micro-commit/
    ├── project-manager/
    ├── pull-request/
    ├── todo-output-template/
    ├── todo-task-planning/
    └── todo-task-run/
```

### 2. plugin.json Structure Changes

**v2.x configuration**:
```json
{
  "name": "cccp",
  "version": "2.2.0",
  "description": "Git operations specialist agent and workflow commands...",
  "agents": [
    "./agents/project-manager.md"
  ],
  "commands": [
    "./commands/commit.md",
    "./commands/micro-commit.md",
    "./commands/todo-task-planning.md",
    "./commands/todo-task-run.md",
    "./commands/pull-request.md"
  ],
  "skills": "./skills/"
}
```

**v3.0.0 configuration**:
```json
{
  "name": "cccp",
  "version": "3.0.0",
  "description": "Git operations specialist agent and workflow commands...",
  "skills": "./skills/"
}
```

**変更点 (Changes)**:
- ❌ `agents` array removed
- ❌ `commands` array removed
- ✅ `skills` directory-based auto-discovery retained

### 3. Frontmatter Format Changes

#### Agents → Skills

**v2.x frontmatter (agents/)**:
```yaml
---
name: my-agent
description: >
  Use this agent when you need to perform a specialized task...
model: Haiku
trigger_words:
  - example
  - trigger
---
```

**v3.0.0 frontmatter (skills/)**:
```yaml
---
name: my-agent
description: >
  Use this agent when you need to perform a specialized task...
model: Haiku
trigger_words:
  - example
  - trigger
context: fork
user-invocable: true
---
```

**追加フィールド (Added fields)**:
- `context: fork` - Agent Skillsとして動作するためのフォークコンテキスト指定
- `user-invocable: true` - ユーザーから直接呼び出し可能であることを明示

#### Commands → Skills

**v2.x frontmatter (commands/)**:
```yaml
---
description: git stageされている内容でコミット
mode: run
arguments:
  - name: message
    type: string
    required: false
    description: カスタムコミットメッセージ
---
```

**v3.0.0 frontmatter (skills/)**:
```yaml
---
name: commit
description: git stageされている内容でコミット
user-invocable: true
---
```

**変更点 (Changes)**:
- ✅ `name` field added (required)
- ❌ `mode` field removed (no longer needed)
- ❌ `arguments` array removed from frontmatter
- ✅ `user-invocable: true` added (required)
- ℹ️ Arguments documentation moved to skill body

---

## For Plugin Users

### 🎉 No Action Required

プラグインユーザーは**何もする必要がありません**。以下の点が保証されています:

Plugin users need to take **no action**. The following are guaranteed:

- ✅ **Command invocation syntax unchanged**: `/skill-name` continues to work
- ✅ **All existing skills continue to work**: No functionality lost
- ✅ **Automatic migration**: Plugin handles internal structure changes transparently
- ✅ **Backward compatibility**: Skill names and behaviors remain identical

### What Changed (Transparent to Users)

ユーザーから見えない内部変更:

- Internal file paths (agents/ → skills/, commands/ → skills/)
- Plugin configuration structure
- Version number (v2.2.0 → v3.0.0)

### Example Usage (Unchanged)

**v2.x**:
```
/commit
/todo-task-planning
```

**v3.0.0** (same):
```
/commit
/todo-task-planning
```

---

## For Plugin Developers

プラグインを拡張する開発者向けの移行ガイドです。

This section is for developers who extend the plugin with custom skills.

### Required Actions

1. **Update file path references**
   - パス参照がある場合は更新が必要
   - Name-based references (`/skill-name`) は変更不要

2. **Update frontmatter in custom skills**
   - Add required `name` field
   - Add required `user-invocable: true` field
   - Remove obsolete `mode` field
   - Move `arguments` documentation to body

3. **Remove obsolete fields**
   - `mode` (no longer used)
   - `arguments` array in frontmatter (move to body)

### Migration Procedure

#### Step 1: Create Skills Directory Structure

カスタムスキルがある場合、ディレクトリ構造を変更:

If you have custom skills, create directory structure:

```bash
# Before (v2.x)
agents/my-custom-agent.md

# After (v3.0.0)
skills/my-custom-agent/SKILL.md
```

#### Step 2: Update Frontmatter

**For Agent-type skills**:
```yaml
# v2.x
---
name: my-custom-agent
description: My custom agent description
model: Haiku
trigger_words:
  - custom
  - agent
---

# v3.0.0
---
name: my-custom-agent
description: My custom agent description
model: Haiku
trigger_words:
  - custom
  - agent
context: fork          # ADD THIS
user-invocable: true   # ADD THIS
---
```

**For Command-type skills**:
```yaml
# v2.x
---
description: My custom command description
mode: run
arguments:
  - name: param1
    type: string
    required: true
    description: Parameter description
---

# v3.0.0
---
name: my-custom-command        # ADD THIS
description: My custom command description
user-invocable: true           # ADD THIS
---

# (Move arguments documentation to body)

## Arguments

- `param1` (string, required): Parameter description
```

#### Step 3: Update plugin.json

カスタムプラグインの場合、`plugin.json`を更新:

If you maintain a custom plugin, update `plugin.json`:

```json
// v2.x
{
  "agents": ["./agents/my-agent.md"],
  "commands": ["./commands/my-command.md"],
  "skills": "./skills/"
}

// v3.0.0
{
  "skills": "./skills/"  // Auto-discovery
}
```

#### Step 4: Remove Legacy Directories

レガシーディレクトリを削除:

Remove legacy directories:

```bash
# After migrating all files
rm -rf agents/
rm -rf commands/
```

### Frontmatter Field Reference

#### Required Fields (全スキル共通)

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Skill name (used in `/name`) |
| `description` | string | Skill description |
| `user-invocable` | boolean | Always `true` for user-callable skills |

#### Optional Fields (Agent-type skills)

| Field | Type | Description |
|-------|------|-------------|
| `context` | string | Set to `fork` for agent-type skills |
| `model` | string | Preferred model (e.g., `Haiku`, `sonnet`) |
| `trigger_words` | array | Words that auto-invoke the skill |

#### Deprecated Fields (削除推奨)

| Field | Status | Migration |
|-------|--------|-----------|
| `mode` | ❌ Removed | No longer needed |
| `arguments` | ❌ Removed | Move to skill body documentation |

---

## Impact on Existing Skills

### All 8 Skills Migrated Successfully

| Skill Name | Type | Migration Path | Status |
|------------|------|----------------|--------|
| project-manager | Agent | `agents/` → `skills/project-manager/` | ✅ |
| commit | Command | `commands/` → `skills/commit/` | ✅ |
| micro-commit | Command | `commands/` → `skills/micro-commit/` | ✅ |
| pull-request | Command | `commands/` → `skills/pull-request/` | ✅ |
| todo-task-planning | Command | `commands/` → `skills/todo-task-planning/` | ✅ |
| todo-task-run | Command | `commands/` → `skills/todo-task-run/` | ✅ |
| key-guidelines | Template | `skills/` → `skills/key-guidelines/` | ✅ |
| todo-output-template | Template | `skills/` → `skills/todo-output-template/` | ✅ |

### Functionality Preserved

- ✅ **Trigger words**: Agent auto-invocation works (project-manager)
- ✅ **Model preferences**: Custom model settings preserved
- ✅ **Internal references**: All `/*` references working
- ✅ **Documentation**: Skill bodies unchanged

---

## Troubleshooting

### Issue 1: Skill not found after upgrade

**Symptom**: `/skill-name` returns "Skill not found"

**Cause**: Plugin cache issue

**Solution**:
1. Restart Claude Desktop
2. Verify `plugin.json` version is `3.0.0`
3. Check skill directory structure: `skills/skill-name/SKILL.md`

### Issue 2: Custom skill not loading

**Symptom**: Custom skill not appearing in plugin list

**Cause**: Missing required frontmatter fields

**Solution**:
1. Verify frontmatter has `name` field
2. Verify frontmatter has `user-invocable: true`
3. Check file naming: Must be `SKILL.md` (not `skill.md` or other)

### Issue 3: Agent trigger words not working

**Symptom**: Agent not auto-invoked when trigger words are mentioned

**Cause**: Missing `context: fork` field

**Solution**:
Add `context: fork` to agent-type skill frontmatter:
```yaml
---
name: my-agent
description: My agent
context: fork          # Add this
user-invocable: true
trigger_words:
  - trigger
---
```

### Issue 4: Arguments not working

**Symptom**: Skill ignores arguments passed by user

**Cause**: Arguments in frontmatter (old format)

**Solution**:
1. Remove `arguments` array from frontmatter
2. Document arguments in skill body
3. Implement argument parsing in skill logic

---

## FAQ

### Q1: Do I need to update my existing skill invocations?

**A**: No. Skill invocation syntax (`/skill-name`) remains unchanged.

### Q2: Are there any performance changes?

**A**: No performance impact. Directory-based auto-discovery has negligible overhead.

### Q3: Can I still use the old `mode` field?

**A**: The `mode` field is deprecated and ignored. It's safe to leave it in frontmatter, but it has no effect.

### Q4: Why was the `arguments` array removed from frontmatter?

**A**: Agent Skills standard recommends documenting arguments in skill body for better flexibility and readability. Frontmatter is reserved for metadata.

### Q5: Do I need to update README references?

**A**: If you maintain custom documentation referencing `agents/` or `commands/` directories, update them to `skills/`.

### Q6: What happens to old plugin versions?

**A**: v2.x plugins continue to work, but won't receive updates. Migration to v3.0.0 is recommended.

### Q7: Can I mix v2.x and v3.0.0 structures?

**A**: No. Plugin expects either v2.x structure OR v3.0.0 structure, not both. Complete migration is required.

### Q8: How do I rollback if something goes wrong?

**A**:
```bash
# Rollback to v2.2.0
git checkout v2.2.0

# Or revert migration commit
git revert <migration-commit-hash>
```

---

## Version Compatibility

| Plugin Version | Structure | Status |
|----------------|-----------|--------|
| v0.4.1 - v2.2.0 | `agents/` + `commands/` + `skills/` | ⚠️ Legacy |
| v3.0.0+ | `skills/` only | ✅ Current |

---

## Additional Resources

- [Agent Skills Documentation](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [CHANGELOG.md](../CHANGELOG.md) - Detailed version history
- [README.md](../README.md) - Plugin usage guide
- [plugin.json](../.claude-plugin/plugin.json) - Plugin configuration

---

## Summary

v3.0.0への移行により、プラグイン構造がシンプルかつ標準準拠になりました:

The v3.0.0 migration simplifies plugin structure and ensures standard compliance:

- ✅ **Unified architecture**: All skills in `skills/` directory
- ✅ **Standard compliance**: Full Agent Skills support
- ✅ **Backward compatible**: No user-facing changes
- ✅ **Simplified configuration**: Single `skills` directory reference
- ✅ **Future-proof**: Aligned with Claude platform evolution

**For users**: No action required - continue using skills as before.

**For developers**: Update custom skills to new frontmatter format and directory structure.
