# TCP Chat Server

![Go Version](https://img.shields.io/badge/Go-1.25-00ADD8?style=flat&logo=go)
![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Protocol](https://img.shields.io/badge/Protocol-TCP-green.svg)
![Concurrency](https://img.shields.io/badge/Concurrency-Goroutines-00ADD8.svg)

A lightweight TCP-based chat server written in Go that supports multiple chat rooms and real-time messaging. Clients connect via raw TCP sockets and interact using simple slash commands.

## What It Does

The server listens on port 8888 and handles multiple concurrent connections. Each client gets a nickname and can join rooms to exchange messages with other members. The architecture uses Go channels for command processing and goroutines for concurrent client handling.

## Techniques Used

- **Goroutines for concurrency** - Each client connection runs in its own goroutine, and the server command processor runs independently. [Go concurrency patterns](https://go.dev/blog/pipelines)
- **Channel-based command processing** - Commands from all clients flow through a single channel to a central processor, preventing race conditions
- **Buffer reading with delimiters** - Uses `bufio.NewReader().ReadString('\n')` to read line-delimited input from TCP connections
- **Map-based routing** - Rooms and clients are stored in maps using network addresses as keys for O(1) lookups

## Technologies

This is pure Go standard library code with no external dependencies:

- `net` package for TCP server and connection handling
- `bufio` for buffered I/O operations
- Go 1.25.5 (as specified in [go.mod](go.mod))

## Project Structure

```
ovenpickled-tcp-chat-using-golang/
├── client.go
├── command.go
├── go.mod
├── LICENSE
├── main.go
├── room.go
├── server.go
└── README.md
```

**Core files:**
- [client.go](client.go) - Client connection handler with input parsing
- [server.go](server.go) - Main server logic and command execution
- [room.go](room.go) - Chat room management and message broadcasting
- [command.go](command.go) - Command type definitions and constants
- [main.go](main.go) - Entry point that starts the TCP listener

## Available Commands

- `/nick <name>` - Set your nickname
- `/join <room>` - Join or create a chat room
- `/rooms` - List all active rooms
- `/msg <message>` - Send a message to your current room
- `/quit` - Disconnect from the server

## License

MIT License - see [LICENSE](LICENSE) file for details
