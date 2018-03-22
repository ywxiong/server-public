%%%-------------------------------------------------------------------
%%% @author zyuyou
%%% @copyright (C) 2017, <COMPANY>
%%% @doc
%%%     命令行执行脚本
%%% @end
%%% Created : 17. 一月 2017 14:00
%%%-------------------------------------------------------------------
-module(shell_console).
-author("zyuyou").

-include("shell_console.hrl").

%% API
-export([
    exec/0,
    exec/2
]).

%% ====================================================================
%% API functions
%% ====================================================================
exec() ->
    [Node0, Opt0 | Args] = init:get_plain_arguments(),
    Ret =
        case rpc:call(to_atom(Node0), shell_console, exec, [to_atom(Opt0), Args], ?ERLANG_TIMEOUT) of
            RetCode when is_integer(RetCode) ->
                RetCode;
            ok ->
                ?ERLANG_RET_OK;
            {error, no_opt} ->
                ?ERLANG_RET_NO_OPT;
            {error, no_config_handler} ->
                ?ERLANG_RET_UNKNOWN_HANDLER;
            {badrpc, nodedown} ->
                ?ERLANG_RET_NODE_DOWN;
            {badrpc, timeout} ->
                ?ERLANG_RET_TIMEOUT;
            _ErrorCode ->
                io:format(standard_error, "===== ~p ===== Opt: [~s] Args: ~s ~n~p~n~n", [erlang:localtime(), Opt0, Args, _ErrorCode]),
                ?ERLANG_RET_ERROR
        end,

    erlang:halt(Ret).

%% --------------------------------------------------------------------
exec(app_status, Args) ->
    case app_status(Args) of
        true ->
            ?STATUS_SUCCESS;
        false ->
            ?STATUS_ERROR
    end;

exec(Cmd, Args) ->
    case application:get_env(shell_console, console_handler) of
        undefined ->
            case application:get_env(shell_console, console_cmds) of
                undefined ->
                    {error, no_opt};
                {ok, CmdList} ->
                    case lists:keyfind(Cmd, 1, CmdList) of
                        false ->
                            {error, no_opt};
                        {Cmd, Module, Fun} ->
                            apply(Module, Fun, [Args]);
                        {Cmd, Module} ->
                            apply(Module, exec, Args)
                    end
            end;
        {ok, Module} when is_atom(Module) ->
            apply(Module, exec, [Cmd, Args]);
        {ok, {Module, Fun}} ->
            apply(Module, Fun, [Cmd, Args]);
        {ok, _UnknownHandler} ->
            {error, unknown_handler}
    end.

%% ====================================================================
%% Internal functions
%% ====================================================================
%% @doc convert other type to atom
to_atom(Val) when is_atom(Val) ->
    Val;
to_atom(Val) when is_binary(Val) ->
    binary_to_atom(Val, utf8);
to_atom(Val) when is_list(Val) ->
    list_to_atom(Val);
to_atom(_) ->
    throw({error, badarg}).

%% @private 查看应用状态
app_status(App) when not is_list(App) ->
    app_status([App]);
app_status(Apps) ->
    AllStartedApps = application:which_applications(),
    lists:all(
        fun(App) ->
            lists:keyfind(to_atom(App), 1, AllStartedApps) =/= false
        end, Apps).
