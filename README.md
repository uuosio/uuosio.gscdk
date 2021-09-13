Go Smart Contracts Development Kit

# Quick Start

<h3>
  <a
    target="_blank"
    href="https://mybinder.org/v2/gh/uuosio/uuosio.gscdk/main?filepath=quickstart/quickstart.ipynb"
  >
    Quick Start
    <img alt="Binder" valign="bottom" height="25px"
    src="https://mybinder.org/badge_logo.svg"
    />
  </a>
</h3>

# What the Go Smart Contracts look like?

Here is an example

```go
package main

import (
	"github.com/uuosio/chain"
	"github.com/uuosio/chain/logger"
)

//table mytable
type MyData struct {
	primary uint64 //primary: t.primary
	name    string
}

//contract mycontract
type MyContract struct {
	Receiver      chain.Name
	FirstReceiver chain.Name
	Action        chain.Name
}

func NewContract(receiver, firstReceiver, action chain.Name) *MyContract {
	return &MyContract{receiver, firstReceiver, action}
}

//action sayhello
func (c *MyContract) SayHello(name string) {
	code := c.Receiver
	scope := code
	payer := c.Receiver
	mydb := NewMyDataDB(code, scope)
	primary := uint64(111)
	it, data := mydb.Get(primary)
	if !it.IsOk() {
		logger.Println("Welcome new friend", name)
		data := &MyData{primary, name}
		mydb.Store(data, payer)
	} else {
		if data.name != name {
			logger.Println("Welcome new friend:", name)
		} else {
			logger.Println("Welcome old friend", name)
		}
		data.name = name
		mydb.Update(it, data, payer)
	}
}
```

# Build Go Smart Contracts Compiler

Follow the steps in [Building](./BUILDING.md)

That will build tinygo command in compiler/build directory that support build Go Smart Contracts.

#### Set PATH

```bash
export PATH=$(pwd)/compiler/build:$PATH
```

# Install from Pre-built Binary

First download binary file from [releases](https://github.com/uuosio/uuosio.gscdk/releases)

For tar.gz file

```bash
tar -C /usr/local -xzf uuosio.gscdk-linux-0.1.0.tar.gz
export PATH=/usr/local/uuosio.gscdk/bin:$PATH
```

Install debian package directly on Ubuntu platform

```bash
sudo apt install ./uuosio.gscdk-linux-0.1.0.deb
```

# eosio-go Command

All commands based on examples/hello example

First cd to hello directory:

```bash
cd examples/hello
```

#### Building

```bash
eosio-go build -o hello.wasm .
```

#### Generating ABI and Extra Code for Smart Contracts

```
eosio-go gencode
```

Code generation is also the default option for "build" command


#### Disable Code Generation during Building

```bash
eosio-go build -gen-code=false -o hello .
```

#### Disable Striping WASM File after Building

```bash
eosio-go build -strip=false -o hello .
```
