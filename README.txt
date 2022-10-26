Build clangd with:

cd /var/tmp
git clone -b v1.36.3 https://github.com/grpc/grpc grpc-1.36.3-src
cd grpc-1.36.3-src
git submodule update --init
export GRPC_INSTALL_PATH=/var/tmp/grpc-1.36.3-bin
mkdir build; cd build
cmake -DgRPC_INSTALL=ON -DCMAKE_INSTALL_PREFIX=${GRPC_INSTALL_PATH} -DCMAKE_BUILD_TYPE=Release ..
make install
cd /var/tmp
git clone -b llvmorg-15.0.3 https://github.com/llvm/llvm-project.git llvm-15.0.3-src
cd llvm-15.0.3-src
cmake -S llvm -B build -G "Unix Makefiles" -DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra" -DCMAKE_INSTALL_PREFIX=/var/tmp/llvm-15.0.3-bin -DCLANGD_ENABLE_REMOTE=On -DGRPC_INSTALL_PATH=$GRPC_INSTALL_PATH -DCMAKE_BUILD_TYPE=Release
cd build
make -j6 install
cp bin/clangd-* /var/tmp/llvm-15.0.3-bin/bin/


Start dummy index server:

netcat -vv -l -p 50051


Load project into VSCode with clangd extension:

