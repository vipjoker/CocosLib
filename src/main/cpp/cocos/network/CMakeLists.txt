

  set(PLATFORM_LINK websockets)

  set(COCOS_NETWORK_PLATFORM_SRC network/CCDownloader-android.cpp)


  set(COCOS_NETWORK_SRC
          ${COCOS_NETWORK_PLATFORM_SRC}
          network/HttpClient.cpp
          network/SocketIO.cpp
          network/WebSocket.cpp
          network/CCDownloader.cpp
          network/Uri.cpp
          )



set(COCOS_NETWORK_LINK curl ${PLATFORM_LINK})
