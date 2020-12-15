src/node_main.cc  
```node::Start(argc, argv);```  
src/node.cc  
```
int Start(int argc, char** argv) {
  PlatformInt()
  RegisterSignalHandler(SIGINT, SignalExit, true);
  RegisterSignalHandler(SIGTERM, SignalExit, true);  
```
