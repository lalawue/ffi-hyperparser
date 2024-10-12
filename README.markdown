
Deprecated, replaced by https://github.com/lalawue/ffi-http1-session

----

Hyperparser - LuaJIT HTTP parser (based on [http-parser](https://github.com/nodejs/http-parser))

It runs on *BSD and Linux. Windows was not tested.

support LuaJIT's pull-style api, require luarocks to make.

## Install or Compilation

   $ luarocks install ffi-hyperparser

Needs luarocks to compile and install:

    $ [sudo] make

## Usage:

Using pull-style API:

```lua
local Hyperparser = require "ffi_hyperparser"

local function testRequest()
   local parser = Hyperparser.createParser("request")
   local req = "GET /index/?key=val&key2=val2 HTTP/1.1\r\nHost: www.example.com\r\n\r\n"
   local nread, tbl = parser:process(req)
   return nread, tbl
end

local function testResponse()
   local parser = Hyperparser.createParser("response")
   local resp = "HTTP/1.1 200 OK\r\ncontent-type: text/html; charset=UTF-8\r\ndate: Tue, 15 May 1999 12:20:37 GMT\r\nserver: nginx\r\nx-powered-by: PHP/5.6.32\r\nContent-Length: 9\r\n\r\n123456789\r\n"
   local nread, tbl = parser:process(resp)
   return nread, tbl
end

local function getResult(nread, tbl)
   if nread < 0 then
      print("error encouter")
      os.exit(0)
      -- handle error
   end

   if tbl == nil then
      print("header data not ready")
      os.exit(0)
   elseif tbl.content == nil then
      -- body data not ready, or no body data
   end


   if tbl.method then
      print("method:", tbl.method)
   end
   if tbl.url then
      print("url:", tbl.url)
   end
   if tbl.status_code then
      print("status_code:", tbl.status_code)
   end
   if tbl.content_length then
      print("content_length:", tbl.content_length)
   end
   print("readed_length:", tbl.readed_length)
   print("header:")
   for k, v in pairs(tbl.header) do
      print("\t", k, v)
   end
   if tbl.contents then
      print("contents:")
      for _, c in ipairs(tbl.contents) do
         print(c)
      end
   end
end

print("-- test request:")
getResult( testRequest() )
print("-- test response:")
getResult( testResponse() )
```

## Origin

from https://github.com/armatys/hyperparser

## Used in

- [Cincau](https://github.com/lalawue/cincau)
- [rpc_framework](https://github.com/lalawue/rpc_framework)
