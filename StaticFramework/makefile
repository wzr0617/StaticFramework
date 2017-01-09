XCODEBUILD?=$(shell which xcodebuild)
TARGET_NAME?=StaticFramework
LIB_NAME:=lib${TARGET_NAME}.a
PROJECT_NAME:=${TARGET_NAME}.xcodeproj
BUILD_DIR:=./
CONFIGURATION?=Release
FW_DIR:=${TARGET_NAME}.framework
FW_DIR_RES:=${FW_DIR}/Resources
FW_DIR_HEAD:=${FW_DIR}/Headers
FW_DIR_MOD:=${FW_DIR}/Modules
FW_DIRS:=${FW_DIR_RES} ${FW_DIR_HEAD} ${FW_DIR_MOD}

all: StaticFramework.framework

StaticFramework.framework: libStaticFramework.a module.modulemap
	mkdir -p ${FW_DIRS}
	cp ${TARGET_NAME}/*.h ${FW_DIR_HEAD}/.
	cp ${TARGET_NAME}/Info.plist ${FW_DIR_RES}/.
	cp ${TARGET_NAME}/module.modulemap ${FW_DIR_MOD}/.
	cp $< ${FW_DIR}/${TARGET_NAME}

libStaticFramework.a: libStaticFrameworkPhoneos.a libStaticFrameworkPhonesimulator.a
	lipo -create -output $@ $^
	rm -rf build/ Release-*/

libStaticFrameworkPhoneos.a:
	${XCODEBUILD} -target ${TARGET_NAME} -project ${PROJECT_NAME} -configuration ${CONFIGURATION} -sdk iphoneos -arch armv7 -arch armv7s -arch arm64 clean build ONLY_ACTIVE_ARCH=NO BUILD_DIR="${BUILD_DIR}"
	mv ${BUILD_DIR}/${CONFIGURATION}-iphoneos/${LIB_NAME} $@

libStaticFrameworkPhonesimulator.a:
	${XCODEBUILD} -target ${TARGET_NAME} -project ${PROJECT_NAME} -configuration ${CONFIGURATION} -sdk iphonesimulator -arch i386 -arch x86_64 clean build ONLY_ACTIVE_ARCH=NO BUILD_DIR="${BUILD_DIR}"
	mv ${BUILD_DIR}/${CONFIGURATION}-iphonesimulator/${LIB_NAME} $@

clean:
	rm *.a
	rm -rf *.framework