I[15:09:18.601] clangd version 15.0.3 (https://github.com/llvm/llvm-project.git 4a2c05b05ed07f1f620e94f6524a8b4b2760a0b1)
I[15:09:18.601] Features: linux+grpc
I[15:09:18.601] PID: 501229
I[15:09:18.601] Working directory: /var/chris/clangd-test
I[15:09:18.601] argv[0]: /var/tmp/llvm-15.0.3-bin/bin/clangd
I[15:09:18.601] argv[1]: --compile-commands-dir=.
I[15:09:18.601] argv[2]: --log=verbose
I[15:09:18.601] argv[3]: --remote-index-address=localhost:50051
I[15:09:18.601] argv[4]: --project-root=/var/chris/clangd-test/
I[15:09:18.601] Connecting to remote index at localhost:50051
V[15:09:18.601] User config file is /var/chris/.config/clangd/config.yaml
I[15:09:18.601] Starting LSP over stdin/stdout
V[15:09:18.602] <<< {"id":0,"jsonrpc":"2.0","method":"initialize","params":{"capabilities":{"general":{"markdown":{"parser":"marked","version":"1.1.0"},"positionEncodings":["utf-16"],"regularExpressions":{"engine":"ECMAScript","version":"ES2020"},"staleRequestSupport":{"cancel":true,"retryOnContentModified":["textDocument/semanticTokens/full","textDocument/semanticTokens/range","textDocument/semanticTokens/full/delta"]}},"notebookDocument":{"synchronization":{"dynamicRegistration":true,"executionSummarySupport":true}},"textDocument":{"callHierarchy":{"dynamicRegistration":true},"codeAction":{"codeActionLiteralSupport":{"codeActionKind":{"valueSet":["","quickfix","refactor","refactor.extract","refactor.inline","refactor.rewrite","source","source.organizeImports"]}},"dataSupport":true,"disabledSupport":true,"dynamicRegistration":true,"honorsChangeAnnotations":false,"isPreferredSupport":true,"resolveSupport":{"properties":["edit"]}},"codeLens":{"dynamicRegistration":true},"colorProvider":{"dynamicRegistration":true},"completion":{"completionItem":{"commitCharactersSupport":true,"deprecatedSupport":true,"documentationFormat":["markdown","plaintext"],"insertReplaceSupport":true,"insertTextModeSupport":{"valueSet":[1,2]},"labelDetailsSupport":true,"preselectSupport":true,"resolveSupport":{"properties":["documentation","detail","additionalTextEdits"]},"snippetSupport":true,"tagSupport":{"valueSet":[1]}},"completionItemKind":{"valueSet":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25]},"completionList":{"itemDefaults":["commitCharacters","editRange","insertTextFormat","insertTextMode"]},"contextSupport":true,"dynamicRegistration":true,"editsNearCursor":true,"insertTextMode":2},"declaration":{"dynamicRegistration":true,"linkSupport":true},"definition":{"dynamicRegistration":true,"linkSupport":true},"diagnostic":{"dynamicRegistration":true,"relatedDocumentSupport":false},"documentHighlight":{"dynamicRegistration":true},"documentLink":{"dynamicRegistration":true,"tooltipSupport":true},"documentSymbol":{"dynamicRegistration":true,"hierarchicalDocumentSymbolSupport":true,"labelSupport":true,"symbolKind":{"valueSet":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26]},"tagSupport":{"valueSet":[1]}},"foldingRange":{"dynamicRegistration":true,"foldingRange":{"collapsedText":false},"foldingRangeKind":{"valueSet":["comment","imports","region"]},"lineFoldingOnly":true,"rangeLimit":5000},"formatting":{"dynamicRegistration":true},"hover":{"contentFormat":["markdown","plaintext"],"dynamicRegistration":true},"implementation":{"dynamicRegistration":true,"linkSupport":true},"inlayHint":{"dynamicRegistration":true,"resolveSupport":{"properties":["tooltip","textEdits","label.tooltip","label.location","label.command"]}},"inlineValue":{"dynamicRegistration":true},"linkedEditingRange":{"dynamicRegistration":true},"onTypeFormatting":{"dynamicRegistration":true},"publishDiagnostics":{"codeDescriptionSupport":true,"dataSupport":true,"relatedInformation":true,"tagSupport":{"valueSet":[1,2]},"versionSupport":false},"rangeFormatting":{"dynamicRegistration":true},"references":{"dynamicRegistration":true},"rename":{"dynamicRegistration":true,"honorsChangeAnnotations":true,"prepareSupport":true,"prepareSupportDefaultBehavior":1},"selectionRange":{"dynamicRegistration":true},"semanticTokens":{"augmentsSyntaxTokens":true,"dynamicRegistration":true,"formats":["relative"],"multilineTokenSupport":false,"overlappingTokenSupport":false,"requests":{"full":{"delta":true},"range":true},"serverCancelSupport":true,"tokenModifiers":["declaration","definition","readonly","static","deprecated","abstract","async","modification","documentation","defaultLibrary"],"tokenTypes":["namespace","type","class","enum","interface","struct","typeParameter","parameter","variable","property","enumMember","event","function","method","macro","keyword","modifier","comment","string","number","regexp","operator","decorator"]},"signatureHelp":{"contextSupport":true,"dynamicRegistration":true,"signatureInformation":{"activeParameterSupport":true,"documentationFormat":["markdown","plaintext"],"parameterInformation":{"labelOffsetSupport":true}}},"synchronization":{"didSave":true,"dynamicRegistration":true,"willSave":true,"willSaveWaitUntil":true},"typeDefinition":{"dynamicRegistration":true,"linkSupport":true},"typeHierarchy":{"dynamicRegistration":true}},"window":{"showDocument":{"support":true},"showMessage":{"messageActionItem":{"additionalPropertiesSupport":true}},"workDoneProgress":true},"workspace":{"applyEdit":true,"codeLens":{"refreshSupport":true},"configuration":true,"diagnostics":{"refreshSupport":true},"didChangeConfiguration":{"dynamicRegistration":true},"didChangeWatchedFiles":{"dynamicRegistration":true,"relativePatternSupport":true},"executeCommand":{"dynamicRegistration":true},"fileOperations":{"didCreate":true,"didDelete":true,"didRename":true,"dynamicRegistration":true,"willCreate":true,"willDelete":true,"willRename":true},"inlayHint":{"refreshSupport":true},"inlineValue":{"refreshSupport":true},"semanticTokens":{"refreshSupport":true},"symbol":{"dynamicRegistration":true,"resolveSupport":{"properties":["location.range"]},"symbolKind":{"valueSet":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26]},"tagSupport":{"valueSet":[1]}},"workspaceEdit":{"changeAnnotationSupport":{"groupsOnLabel":true},"documentChanges":true,"failureHandling":"textOnlyTransactional","normalizesLineEndings":true,"resourceOperations":["create","rename","delete"]},"workspaceFolders":true}},"clientInfo":{"name":"Visual Studio Code","version":"1.71.2"},"initializationOptions":{"clangdFileStatus":true,"fallbackFlags":[]},"locale":"en-gb","processId":501188,"rootPath":"/var/chris/clangd-test","rootUri":"file:///var/chris/clangd-test","trace":"off","workspaceFolders":[{"name":"clangd-test","uri":"file:///var/chris/clangd-test"}]}}

