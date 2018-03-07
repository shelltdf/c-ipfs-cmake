PROJECT(c-ipfs-cmake)
SET(PROJECT_NAME c-ipfs-cmake)

#input SETUP_TARGET_NAME
#input SETUP_SOURCES
#input SETUP_HEADERS
MACRO(SETUP_LIBRARY)

# IF(BUILD_STATIC_LIBRAY)
    # ADD_DEFINITIONS(-DEGG_LIBRARY_STATIC)
    ADD_LIBRARY(${SETUP_TARGET_NAME} 
        STATIC ${SETUP_HEADERS} ${SETUP_SOURCES})
# ELSE(BUILD_STATIC_LIBRAY)
    # ADD_LIBRARY(${NIUBI_SETUP_TARGET_NAME} SHARED ${NIUBI_SETUP_HEADERS} ${NIUBI_SETUP_SOURCES})
# ENDIF(BUILD_STATIC_LIBRAY)

ENDMACRO(SETUP_LIBRARY)


#input SETUP_TARGET_NAME
#input SETUP_SOURCES
#input SETUP_HEADERS
MACRO(NIUBI_SETUP_EXECUTABLE)

    ADD_EXECUTABLE(${SETUP_TARGET_NAME} ${SETUP_HEADERS} ${SETUP_SOURCES})

ENDMACRO(NIUBI_SETUP_EXECUTABLE)



# ADD_SUBDIRECTORY(c-protobuf)
# ADD_SUBDIRECTORY(c-multihash)
# ADD_SUBDIRECTORY(c-multiaddr)
# ADD_SUBDIRECTORY(c-libp2p)
# ADD_SUBDIRECTORY(c-ipfs)
    
#c-protobuf
SET(SETUP_TARGET_NAME c-protobuf)
SET(SETUP_HEADERS
    c-protobuf/protobuf.h
)
SET(SETUP_SOURCES
    c-protobuf/protobuf.c
)
SETUP_LIBRARY()


#c-multihash
SET(SETUP_TARGET_NAME c-multihash)
SET(SETUP_HEADERS
    c-multihash/include/mh/multihash.h
)
SET(SETUP_SOURCES
    c-multihash/src/multihash.c
)
INCLUDE_DIRECTORIES(c-multihash/include)
SETUP_LIBRARY()


#c-multiaddr
SET(SETUP_TARGET_NAME c-multiaddr)
SET(SETUP_HEADERS
    c-multiaddr/include/multiaddr/multiaddr.h
)
SET(SETUP_SOURCES
    c-multiaddr/multiaddr.c
)
INCLUDE_DIRECTORIES(c-multiaddr/include)
SETUP_LIBRARY()



#c-libp2p
SET(SETUP_TARGET_NAME c-libp2p)
SET(SETUP_HEADERS
    # c-libp2p/include/
)
SET(SETUP_SOURCES
    # c-libp2p/multiaddr.c
)
file(GLOB_RECURSE SETUP_SOURCES c-libp2p "*.c")
INCLUDE_DIRECTORIES(c-libp2p/include)
SETUP_LIBRARY()


#c-ipfs
SET(SETUP_TARGET_NAME c-ipfs)
SET(SETUP_HEADERS
    # c-ipfs/include/
)
SET(SETUP_SOURCES
    c-ipfs/core/api.c
    c-ipfs/core/bootstrap.c
    c-ipfs/core/builder.c
    c-ipfs/core/client_api.c
    c-ipfs/core/daemon.c
    c-ipfs/core/http_request.c
    c-ipfs/core/ipfs_node.c
    # c-ipfs/core/net.c
    c-ipfs/core/null.c
    c-ipfs/core/ping.c
    c-ipfs/core/swarm.c
)

SET (DIRS 
    main blocks cid cmd commands datastore dnslink exchange flatfs 
    importer journal merkledag multibase namesys path pin repo routing 
    thirdparty unixfs util
    )

foreach(loop_var ${DIRS})
    SET(GLOB_SOURCES "")
    file(GLOB_RECURSE GLOB_SOURCES "c-ipfs/${loop_var}/*.c")
    SET(SETUP_SOURCES ${SETUP_SOURCES} ${GLOB_SOURCES})
endforeach(loop_var)

INCLUDE_DIRECTORIES(c-ipfs/include)
INCLUDE_DIRECTORIES(c-protobuf)
INCLUDE_DIRECTORIES(c-multihash/include)
INCLUDE_DIRECTORIES(c-multiaddr/include)
INCLUDE_DIRECTORIES(c-libp2p/include)
NIUBI_SETUP_EXECUTABLE()


