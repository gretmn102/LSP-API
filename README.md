# LSP-API for F#

Forked from [this](https://github.com/fsharp/FsAutoComplete/blob/0ab77c9e471b6d5a2f8edae818c28e36ca4578d1/src/LanguageServerProtocol/LanguageServerProtocol.fs) because:
* Implementation outdated and does not match my requirements. For example, there absent [`prepareRename`](https://microsoft.github.io/language-server-protocol/specifications/specification-3-15/#textDocument_prepareRename) method, and I don't want to ask they add this personally for me;
* How to answer a request that it is successful, but without value? I tried use `LspResult.success None` in `TextDocumentHover`, but targeted VS Code extension throw exception:
    ```
    [Error - 08:24:09] Request textDocument/TextDocumentHover failed.
    Error: The received response has neither a result nor an error property.
    	at handleInvalidMessage (c:\Users\User\.vscode\extensions\fering.qsp-0.0.3\qsp.js:5407:40)
    	at processMessageQueue (c:\Users\User\.vscode\extensions\fering.qsp-0.0.3\qsp.js:5156:17)
    	at Immediate.<anonymous> (c:\Users\User\.vscode\extensions\fering.qsp-0.0.3\qsp.js:5137:13)
    	at processImmediate (internal/timers.js:439:21)
    ```
    Why? ¯\\\_(ツ)\_/¯
    
    I suspect he answering like this:
    ```json
    {
	    "id":0,
	    "result":null,
        "error":null
    }
    ```
    But should:
    ```json
    {
	    "id":0,
	    "result":null
    }
    ```
* How to use, for example, `LspClient.WorkspaceWorkspaceFolders` ([specification](https://microsoft.github.io/language-server-protocol/specifications/specification-3-15/#workspace_workspaceFolders)) if client request not implemented? Just there is `ClientNotificationSender` that always return `unit` for all request.

## Build
For build needed 
1. Run `dotnet paket install`
2. Run `build.cmd` for Windows and `build.sh` for other platforms