I[15:09:18.602] <-- initialize(0)
I[15:09:18.602] --> reply:initialize(0) 0 ms
V[15:09:18.603] >>> {"id":0,"jsonrpc":"2.0","result":{"capabilities":{"astProvider":true,"callHierarchyProvider":true,"clangdInlayHintsProvider":true,"codeActionProvider":{"codeActionKinds":["quickfix","refactor","info"]},"compilationDatabase":{"automaticReload":true},"completionProvider":{"resolveProvider":false,"triggerCharacters":[".","<",">",":","\"","/","*"]},"declarationProvider":true,"definitionProvider":true,"documentFormattingProvider":true,"documentHighlightProvider":true,"documentLinkProvider":{"resolveProvider":false},"documentOnTypeFormattingProvider":{"firstTriggerCharacter":"\n","moreTriggerCharacter":[]},"documentRangeFormattingProvider":true,"documentSymbolProvider":true,"executeCommandProvider":{"commands":["clangd.applyFix","clangd.applyTweak"]},"hoverProvider":true,"implementationProvider":true,"inlayHintProvider":true,"memoryUsageProvider":true,"referencesProvider":true,"renameProvider":{"prepareProvider":true},"selectionRangeProvider":true,"semanticTokensProvider":{"full":{"delta":true},"legend":{"tokenModifiers":["declaration","deprecated","deduced","readonly","static","abstract","virtual","dependentName","defaultLibrary","usedAsMutableReference","functionScope","classScope","fileScope","globalScope"],"tokenTypes":["variable","variable","parameter","function","method","function","property","variable","class","interface","enum","enumMember","type","type","unknown","namespace","typeParameter","concept","type","macro","comment"]},"range":false},"signatureHelpProvider":{"triggerCharacters":["(",")","{","}","<",">",","]},"standardTypeHierarchyProvider":true,"textDocumentSync":{"change":2,"openClose":true,"save":true},"typeDefinitionProvider":true,"typeHierarchyProvider":true,"workspaceSymbolProvider":true},"serverInfo":{"name":"clangd","version":"clangd version 15.0.3 (https://github.com/llvm/llvm-project.git 4a2c05b05ed07f1f620e94f6524a8b4b2760a0b1) linux+grpc x86_64-unknown-linux-gnu"}}}

V[15:09:18.618] <<< {"jsonrpc":"2.0","method":"initialized","params":{}}

I[15:09:18.618] <-- initialized
V[15:09:18.623] <<< {"jsonrpc":"2.0","method":"textDocument/didOpen","params":{"textDocument":{"languageId":"cpp","text":"#include <iostream>\n\nint main() {\n  std::cout << \"Hello world!\\n\";\n  return 0;\n}\n","uri":"file:///var/chris/clangd-test/foo.cpp","version":1}}}

