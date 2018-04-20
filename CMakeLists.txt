cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    beldexcoin/external
    beldexcoin/include
    beldexcoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("beldexcoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  beldexcoin/external/miniupnpc/connecthostport.c
  beldexcoin/external/miniupnpc/igd_desc_parse.c
  beldexcoin/external/miniupnpc/minisoap.c
  beldexcoin/external/miniupnpc/miniupnpc.c
  beldexcoin/external/miniupnpc/miniwget.c
  beldexcoin/external/miniupnpc/minixml.c
  beldexcoin/external/miniupnpc/portlistingparse.c
  beldexcoin/external/miniupnpc/receivedata.c
  beldexcoin/external/miniupnpc/upnpcommands.c
  beldexcoin/external/miniupnpc/upnpreplyparse.c
  beldexcoin/src/Common/Base58.cpp
  beldexcoin/src/Common/CommandLine.cpp
  beldexcoin/src/Common/Util.cpp
  beldexcoin/src/Common/StringTools.cpp
  beldexcoin/src/Common/JsonValue.cpp
  beldexcoin/src/Common/ConsoleTools.cpp
  beldexcoin/src/Common/MemoryInputStream.cpp
  beldexcoin/src/Common/PathTools.cpp
  beldexcoin/src/Common/StdInputStream.cpp
  beldexcoin/src/Common/StdOutputStream.cpp
  beldexcoin/src/Common/StreamTools.cpp
  beldexcoin/src/Common/StringOutputStream.cpp
  beldexcoin/src/Common/StringView.cpp
  beldexcoin/src/Common/VectorOutputStream.cpp
  beldexcoin/src/crypto/blake256.c
  beldexcoin/src/crypto/chacha8.c
  beldexcoin/src/crypto/crypto-ops-data.c
  beldexcoin/src/crypto/crypto-ops.c
  beldexcoin/src/crypto/crypto.cpp
  beldexcoin/src/crypto/groestl.c
  beldexcoin/src/crypto/hash-extra-blake.c
  beldexcoin/src/crypto/hash-extra-groestl.c
  beldexcoin/src/crypto/hash-extra-jh.c
  beldexcoin/src/crypto/hash-extra-skein.c
  beldexcoin/src/crypto/hash.c
  beldexcoin/src/crypto/jh.c
  beldexcoin/src/crypto/keccak.c
  beldexcoin/src/crypto/oaes_lib.c
  beldexcoin/src/crypto/random.c
  beldexcoin/src/crypto/skein.c
  beldexcoin/src/crypto/slow-hash.c
  beldexcoin/src/crypto/slow-hash.cpp
  beldexcoin/src/crypto/tree-hash.c
  beldexcoin/src/CryptoNoteCore/BlockchainIndices.cpp
  beldexcoin/src/CryptoNoteCore/BlockchainMessages.cpp
  beldexcoin/src/CryptoNoteCore/BlockIndex.cpp
  beldexcoin/src/CryptoNoteCore/CoreConfig.cpp
  beldexcoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  beldexcoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  beldexcoin/src/CryptoNoteCore/Currency.cpp
  beldexcoin/src/CryptoNoteCore/MinerConfig.cpp
  beldexcoin/src/CryptoNoteCore/Transaction.cpp
  beldexcoin/src/CryptoNoteCore/Account.cpp
  beldexcoin/src/CryptoNoteCore/Blockchain.cpp
  beldexcoin/src/CryptoNoteCore/Checkpoints.cpp
  beldexcoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  beldexcoin/src/CryptoNoteCore/Core.cpp
  beldexcoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  beldexcoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  beldexcoin/src/CryptoNoteCore/Difficulty.cpp
  beldexcoin/src/CryptoNoteCore/IBlock.cpp
  beldexcoin/src/CryptoNoteCore/Miner.cpp
  beldexcoin/src/CryptoNoteCore/TransactionExtra.cpp
  beldexcoin/src/CryptoNoteCore/TransactionPool.cpp
  beldexcoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  beldexcoin/src/CryptoNoteCore/TransactionUtils.cpp
  beldexcoin/src/InProcessNode/InProcessNode.cpp
  beldexcoin/src/InProcessNode/InProcessNodeErrors.cpp
  beldexcoin/src/NodeRpcProxy/NodeErrors.cpp
  beldexcoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  beldexcoin/src/P2p/NetNodeConfig.cpp
  beldexcoin/src/Serialization/BinaryInputStreamSerializer.cpp
  beldexcoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  beldexcoin/src/Serialization/JsonInputValueSerializer.cpp
  beldexcoin/src/Serialization/JsonOutputStreamSerializer.cpp
  beldexcoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  beldexcoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  beldexcoin/src/Serialization/SerializationOverloads.cpp
  beldexcoin/src/Transfers/BlockchainSynchronizer.cpp
  beldexcoin/src/Transfers/SynchronizationState.cpp
  beldexcoin/src/Transfers/TransfersConsumer.cpp
  beldexcoin/src/Transfers/TransfersContainer.cpp
  beldexcoin/src/Transfers/TransfersSubscription.cpp
  beldexcoin/src/Transfers/TransfersSynchronizer.cpp
  beldexcoin/src/Wallet/LegacyKeysImporter.cpp
  beldexcoin/src/Wallet/WalletAsyncContextCounter.cpp
  beldexcoin/src/Wallet/WalletErrors.cpp
  beldexcoin/src/WalletLegacy/KeysStorage.cpp
  beldexcoin/src/WalletLegacy/WalletLegacy.cpp
  beldexcoin/src/WalletLegacy/WalletHelper.cpp
  beldexcoin/src/WalletLegacy/WalletLegacySerializer.cpp
  beldexcoin/src/WalletLegacy/WalletLegacySerialization.cpp
  beldexcoin/src/WalletLegacy/WalletTransactionSender.cpp
  beldexcoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  beldexcoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  beldexcoin/src/System/ContextGroup.cpp
  beldexcoin/src/System/Event.cpp
  beldexcoin/src/System/EventLock.cpp
  beldexcoin/src/System/InterruptedException.cpp
  beldexcoin/src/System/Ipv4Address.cpp
  beldexcoin/src/System/TcpStream.cpp
  beldexcoin/src/HTTP/HttpRequest.cpp
  beldexcoin/src/HTTP/HttpParser.cpp
  beldexcoin/src/HTTP/HttpParserErrorCodes.cpp
  beldexcoin/src/HTTP/HttpResponse.cpp
  beldexcoin/src/Rpc/HttpClient.cpp
  beldexcoin/src/Rpc/JsonRpc.cpp
  beldexcoin/src/P2p/NetNode.cpp
  beldexcoin/src/P2p/LevinProtocol.cpp
  beldexcoin/src/P2p/NetNodeConfig.cpp
  beldexcoin/src/P2p/PeerListManager.cpp
  beldexcoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  beldexcoin/src/Logging/ILogger.cpp
  beldexcoin/src/Logging/LoggerGroup.cpp
  beldexcoin/src/Logging/CommonLogger.cpp
  beldexcoin/src/Logging/LoggerManager.cpp
  beldexcoin/src/Logging/FileLogger.cpp
  beldexcoin/src/Logging/StreamLogger.cpp
  beldexcoin/src/Logging/ConsoleLogger.cpp
  beldexcoin/src/Logging/LoggerMessage.cpp
  beldexcoin/src/Logging/LoggerRef.cpp
  beldexcoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  beldexcoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  beldexcoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(beldexcoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} beldexcoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(beldexcoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES beldexcoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
