# MODIFICATIONS.md —— 相对上游的修改

> 上游：omdsh-dev/dsh-tool-calculator @ b2007a13f06bcf75bf07b9d277ee8d434a316490
> Fork 维护者：moqsting（GitHub）
> 原则：保留原 author 与 LICENSE；本文件是“相对原作者已修改”的权威记录，整合包文档直接引用。

## 修改清单

| # | 文件 | 修改 | 原因 | 对应审查结论 |
|---|---|---|---|---|
| 1 | `package.json` | 包名 `@deepseek-ai/dsh-tool-calculator` → `dsh-tool-calculator`；`peerDependencies` 的 `@deepseek-ai/dsh-tools`、`@deepseek-ai/dsh-invariants` 范围 `>=0.0.1-rc.1 <0.2.0` → `>=0.0.1-rc.1 <0.3.0`；`repository` 改指 fork URL | 去掉对官方 `@deepseek-ai` scope 的占用；修复 peer 版本上界过窄（约束#3） | 瑕疵 A；约束#3 |
| 2 | `cordis.patch.yml` | `name` 由 `@deepseek-ai/dsh-tool-calculator` → `dsh-tool-calculator` | 与包名一致，否则 patch 按 name 找不到插件 | 瑕疵 A |
| 3 | `src/index.ts` + `lib/index.js`（含 `lib/index.d.ts`、`lib/types/index.d.ts`） | `export const name` 同步新包名 | 一致 | 瑕疵 A |
| 4 | `src/invariant.ts` + `lib/invariant.js`（含 `lib/invariant.d.ts`、`lib/types/invariant.d.ts`） | `PACKAGE_NAME` 同步新包名 | 一致 | 瑕疵 A |
| 5 | `tests/register.spec.ts` | 断言 `name` 同步新包名 | 重命名后测试必须通过（约束#4） | 瑕疵 A |
| 6 | `README.md` / `README.en.md` | 代码片段与 peer 范围说明同步新包名与新范围 | 文档一致 | 瑕疵 A；约束#3 |

## 验证

- `npm test`：31/31 通过（`register.spec.ts` 2 项 + `evaluate.spec.ts` 29 项）。
- `verify-security.mjs`（90 项对抗）：90 passed / 0 failed，`globalThis.__pwn` 未触发，ALL CHECKS PASSED。
- `lib/evaluate.js` 与上游字节一致（SHA256 `E8B1260E…`），求值器安全边界不受重命名影响。

## 未改

- 零运行时依赖（`dependencies` 为空），无需锁版本。
- 保留上游 `author`、`LICENSE`（MIT）。
