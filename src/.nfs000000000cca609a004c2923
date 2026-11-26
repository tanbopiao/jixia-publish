/**
 * DSH Human Commands 类型扩充（对应 DSH docs/subsystems/commands.md 的 CommandRuntime）。
 *
 * `ctx.commands` 由 DSH 的 commands 服务贡献；该服务目前随 DSH monorepo 分发，
 * 未单独发布到 npm。独立开发插件时在此按官方文档声明扩充；
 * DSH 后续发布官方类型后，本文件可整体替换为对官方包的 `import type {}`。
 */

/** Invocation passed to one registered command handler. */
export interface CommandInvocation {
  /** Pairing id already written to this invocation's `command/run` event. */
  readonly commandId: string
  /** Exact text following the registered command name, including separator whitespace. */
  readonly rawInput: string
  /** Cancellation signal owned by the dispatching UI request. */
  readonly signal: AbortSignal
}

/** Expected command outcome rendered directly by the dispatching UI. */
export type CommandResult =
  | { readonly kind: 'success'; readonly text?: string }
  | { readonly kind: 'error'; readonly text: string }

/** Plugin-owned command registration. */
export interface CommandDefinition {
  /** Lowercase command name without the leading slash. */
  readonly name: string
  /** Human-readable summary used in discovery UI. */
  readonly description: string
  /** Execute against the receiving agent without sending the command to the model. */
  readonly handler: (invocation: CommandInvocation) => CommandResult | Promise<CommandResult>
}

/** Human-command registry contributed as `ctx.commands`. */
export interface CommandRuntime {
  /** Register a global or calling-agent-scoped command; returns its disposer. */
  register(definition: CommandDefinition): () => void
}

declare module '@deepseek-ai/cordis' {
  interface Context {
    commands: CommandRuntime
  }
}
