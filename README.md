# simpledb

## Architecture

This project has a C/C architecture. The C++ server is responsible for parsing and executing SQL, and the Python client is responsible for processing user input. The network transmission between the two is throguh (gRPC).

The various parts are:

* `SimpleDB`(Dependent library): DBMS, responsible for databse management,  SQL parsing and execution

* `SQLParser`(Library dependent): Parse SQL, and generate AST

* `SimpleDBService`(Dependent library): gRPC library, used to support network transmission

* `SimpleDBServer`(Binary program): Server, handles `SimpleDB` client requests and returns result by 

* `SimpleDBClient`(Binary program): Client, processes user requests, sends SQL statement to the server, and displays the returned result

* `SImpleDBTest`(Unit Test): `SimpleDB` Test for

The dependencies of each part and the third-party libraries they use are as follows:


## Compiler and run

> The test and compilation of macOS 13.0 and Ubuntu 22.04 passes, but other environments have not been tested.

### Environment confirguration

COmpiled using (Bazel) and clang.

**For macOS:**

Install Xcode command line tools:

```
xcode-select --install
```

Install Bazel:

```
brew install bazel
```

**For Ubuntu:**

Install Bazel 6.0.0

```
sudo apt install apt-transport-https curl gnupg -y
curl -fsSL https://bazel.build/bazel-release.pub.gpg | gpg -- dearmon >bazel-archive-ketring.gpg
sudo mx bazel-archive-keyring.gpg /usr/share/keyrings
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/bazel-archive-keyring.gpg] https://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
sudo apt update && sudo apt install bazel-6.0.0
```

Install clang and lld:

```
sudo apt install clang lld
```

### run

Run the server:

```
bazel run -- :simpledb_server --dir=<data_directory> [--debug | --verbose] [--addr=<listening_address>]
```

Run the interactive client:

```
bazel run -- :simpledb_client --server=<addr> 
```

Import data from CSV:

```
bazel run -- :simpledb_client --server=<addr> -- csv=<csv_file> --db=<db_name> --table=<table_name>
```

Run unit tests:

```
bazel test :test_all
```

Compile all targets:

```
bazel build //...
```

After compilation, run this command to generate `compile_commands.json`:

```
bazel run @hedron_compile_commands//:refresh_all
```