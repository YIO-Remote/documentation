There's a websocket based API for communicating with the remote, running on port 946(YIO). Mainly used by the web configurator tool and the YIO Dock. 

The API automatically starts on power up and is always running in the background. The API is stopped, before wifi is turned off and started, after wifi is turned on.

Messages are sent in JSON format.

### Using the API
You can access the API from the QML side:

`api.start();`


or from C++ side:
- include the header file:
`#include "yioapi.h`
- call a function 
`YioAPI::getInstance()->start();` 

### Authentication
When a client connects, the remote sends a JSON message:

      {
      "type":"auth_required"
      }

The client needs to respond with:

      {
      "type":"auth",
      "token": "<token>"
      }

If you don't provide a token or the token is invalid, you get an error message:

      {
      "type":"auth",
      "message": "Invalid token"
      }

or

      {
      "type":"auth",
      "message": "Token needed"
      }

After successful authentication the client is flagged to be able to communicate with the API.

### Messages
When a message arrives from a flagged client, it gets emitted via the `messageReceived` signal. You can connect to this signal from QML or C++ to further process the message.

### Sending messages
You can send messages to all authenticated clients with the `sendMessage(QString msg)` method. The message should be stringified JSON.
