cmake_minimum_required(VERSION 3.6)
project(protocol)

set(SRC
	dns_parser.c
	DnsMessage.cc
	DnsUtil.cc
	SSLWrapper.cc
	http_parser.c
	HttpMessage.cc
	HttpUtil.cc
)

if (MYSQL STREQUAL "y")
	set(SRC
		${SRC}
		mysql_stream.c
		mysql_parser.c
		mysql_byteorder.c
		MySQLMessage.cc
		MySQLResult.cc
	)
endif ()

if (REDIS STREQUAL "y")
	set(SRC
		${SRC}
		redis_parser.c
		RedisMessage.cc
	)
endif ()

add_library(${PROJECT_NAME} OBJECT ${SRC})

if (KAFKA STREQUAL "y")
	set(SRC
		kafka_parser.c
		KafkaMessage.cc
		KafkaDataTypes.cc
		KafkaResult.cc
	)
	add_library("protocol_kafka" OBJECT ${SRC})
	set_property(SOURCE KafkaMessage.cc APPEND PROPERTY COMPILE_OPTIONS "-fno-rtti")
	set_property(SOURCE KafkaDataTypes.cc APPEND PROPERTY COMPILE_OPTIONS "-fno-rtti")
	set_property(SOURCE KafkaResult.cc APPEND PROPERTY COMPILE_OPTIONS "-fno-rtti")
endif ()
