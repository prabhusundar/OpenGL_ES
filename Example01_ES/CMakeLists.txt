#
# Example01_ES CMake file
#
# (c) Norbert Nopper
# 

cmake_minimum_required(VERSION 2.6)

project(Example01_ES)

IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	# Linux

	IF(${CMAKE_SYSTEM_PROCESSOR} MATCHES "arm")
		# Raspberry Pi
		# ToDo Add i.MX6
		
		add_definitions(-DGLUS_ES3)
		
		include_directories(${Example01_ES_SOURCE_DIR}/../GLUS/src)		
	ENDIF()
	
ELSEIF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
	# Windows
	
	add_definitions(-D_CRT_SECURE_NO_WARNINGS)
	
	# Desktop OpenGL ES 3.0

	add_definitions(-DGLUS_ES3)
	
	include_directories(C:\\Imagination\\PowerVR_Graphics\\PowerVR_SDK\\SDK_3.4\\Builds\\Include ${Example01_ES_SOURCE_DIR}/../GLUS/src)	
	
	link_directories(C:\\Imagination\\PowerVR_Graphics\\PowerVR_SDK\\SDK_3.4\\Builds\\Windows\\x86_32\\Lib ${Example01_ES_SOURCE_DIR}/../GLUS)
			
ENDIF()

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${Example01_ES_SOURCE_DIR}/../Binaries)

# Source files
file(GLOB C_FILES ${Example01_ES_SOURCE_DIR}/src/*.c)

# Header files
file(GLOB_RECURSE H_FILES ${Example01_ES_SOURCE_DIR}/src/*.h)

add_executable(Example01_ES ${C_FILES} ${H_FILES})
	
IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	# Linux

	IF(${CMAKE_SYSTEM_PROCESSOR} MATCHES "arm")
        link_directories(${Example01_ES_SOURCE_DIR}/../GLUS)
		target_link_libraries(Example01_ES SDL GLUS EGL GLESv2 pthread rt m)
		IF(${SoC} MATCHES "iMX6")
            # i.MX6
            # Everything is in 

        ELSE()
            # Raspberry Pi

            include_directories(/opt/vc/include /opt/vc/include/interface/vcos/pthreads /opt/vc/include/interface/vmcs_host/linux)
			link_directories(/opt/vc/lib)        
           	target_link_libraries(Example01_ES bcm_host vcos vchiq_arm)

        ENDIF()
		
	ENDIF()
	
ELSEIF(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
	# Windows

	target_link_libraries(Example01_ES GLUS libEGL libGLESv2)

	message("Executable is deployed either to Binaries/Release or Binaries/Debug.")
	message("Copy the executable to the Binaries folder.")
	message("CMAKE_RUNTIME_OUTPUT_DIRECTORY is set to Binaries, but Release/Debug is appended.")
				
ENDIF()
