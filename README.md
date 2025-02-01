# HandyRPC (WIP)

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
|i64|64bit signed integer|`-?(0\|[1-9][0-9]*)`<br>`0x[0-9a-fA-F_]+`<br>`0b[01_]+`|
|f64|64bit floating point|`-?(0\|[1-9][0-9]*(\.[0-9]+)?([eE][0-9]+)?)`|
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

- Success: `OK 0 return_value`
- Failed: `ERR status_code message`

# Status Code Definition

|Code Range|Description|
|:--:|:--|
|0x00|Success|
|0x20-0x2F|Local Protocol Error|
|0x30-0x3F|Remote Protocol Error|
|0x40-0x5F|Procedure Call Error|
|0x80-0xFF|Application Defined Error|
|other|(reserved)|

|Mnemonic|Code|Description|
|:--|:--:|:--|
|`SUCCESS`|0x00|No Error|
|`ERR_CONNECTION_FAILED`|0x20|Connection Failed|
|`ERR_RESPONSE_SYNTAX`|0x21|Response Syntax Error|
|`ERR_UNEXPECTED_RESPONSE_TYPE`|0x22|Unexpected Response Type|
|`ERR_COMMAND_SYNTAX`|0x30|Generic Command Line Syntax Error|
|`ERR_CONNECTION_REFUSED`|0x30|Generic Command Line Syntax Error|
|`ERR_COMMAND_NOT_FOUND`|0x40|Command Not Found|
|`ERR_BAD_ARGUMENT`|0x41|Generic Argument Error|

# System Commands

## handyrpc_hello

他の全てのコマンドの前に送信する。ホストは、コマンドの成功を示す文字列が返ってきたことをもって接続の確立と判断する。

### Arguments

none

### Return value

|Type|Value|Description|
|:--:|:--:|:--|
|str|`"handyrpc_welcome"`|indicates connection established|

## device_name

### Arguments

none

### Return Value

|Type|Value|Description|
|:--:|:--:|:--|
|str|any|device name|
