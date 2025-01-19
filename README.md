# HandyRPC

# Recommended Serial Setting

|Config|Value|
|:--|:--|
|Baudrate|115200 bps|
|Stop Bits|1|
|Parity Bits|0|
|Return Code|`CR` or `LF` or `CRLF`|

# Data Type Definition

|Type Name|Description|Format|
|:--:|:--|:--|
|i32|32bit signed integer|`-?(0\|[1-9][0-9]*)`<br>`0x[0-9a-fA-F_]+`<br>`0b[01_]+`|
|f32|32bit floating point|`-?(0\|[1-9][0-9]*(\.[0-9]+)?([eE][0-9]+)?)`|
|bool|boolean|`false` or `true`|
|str|string|Printable ASCII characters only<br>should be enclosed it in `"..."`<br>`\"`-->`"`, `\r`-->`CR`, `\n`-->`LF`, `\t`-->Tab|
|void|void|`void`|

# Command/Response Format

空行は無視すること。

送信側は改行文字に `CRLF` を使用できるが、受信側は 1 個の `CR` または `LF` を改行として扱ってよい。この場合 `CRLF` は行末の後に空行が存在するように見える。

## Command (Host --> Device)

一般的な CLI のコマンドラインと同じような形でコマンドを 1 行で送信する。

`command_name -arg0_name arg0_value -arg1_name arg1_value ...`

## Response (Device --> Host)

1個のコマンドに対してデバイスは 1 個の応答を返す。

- Success: `OK return_value`
- Failed: `ERR code message`

# Error Code Definition

|Mnemonic|Code|Description|
|:--|:--:|:--|
|`ERR_BAD_SYNTAX`|0x1|command line syntax error|
|`ERR_CMD_NOT_FOUND`|0x2|command not found|
|`ERR_BAD_ARG_FORMAT`|0x41|bad argument|
|(application defined)|0x80-0xFF|application defined error|

# System Commands

## handyrpc_hello

他の全てのコマンドの前に送信する。ホストは、コマンドの成功を示す文字列が返ってきたことをもって接続の確立と判断する。

- Arguments: none
- Return value:

  |Value|Type|Description|
  |:--:|:--:|:--|
  |`"handyrpc_welcome"`|str|indicates connection established|
