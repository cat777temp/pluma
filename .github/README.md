# GitHub Actions 工作流

本目录包含Pluma项目的GitHub Actions工作流配置文件，用于自动化构建和发布过程。

## 工作流文件

1. **cmake-multi-platform.yml**
   - 跨平台构建工作流
   - 在Windows和Linux上使用不同的编译器构建项目
   - 自动打包构建产物并上传

2. **create-source-package.yml**
   - 创建源代码包的工作流
   - 生成ZIP和TAR.GZ格式的源代码包

## 触发方式

这些工作流可以通过以下方式触发：

1. **推送到main分支**：
   - 自动触发跨平台构建
   - 构建产物可在Actions页面下载

2. **创建Pull Request**：
   - 自动触发跨平台构建
   - 用于验证PR不会破坏构建

3. **创建Release**：
   - 自动触发跨平台构建和源代码包创建
   - 构建产物和源代码包会自动上传到Release页面

4. **手动触发**：
   - 在GitHub Actions页面可以手动触发任何工作流
   - 适用于需要临时构建的情况

## 发布流程

要发布新版本，请按照以下步骤操作：

1. 确保所有更改已合并到main分支
2. 在GitHub上创建新的Release
   - 点击"Releases" > "Draft a new release"
   - 创建新标签（如v1.0.0）
   - 填写发布说明
   - 点击"Publish release"
3. 工作流会自动运行并上传构建产物到Release页面

## 构建产物

自动构建会生成以下产物：

1. **Windows构建**：
   - pluma-Release-cl-windows.zip

2. **Linux构建**：
   - pluma-Release-gcc-linux.tar.gz
   - pluma-Release-clang-linux.tar.gz

3. **源代码包**：
   - pluma-source-[版本].zip
   - pluma-source-[版本].tar.gz