I[15:09:18.623] <-- textDocument/didOpen
I[15:09:18.623] Loaded compilation database from /var/chris/clangd-test/./compile_commands.json
V[15:09:18.623] Broadcasting compilation database from /var/chris/clangd-test/.
I[15:09:18.623] ASTWorker building file /var/chris/clangd-test/foo.cpp version 1 with command 
[/var/chris/clangd-test]
/usr/bin/g++ --driver-mode=g++ -Wall -Wextra -pedantic-errors -c -resource-dir=/var/tmp/llvm-15.0.3-bin/lib/clang/15.0.3 -- /var/chris/clangd-test/foo.cpp
I[15:09:18.623] --> window/workDoneProgress/create(0)
V[15:09:18.623] >>> {"id":0,"jsonrpc":"2.0","method":"window/workDoneProgress/create","params":{"token":"backgroundIndexProgress"}}

I[15:09:18.624] Enqueueing 1 commands for indexing
V[15:09:18.624] <<< {"id":0,"jsonrpc":"2.0","result":null}

I[15:09:18.624] <-- reply(0)
I[15:09:18.624] --> $/progress
V[15:09:18.624] >>> {"jsonrpc":"2.0","method":"$/progress","params":{"token":"backgroundIndexProgress","value":{"kind":"begin","percentage":0,"title":"indexing"}}}

I[15:09:18.624] --> $/progress
V[15:09:18.624] >>> {"jsonrpc":"2.0","method":"$/progress","params":{"token":"backgroundIndexProgress","value":{"kind":"end"}}}

