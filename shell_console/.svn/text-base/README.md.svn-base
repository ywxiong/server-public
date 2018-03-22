shell_console
=====

An Shell Console OTP library

Build
-----

    $ rebar3 compile
    
Usage
-----
- Add `shell_console` to application source file's **applications** tag.

- Add handler config to application runtime config `sys.config`
```
[
    {shell_console, [
        {console_handler, 'Module'}     % has fun exec/2 .
        ... or ...
        {console_handler, {'Module', 'fun Fun/2'}}
        
        ... or ...
        
        {console_cmds, [
            {'Cmd1', 'Module', 'fun Fun/1'},
            {'Cmd2', 'Module'}  % has fun exec/1 .
        ]}
    ]}
].
```




Ret Code
-----

```
-define(STATUS_SUCCESS, 0).     % 应用节点状态 - 正常状态
-define(STATUS_ERROR, 1).       % 应用节点状态 - 异常状态

-define(ERLANG_RET_OK, 100).                % 成功
-define(ERLANG_RET_NODE_DOWN, 102).         % 节点未启动
-define(ERLANG_RET_TIMEOUT, 103).           % 超时
-define(ERLANG_RET_NO_OPT, 104).            % 操作不存在
-define(ERLANG_RET_UNKNOWN_HANDLER, 105).   % 未知处理模块

-define(ERLANG_RET_ERROR, 200).             % 错误

-define(ERLANG_RET_OPEN, 300).              % 游戏服状态 - 开
-define(ERLANG_RET_CLOSE, 301).             % 游戏服状态 - 关
```