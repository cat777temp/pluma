# 设计模式

Pluma框架在其实现中使用了多种设计模式，以下是主要的设计模式及其应用：

## 工厂模式（Factory Pattern）
- **核心应用**：Provider类作为抽象工厂，定义了创建对象的接口
- **实现方式**：每个插件通过实现特定的Provider子类来创建具体的对象
- **优势**：主程序不需要知道具体实现类，只需通过Provider接口获取对象
- **示例**：WarriorProvider定义了创建Warrior对象的接口，而EagleProvider、JaguarProvider等实现了具体的创建逻辑

## 插件模式（Plugin Pattern）
- **核心应用**：整个Pluma框架本身就是一个插件系统的实现
- **实现方式**：通过动态加载库（DLibrary）和连接器（Connector）实现插件的加载和注册
- **优势**：允许在不修改主程序的情况下扩展功能
- **示例**：主程序通过loadFromFolder方法加载插件，插件通过connect函数注册自己的Provider

## 桥接模式（Bridge Pattern）
- **核心应用**：将抽象（接口）与实现分离，使它们可以独立变化
- **实现方式**：Host类作为桥接器，连接PluginManager和Provider
- **优势**：降低了插件系统各组件之间的耦合度
- **示例**：PluginManager通过Host管理Provider，而不直接操作Provider

## 单一职责原则（Single Responsibility Principle）
- **核心应用**：各个类都有明确的单一职责
- **实现方式**：DLibrary负责动态库加载，Host负责Provider管理，PluginManager负责插件生命周期
- **优势**：代码结构清晰，易于维护和扩展

## 外观模式（Facade Pattern）
- **核心应用**：Pluma类作为整个框架的外观，提供简化的接口
- **实现方式**：Pluma类封装了PluginManager的复杂性，提供简单的API
- **优势**：简化了客户端代码，提高了易用性
- **示例**：客户端只需要与Pluma类交互，而不需要了解内部实现细节

## 组合模式（Composite Pattern）
- **核心应用**：在Provider管理中的应用
- **实现方式**：Host类管理多个Provider，形成树形结构
- **优势**：统一处理单个Provider和Provider集合

## 策略模式（Strategy Pattern）
- **核心应用**：不同的Provider实现代表不同的策略
- **实现方式**：通过Provider接口定义算法族，各Provider实现不同的策略
- **优势**：运行时可以灵活切换不同的实现策略
- **示例**：不同的WarriorProvider提供不同类型的Warrior实现

# 构建系统

Pluma项目使用CMake构建系统，实现了跨平台的编译和构建。以下是构建系统的主要特点：

## 主项目CMakeLists.txt

- **现代CMake实践**：采用现代CMake的目标属性方法，使用target_include_directories和target_link_libraries
- **跨平台兼容**：针对不同平台（Windows、Linux、macOS）设置了相应的编译选项
- **安装规则**：定义了库和头文件的安装规则
- **模块化**：将Pluma库和示例分离为不同的模块

## 示例项目CMakeLists.txt

- **组件化**：将示例分为接口库、主程序和插件三个组件
- **插件管理**：自动将编译好的插件复制到正确的输出目录
- **跨平台兼容**：处理了不同平台上插件文件后缀的差异

## 构建步骤

1. 创建build目录：`mkdir build && cd build`
2. 生成构建文件：`cmake ..`
3. 编译项目：`cmake --build .`
4. 运行示例：`./bin/warrior_host`

重构后的CMake系统提供了更好的可维护性和可扩展性，使得项目更容易在不同平台上构建和运行。