V[15:09:18.625] Driver produced command: cc1 -cc1 -triple x86_64-unknown-linux-gnu -fsyntax-only -disable-free -clear-ast-before-backend -disable-llvm-verifier -discard-value-names -main-file-name foo.cpp -mrelocation-model pic -pic-level 2 -pic-is-pie -mframe-pointer=all -fmath-errno -ffp-contract=on -fno-rounding-math -mconstructor-aliases -funwind-tables=2 -target-cpu x86-64 -tune-cpu generic -mllvm -treat-scalable-fixed-error-as-warning -debugger-tuning=gdb -fcoverage-compilation-dir=/var/chris/clangd-test -resource-dir /var/tmp/llvm-15.0.3-bin/lib/clang/15.0.3 -internal-isystem /usr/bin/../lib/gcc/x86_64-linux-gnu/10/../../../../include/c++/10 -internal-isystem /usr/bin/../lib/gcc/x86_64-linux-gnu/10/../../../../include/x86_64-linux-gnu/c++/10 -internal-isystem /usr/bin/../lib/gcc/x86_64-linux-gnu/10/../../../../include/c++/10/backward -internal-isystem /var/tmp/llvm-15.0.3-bin/lib/clang/15.0.3/include -internal-isystem /usr/local/include -internal-isystem /usr/bin/../lib/gcc/x86_64-linux-gnu/10/../../../../x86_64-linux-gnu/include -internal-externc-isystem /usr/include/x86_64-linux-gnu -internal-externc-isystem /include -internal-externc-isystem /usr/include -Wall -Wextra -pedantic-errors -fdeprecated-macro -fdebug-compilation-dir=/var/chris/clangd-test -ferror-limit 19 -fgnuc-version=4.2.1 -fcxx-exceptions -fexceptions -no-round-trip-args -faddrsig -D__GCC_HAVE_DWARF2_CFI_ASM=1 -x c++ /var/chris/clangd-test/foo.cpp
I[15:09:18.625] --> textDocument/clangd.fileStatus
V[15:09:18.625] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"parsing includes, running Update","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:18.625] Building first preamble for /var/chris/clangd-test/foo.cpp version 1
V[15:09:18.656] <<< {"id":1,"jsonrpc":"2.0","method":"textDocument/codeAction","params":{"context":{"diagnostics":[],"triggerKind":2},"range":{"end":{"character":0,"line":0},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:18.656] <-- textDocument/codeAction(1)
V[15:09:18.657] <<< {"id":2,"jsonrpc":"2.0","method":"textDocument/documentLink","params":{"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:18.658] <-- textDocument/documentLink(2)
V[15:09:18.658] <<< {"id":3,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:18.658] <-- textDocument/inlayHint(3)
V[15:09:18.692] <<< {"jsonrpc":"2.0","method":"$/cancelRequest","params":{"id":2}}

I[15:09:18.692] <-- $/cancelRequest
V[15:09:18.694] <<< {"id":4,"jsonrpc":"2.0","method":"textDocument/documentLink","params":{"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:18.694] <-- textDocument/documentLink(4)
V[15:09:18.812] indexed preamble AST for /var/chris/clangd-test/foo.cpp version 1:
  symbol slab: 4405 symbols, 1403008 bytes
  ref slab: 0 symbols, 0 refs, 128 bytes
  relations slab: 285 relations, 8728 bytes
V[15:09:18.828] Build dynamic index for header symbols with estimated memory usage of 3551148 bytes
V[15:09:18.830] Built preamble of size 2590280 for file /var/chris/clangd-test/foo.cpp version 1 in 0.20 seconds
I[15:09:18.830] --> workspace/semanticTokens/refresh(1)
V[15:09:18.830] >>> {"id":1,"jsonrpc":"2.0","method":"workspace/semanticTokens/refresh","params":null}

I[15:09:18.830] --> textDocument/clangd.fileStatus
V[15:09:18.830] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"parsing includes, running Build AST","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:18.831] <<< {"id":1,"jsonrpc":"2.0","result":null}

I[15:09:18.831] <-- reply(1)
V[15:09:18.840] indexed file AST for /var/chris/clangd-test/foo.cpp version 1:
  symbol slab: 1 symbols, 4448 bytes
  ref slab: 3 symbols, 3 refs, 4320 bytes
  relations slab: 0 relations, 24 bytes
V[15:09:18.840] Build dynamic index for main-file symbols with estimated memory usage of 11656 bytes
I[15:09:18.840] --> textDocument/publishDiagnostics
V[15:09:18.840] >>> {"jsonrpc":"2.0","method":"textDocument/publishDiagnostics","params":{"diagnostics":[],"uri":"file:///var/chris/clangd-test/foo.cpp","version":1}}

V[15:09:18.840] ASTWorker running EnumerateTweaks on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:18.841] --> reply:textDocument/codeAction(1) 184 ms
V[15:09:18.841] >>> {"id":1,"jsonrpc":"2.0","result":[]}

I[15:09:18.841] --> reply:textDocument/documentLink(2) 183 ms, error: Task was cancelled.
V[15:09:18.841] >>> {"error":{"code":-32800,"message":"Request cancelled"},"id":2,"jsonrpc":"2.0"}

V[15:09:18.841] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:18.841] --> reply:textDocument/inlayHint(3) 182 ms
V[15:09:18.841] >>> {"id":3,"jsonrpc":"2.0","result":[]}

V[15:09:18.841] ASTWorker running DocumentLinks on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:18.841] --> reply:textDocument/documentLink(4) 146 ms
V[15:09:18.841] >>> {"id":4,"jsonrpc":"2.0","result":[{"range":{"end":{"character":19,"line":0},"start":{"character":9,"line":0}},"target":"file:///usr/include/c%2B%2B/10/iostream"}]}

I[15:09:18.841] --> textDocument/clangd.fileStatus
V[15:09:18.841] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

[Error - 15:09:18] Request textDocument/documentLink failed.
[object Object]
V[15:09:18.849] <<< {"id":5,"jsonrpc":"2.0","method":"textDocument/semanticTokens/full","params":{"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:18.849] <-- textDocument/semanticTokens/full(5)
V[15:09:18.849] ASTWorker running SemanticHighlights on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:18.849] --> reply:textDocument/semanticTokens/full(5) 0 ms
V[15:09:18.849] >>> {"id":5,"jsonrpc":"2.0","result":{"data":[2,4,4,3,8193,1,2,3,15,8448,0,5,4,0,8448],"resultId":"1"}}

I[15:09:18.849] --> textDocument/clangd.fileStatus
V[15:09:18.849] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:20.350] <<< {"id":6,"jsonrpc":"2.0","method":"textDocument/codeAction","params":{"context":{"diagnostics":[],"triggerKind":2},"range":{"end":{"character":0,"line":0},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:20.350] <-- textDocument/codeAction(6)
V[15:09:20.350] ASTWorker running EnumerateTweaks on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:20.350] --> reply:textDocument/codeAction(6) 0 ms
V[15:09:20.350] >>> {"id":6,"jsonrpc":"2.0","result":[]}

I[15:09:20.350] --> textDocument/clangd.fileStatus
V[15:09:20.350] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:20.376] <<< {"id":7,"jsonrpc":"2.0","method":"textDocument/documentSymbol","params":{"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:20.376] <-- textDocument/documentSymbol(7)
V[15:09:20.376] ASTWorker running DocumentSymbols on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:20.376] --> reply:textDocument/documentSymbol(7) 0 ms
V[15:09:20.376] >>> {"id":7,"jsonrpc":"2.0","result":[{"detail":"int ()","kind":12,"name":"main","range":{"end":{"character":1,"line":5},"start":{"character":0,"line":2}},"selectionRange":{"end":{"character":8,"line":2},"start":{"character":4,"line":2}}}]}

I[15:09:20.376] --> textDocument/clangd.fileStatus
V[15:09:20.376] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.248] <<< {"id":8,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.248] <-- textDocument/inlayHint(8)
V[15:09:33.248] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.248] --> reply:textDocument/inlayHint(8) 0 ms
V[15:09:33.248] >>> {"id":8,"jsonrpc":"2.0","result":[]}

I[15:09:33.248] --> textDocument/clangd.fileStatus
V[15:09:33.248] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.455] <<< {"id":9,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.455] <-- textDocument/inlayHint(9)
V[15:09:33.455] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.455] --> reply:textDocument/inlayHint(9) 0 ms
V[15:09:33.455] >>> {"id":9,"jsonrpc":"2.0","result":[]}

