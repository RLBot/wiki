# Supported Languages

RLBot is a server and bots can therefore be created in any language that supports sockets and [flatbuffers](https://flatbuffers.dev/).

To ease development of bots and other common use cases, we have created language interface frameworks that provide handling of the communication with RLBot.

The following list of language interfaces are supported by core developers of the RLBot framework, or have reached a very high level of documentation and support from the community. They are featured on [rlbot.org](https://rlbot.org/).

- [RLBot/Python](https://github.com/RLBot/python-interface)
    - [Video](https://youtu.be/GLqvodQ942A?si=IjvvjKeHhKnwrErM)
    - [Migration details](https://github.com/RLBot/python-interface/wiki/Migration)
- [RLBot/C#](https://github.com/RLBot/csharp-interface)
- [RLBot/Go](https://github.com/RLBot/go-interface)
- [RLBot/Rust](https://github.com/RLBot/rust-interface)
- [RLBot/C++](https://github.com/RLBot/cpp-interface)


## Adding Support for a New Language

The new language must support sockets and [flatbuffers](https://flatbuffers.dev/).

The protocol is described in the [Socket Specification](/v5/framework/sockets-specification).

The flatbuffer schema can be generated by Git submoduling [RLBot/flatbuffers-schema](https://github.com/RLBot/flatbuffers-schema) and invoking the `flatc` binary.
