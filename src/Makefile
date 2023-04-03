XBUILD=/Applications/Xcode_14.2.0.app/Contents/Developer/usr/bin/xcodebuild
ROOT=$(PWD)
PROJECT_ROOT=$(ROOT)/../external/SlackTextViewController
PROJECT=$(PROJECT_ROOT)/SlackTextViewController/SlackTextViewController.xcodeproj
BUILD_ROOT=$(PROJECT_ROOT)/build
TARGET_IOS="SlackTextViewController"
PROJECTNAME=SlackTextViewController
OUTPUT=$(ROOT)/Build
FRAMEWORK_PATH=Products/Library/Frameworks/SlackTextViewController.framework
FRAMEWORKS=$(ROOT)/Frameworks
FRAMEWORKS_IOS=$(ROOT)/Frameworks/ios
BINDING_OUTPUT=$(ROOT)/../bindings
IOS_SDK=iphoneos16.2
HEADERS=$(FRAMEWORKS)/$(PROJECTNAME).xcframework/ios-arm64/SlackTextViewController.framework/Headers

all: iossimulator iosdevice maccatalyst xcframework dotnet

build: iossimulator iosdevice maccatalyst xcframework

pack: build nuget

iossimulator:
	$(XBUILD) archive ONLY_ACTIVE_ARCH=NO -project $(PROJECT) -scheme $(TARGET_IOS) -destination "generic/platform=iOS Simulator" -archivePath $(BUILD_ROOT)/simulator.xcarchive -sdk iphonesimulator SKIP_INSTALL=NO BUILD_LIBRARY_FOR_DISTRIBUTION=YES

iosdevice:
	$(XBUILD) archive ONLY_ACTIVE_ARCH=NO -project $(PROJECT) -scheme $(TARGET_IOS) -destination "generic/platform=iOS" -archivePath $(BUILD_ROOT)/iOS.xcarchive -sdk iphoneos SKIP_INSTALL=NO BUILD_LIBRARY_FOR_DISTRIBUTION=YES

maccatalyst:
	$(XBUILD) archive ONLY_ACTIVE_ARCH=NO -project $(PROJECT) -scheme $(TARGET_IOS) -destination "generic/platform=macOS,variant=Mac Catalyst,name=Any Mac" -archivePath $(BUILD_ROOT)/catalyst.xcarchive SKIP_INSTALL=NO BUILD_LIBRARY_FOR_DISTRIBUTION=YES

xcframework:
	rm -rf $(BUILD_ROOT)/SlackTextViewController.xcframework
	$(XBUILD) -create-xcframework -framework $(BUILD_ROOT)/simulator.xcarchive/$(FRAMEWORK_PATH) -framework $(BUILD_ROOT)/iOS.xcarchive/$(FRAMEWORK_PATH) -framework $(BUILD_ROOT)/catalyst.xcarchive/$(FRAMEWORK_PATH) -output $(BUILD_ROOT)/SlackTextViewController.xcframework
	rm -rf $(FRAMEWORKS)
	mkdir -p $(FRAMEWORKS)
	cp -R $(BUILD_ROOT)/ $(FRAMEWORKS)

dotnet:
	dotnet build $(ROOT)/Drastic.SlackTextViewController/Drastic.SlackTextViewController.csproj -c Release

nuget:
	dotnet pack $(ROOT)/Drastic.SlackTextViewController/Drastic.SlackTextViewController.csproj --configuration Release --output $(ROOT)/../nuget

sharpie:
	sharpie bind --sdk=$(IOS_SDK) --output="$(BINDING_OUTPUT)" --namespace="Drastic.$(PROJECTNAME)" --scope="$(HEADERS)/" $(HEADERS)/*.h