I[15:09:33.455] --> textDocument/clangd.fileStatus
V[15:09:33.455] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.544] <<< {"id":10,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.544] <-- textDocument/inlayHint(10)
V[15:09:33.544] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.544] --> reply:textDocument/inlayHint(10) 0 ms
V[15:09:33.544] >>> {"id":10,"jsonrpc":"2.0","result":[]}

I[15:09:33.544] --> textDocument/clangd.fileStatus
V[15:09:33.544] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.644] <<< {"id":11,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.644] <-- textDocument/inlayHint(11)
V[15:09:33.644] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.644] --> reply:textDocument/inlayHint(11) 0 ms
V[15:09:33.644] >>> {"id":11,"jsonrpc":"2.0","result":[]}

I[15:09:33.644] --> textDocument/clangd.fileStatus
V[15:09:33.644] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.714] <<< {"id":12,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.714] <-- textDocument/inlayHint(12)
V[15:09:33.714] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.715] --> reply:textDocument/inlayHint(12) 0 ms
V[15:09:33.715] >>> {"id":12,"jsonrpc":"2.0","result":[]}

I[15:09:33.715] --> textDocument/clangd.fileStatus
V[15:09:33.715] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.764] <<< {"id":13,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.764] <-- textDocument/inlayHint(13)
V[15:09:33.764] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.764] --> reply:textDocument/inlayHint(13) 0 ms
V[15:09:33.764] >>> {"id":13,"jsonrpc":"2.0","result":[]}

I[15:09:33.764] --> textDocument/clangd.fileStatus
V[15:09:33.764] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.808] <<< {"id":14,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.808] <-- textDocument/inlayHint(14)
V[15:09:33.808] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.808] --> reply:textDocument/inlayHint(14) 0 ms
V[15:09:33.808] >>> {"id":14,"jsonrpc":"2.0","result":[]}

I[15:09:33.808] --> textDocument/clangd.fileStatus
V[15:09:33.808] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.846] <<< {"id":15,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.846] <-- textDocument/inlayHint(15)
V[15:09:33.846] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.846] --> reply:textDocument/inlayHint(15) 0 ms
V[15:09:33.846] >>> {"id":15,"jsonrpc":"2.0","result":[]}

