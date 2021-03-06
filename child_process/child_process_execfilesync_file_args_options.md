<!-- YAML
added: v0.11.12
changes:
  - version: v8.8.0
    pr-url: https://github.com/nodejs/node/pull/15380
    description: The `windowsHide` option is supported now.
  - version: v8.0.0
    pr-url: https://github.com/nodejs/node/pull/10653
    description: The `input` option can now be a `Uint8Array`.
  - version: v6.2.1, v4.5.0
    pr-url: https://github.com/nodejs/node/pull/6939
    description: The `encoding` option can now explicitly be set to `buffer`.
-->

* `file` {string} 要运行的可执行文件的名称或路径。
* `args` {string[]} 字符串参数列表。
* `options` {Object}
  * `cwd` {string} 子进程的当前工作目录。
  * `input` {string|Buffer|Uint8Array} 要作为 stdin 传给衍生进程的值。
    - 提供该值会覆盖 `stdio[0]`。
  * `stdio` {string|Array} 子进程的 stdio 配置。默认为 `'pipe'`。
    - `stderr` 默认会输出到父进程中的 stderr，除非指定了 `stdio`。
  * `env` {Object} 环境变量键值对。
  * `uid` {number} 设置该进程的用户标识。（详见 setuid(2)）
  * `gid` {number} 设置该进程的组标识。（详见 setgid(2)）
  * `timeout` {number} 进程允许运行的最大时间数，以毫秒为单位。默认为 `undefined`。
  * `killSignal` {string|integer} 当衍生进程将被杀死时要使用的信号值。默认为 `'SIGTERM'`。
  * [`maxBuffer`] {number} stdout 或 stderr 允许的最大字节数。
    默认为 `200*1024`。
    如果超过限制，则子进程会被终止。
    See caveat at [`maxBuffer` and Unicode][].
  * `encoding` {string} 用于所有 stdio 输入和输出的编码。默认为 `'buffer'`。
  * `windowsHide` {boolean} Hide the subprocess console window that would
    normally be created on Windows systems. **Default:** `false`.
* 返回: {Buffer|string} 该命令的 stdout。

`child_process.execFileSync()` 方法与 [`child_process.execFile()`] 基本相同，除了该方法直到子进程完全关闭后才返回。
当遇到超时且发送了 `killSignal` 时，则该方法直到进程完全退出后才返回结果。

注意，如果子进程拦截并处理了 `SIGTERM` 信号且没有退出，则父进程会一直等待直到子进程退出。

如果进程超时，或有一个非零的退出码，则该方法会抛出一个 [`Error`]，这个错误对象包含了底层 [`child_process.spawnSync()`] 的完整结果。

