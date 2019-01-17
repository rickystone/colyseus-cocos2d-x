<div align="center">
  <a href="https://github.com/gamestdio/colyseus">
    <img src="https://github.com/gamestdio/colyseus/blob/master/media/header.png?raw=true" />
  </a>
  <br>
  <br>
  <a href="https://npmjs.com/package/colyseus">
    <img src="https://img.shields.io/npm/dm/colyseus.svg?style=for-the-badge">
  </a>
  <a href="https://patreon.com/endel" title="Donate to this project using Patreon">
    <img src="https://img.shields.io/badge/patreon-donate-yellow.svg?style=for-the-badge" alt="Patreon donate button" />
  </a>
  <a href="http://discuss.colyseus.io" title="Discuss on Forum">
    <img src="https://img.shields.io/badge/discuss-on%20forum-brightgreen.svg?style=for-the-badge&colorB=b400ff" alt="Discussion forum" />
  </a>
  <a href="https://discord.gg/RY8rRS7">
    <img src="https://img.shields.io/discord/525739117951320081.svg?style=for-the-badge">
  </a>
  <h3>
     Multiplayer Game Client for <a href="https://github.com/cocos2d/cocos2d-x">Cocos2d-x</a>. <br /><a href="http://colyseus.io/docs/">View documentation</a>
  <h3>
</div>

- [Usage](#installation)
- [Installation](#installation)
- [Example](#example)

## Usage

Add the `Source/` directory to your project's `Header Search Paths`.

Below you can see how to use the `Client`, matchmake into a `Room`, and send and
receive messages from the connected room.

```cpp
#include "Colyseus/Client.h";

Client* client;
Room* room;

bool HelloWorld::init()
{
    client = new Client("ws://localhost:2667");
    client->onOpen = CC_CALLBACK_0(HelloWorld::onConnectToServer, this);
    client->connect();
}

void HelloWorld::onConnectToServer()
{
    log("Colyseus: CONNECTED TO SERVER!");
    room->onMessage = CC_CALLBACK_2(HelloWorld::onRoomMessage, this);
    room->onStateChange = CC_CALLBACK_1(HelloWorld::onRoomStateChange, this);

    room->listen("players/:id", [](std::vector<std::string> path, msgpack::object change) -> void {
        std::cout << "players/:id ADDED!" << std::endl;
    });
}

void HelloWorld::onRoomMessage(Room* sender, msgpack::object message)
{
    std::cout << "!! HelloWorld::onRoomMessage !!" << std::endl;
    std::cout << message << std::endl;
}

void HelloWorld::onRoomStateChange(Room* sender)
{
    std::cout << "!! HelloWorld::onRoomStateChange !!" << std::endl;
    std::cout << sender->data->get() << std::endl;
}
```


## Installation

Download and following [installation instructions](https://github.com/cocos2d/cocos2d-x#download-stable-versions) for [Cocos2d-X](http://www.cocos2d-x.org/download).

## Example

The example is using [colyseus-examples](https://github.com/colyseus/colyseus-examples) as a server. Please follow installation instructions from [colyseus-examples](https://github.com/colyseus/colyseus-examples)'s README.

### Running the client

From the `Example` directory, run the `cocos run -p {platform-id}` command,
e.g.:

```
# running on windows
cocos run -p win32
```

```
# running on mac
cocos run -p mac
```

## Contributors

Big thanks to [Hung Hoang](https://github.com/chunho32) for making the initial
implementation of this client.

## License

MIT