I[15:09:33.846] --> textDocument/clangd.fileStatus
V[15:09:33.846] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.887] <<< {"id":16,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.887] <-- textDocument/inlayHint(16)
V[15:09:33.887] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.887] --> reply:textDocument/inlayHint(16) 0 ms
V[15:09:33.887] >>> {"id":16,"jsonrpc":"2.0","result":[]}

I[15:09:33.887] --> textDocument/clangd.fileStatus
V[15:09:33.887] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.929] <<< {"id":17,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.929] <-- textDocument/inlayHint(17)
V[15:09:33.929] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.929] --> reply:textDocument/inlayHint(17) 0 ms
V[15:09:33.929] >>> {"id":17,"jsonrpc":"2.0","result":[]}

I[15:09:33.929] --> textDocument/clangd.fileStatus
V[15:09:33.929] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:33.961] <<< {"id":18,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:33.961] <-- textDocument/inlayHint(18)
V[15:09:33.961] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:33.961] --> reply:textDocument/inlayHint(18) 0 ms
V[15:09:33.961] >>> {"id":18,"jsonrpc":"2.0","result":[]}

I[15:09:33.961] --> textDocument/clangd.fileStatus
V[15:09:33.961] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.117] <<< {"id":19,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.117] <-- textDocument/inlayHint(19)
V[15:09:34.117] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.117] --> reply:textDocument/inlayHint(19) 0 ms
V[15:09:34.117] >>> {"id":19,"jsonrpc":"2.0","result":[]}

I[15:09:34.117] --> textDocument/clangd.fileStatus
V[15:09:34.117] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.148] <<< {"id":20,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.148] <-- textDocument/inlayHint(20)
V[15:09:34.148] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.148] --> reply:textDocument/inlayHint(20) 0 ms
V[15:09:34.148] >>> {"id":20,"jsonrpc":"2.0","result":[]}

I[15:09:34.148] --> textDocument/clangd.fileStatus
V[15:09:34.148] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.180] <<< {"id":21,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.180] <-- textDocument/inlayHint(21)
V[15:09:34.180] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.180] --> reply:textDocument/inlayHint(21) 0 ms
V[15:09:34.180] >>> {"id":21,"jsonrpc":"2.0","result":[]}

I[15:09:34.180] --> textDocument/clangd.fileStatus
V[15:09:34.180] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.213] <<< {"id":22,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.213] <-- textDocument/inlayHint(22)
V[15:09:34.213] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.213] --> reply:textDocument/inlayHint(22) 0 ms
V[15:09:34.213] >>> {"id":22,"jsonrpc":"2.0","result":[]}

I[15:09:34.213] --> textDocument/clangd.fileStatus
V[15:09:34.213] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.248] <<< {"id":23,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.248] <-- textDocument/inlayHint(23)
V[15:09:34.248] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.248] --> reply:textDocument/inlayHint(23) 0 ms
V[15:09:34.248] >>> {"id":23,"jsonrpc":"2.0","result":[]}

I[15:09:34.248] --> textDocument/clangd.fileStatus
V[15:09:34.248] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.277] <<< {"id":24,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.277] <-- textDocument/inlayHint(24)
V[15:09:34.277] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.277] --> reply:textDocument/inlayHint(24) 0 ms
V[15:09:34.277] >>> {"id":24,"jsonrpc":"2.0","result":[]}

I[15:09:34.277] --> textDocument/clangd.fileStatus
V[15:09:34.277] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.310] <<< {"id":25,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.310] <-- textDocument/inlayHint(25)
V[15:09:34.310] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.310] --> reply:textDocument/inlayHint(25) 0 ms
V[15:09:34.310] >>> {"id":25,"jsonrpc":"2.0","result":[]}

