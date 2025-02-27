# Ollama android image

## set environment envirment
```shell
export GOOS=android
export GOARCH=arm64
export CGO_ENABLED=1

export TOOLCHAIN=/home/lin/android/android-ndk-r28/toolchains/llvm/prebuilt/linux-x86_64
export TARGET=aarch64-linux-android
export API=30

export CC=$TOOLCHAIN/bin/$TARGET$API-clang
export CXX=$TOOLCHAIN/bin/$TARGET$API-clang++
export AR=$TOOLCHAIN/bin/llvm-ar
export LD=$TOOLCHAIN/bin/ld
export GO111MODULE=on

export CGO_CFLAGS="-I$TOOLCHAIN/sysroot/usr/include  -static  -D__ANDROID__ -D__clang__"
export CGO_CXXFLAGS="-static -std=c++17 -stdlib=libc++"
export CGO_LDFLAGS="-L/home/lin/android/android-ndk-r28/toolchains/llvm/prebuilt/linux-x86_64/sysroot/usr/lib/aarch64-linux-android/30 -ldl"

echo "
// +build android

package main
" > platform_android.go

go generate ./...

go build -mod=mod -x -a  -tags="android cgo" -ldflags="-s -w" -o ollama-android
```
