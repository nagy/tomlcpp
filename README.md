# tomlcpp
TOML CPP Library

This is a C++ wrapper around the C library available here: https://github.com/cktan/tomlc99.

## Usage

Here is a simple example that parses this config file:

```
[server]
	host = "example.com"
	port = 80
```

Steps for getting values:

1. Read the file into a string
2. Call toml::parse on the string
3. Get the top-level table
4. Get values from the top-level table
5. Examine the values

```
// parse a string containing toml data
auto result = toml::parse(str);
if (!result.table) {
    handle_error(result.errmsg);
}

// get the top level table
auto server = result.table.getTable("server");
if (!server) {
    handle_error("missing table [server]");
}

// get value from the table
auto host = server->getValue("host");
if (!host) {
    handle_error("missing host entry");
}
auto port = server.getValue("port");
if (!port) {
   handle_error("missing port entry");
}

// examine the values
auto hostpair = host->toString();
if (hostpair.first)
   cout << "server.host is " << hostpair.second << "\n";

auto portpair = port->toInt();
if (portpair.first)
   cout << "server.port is " << portpair.second << "\n";

```