I[15:09:34.310] --> textDocument/clangd.fileStatus
V[15:09:34.310] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.343] <<< {"id":26,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.343] <-- textDocument/inlayHint(26)
V[15:09:34.343] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.343] --> reply:textDocument/inlayHint(26) 0 ms
V[15:09:34.343] >>> {"id":26,"jsonrpc":"2.0","result":[]}

I[15:09:34.343] --> textDocument/clangd.fileStatus
V[15:09:34.343] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.387] <<< {"id":27,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.387] <-- textDocument/inlayHint(27)
V[15:09:34.387] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.387] --> reply:textDocument/inlayHint(27) 0 ms
V[15:09:34.387] >>> {"id":27,"jsonrpc":"2.0","result":[]}

I[15:09:34.387] --> textDocument/clangd.fileStatus
V[15:09:34.387] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.426] <<< {"id":28,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.426] <-- textDocument/inlayHint(28)
V[15:09:34.427] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.427] --> reply:textDocument/inlayHint(28) 0 ms
V[15:09:34.427] >>> {"id":28,"jsonrpc":"2.0","result":[]}

I[15:09:34.427] --> textDocument/clangd.fileStatus
V[15:09:34.427] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.465] <<< {"id":29,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.465] <-- textDocument/inlayHint(29)
V[15:09:34.465] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.465] --> reply:textDocument/inlayHint(29) 0 ms
V[15:09:34.465] >>> {"id":29,"jsonrpc":"2.0","result":[]}

I[15:09:34.465] --> textDocument/clangd.fileStatus
V[15:09:34.465] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.499] <<< {"id":30,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.499] <-- textDocument/inlayHint(30)
V[15:09:34.499] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.499] --> reply:textDocument/inlayHint(30) 0 ms
V[15:09:34.499] >>> {"id":30,"jsonrpc":"2.0","result":[]}

I[15:09:34.499] --> textDocument/clangd.fileStatus
V[15:09:34.499] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.537] <<< {"id":31,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.537] <-- textDocument/inlayHint(31)
V[15:09:34.537] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.537] --> reply:textDocument/inlayHint(31) 0 ms
V[15:09:34.537] >>> {"id":31,"jsonrpc":"2.0","result":[]}

I[15:09:34.537] --> textDocument/clangd.fileStatus
V[15:09:34.537] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.580] <<< {"id":32,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.580] <-- textDocument/inlayHint(32)
V[15:09:34.580] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.580] --> reply:textDocument/inlayHint(32) 0 ms
V[15:09:34.580] >>> {"id":32,"jsonrpc":"2.0","result":[]}

I[15:09:34.580] --> textDocument/clangd.fileStatus
V[15:09:34.580] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.609] <<< {"id":33,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.609] <-- textDocument/inlayHint(33)
V[15:09:34.609] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.609] --> reply:textDocument/inlayHint(33) 0 ms
V[15:09:34.609] >>> {"id":33,"jsonrpc":"2.0","result":[]}

I[15:09:34.609] --> textDocument/clangd.fileStatus
V[15:09:34.609] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

V[15:09:34.637] <<< {"id":34,"jsonrpc":"2.0","method":"textDocument/inlayHint","params":{"range":{"end":{"character":0,"line":6},"start":{"character":0,"line":0}},"textDocument":{"uri":"file:///var/chris/clangd-test/foo.cpp"}}}

I[15:09:34.638] <-- textDocument/inlayHint(34)
V[15:09:34.638] ASTWorker running InlayHints on version 1 of /var/chris/clangd-test/foo.cpp
I[15:09:34.638] --> reply:textDocument/inlayHint(34) 0 ms
V[15:09:34.638] >>> {"id":34,"jsonrpc":"2.0","result":[]}

I[15:09:34.638] --> textDocument/clangd.fileStatus
V[15:09:34.638] >>> {"jsonrpc":"2.0","method":"textDocument/clangd.fileStatus","params":{"state":"idle","uri":"file:///var/chris/clangd-test/foo.cpp"}}

