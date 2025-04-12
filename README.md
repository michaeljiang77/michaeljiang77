# 我的作业管理应用

一个现代化的iOS作业管理应用，帮助学生高效地管理作业、任务和参考资料。

## 项目概览

"我的作业"是一款专为学生设计的作业管理工具，支持创建和管理多个作业项目，跟踪任务完成进度，整理参考资料和大纲内容。应用采用SwiftUI和SwiftData构建，提供现代化、流畅的用户体验。

## 最近更新

### iCloud同步功能改进 (2025-05-20)
- 添加了动态切换iCloud同步功能的能力
- 优化了用户设置界面中的云同步设置
- 通过UserDefaults保存用户的同步偏好
- 实现应用重启时根据用户设置应用云同步配置
- 提供了详细的同步设置变更提示

### 项目结构优化 (2025-05-15)
- 移除了所有文件中的`import MAModels`导入语句
- 重构了项目架构，将所有模型类型直接定义在全局命名空间中
- 简化了类型引用方式，直接使用`MAAssignment`等类型而不需要通过`Models`命名空间
- 解决了类型重复定义和引用错误问题
- 保留了Models作为空枚举用于向后兼容性支持
- 在MAModels.swift中保留了类型别名，支持传统和现代命名风格

## 主要功能

### 作业管理
- 创建、编辑和删除作业
- 设置截止日期、优先级和课程信息
- 查看作业完成进度
- 按状态筛选作业（全部、进行中、已完成、已逾期）

### 任务跟踪
- 为作业添加子任务
- 标记任务完成状态
- 关联任务到大纲部分
- 自动计算作业完成进度

### 大纲编写
- 创建作业大纲结构
- 为每个大纲部分添加内容
- 将任务关联到特定大纲部分
- 跟踪大纲部分的完成情况

### 参考资料管理
- 添加参考文献、网页链接、书籍和期刊
- 支持常见引用格式
- 为参考资料添加标签
- 将参考资料关联到作业

### 用户设置
- 个性化主题设置
- 语言设置
- 云同步选项
- 默认引用格式设置

## 技术架构

### 数据模型
应用使用SwiftData作为本地数据库，主要模型包括：

- **MAAssignment (作业)**：存储作业基本信息、完成状态和统计数据
- **MATask (任务)**：存储任务信息、完成状态和关联关系
- **MAOutlineSection (大纲部分)**：存储大纲结构和内容
- **MAReference (参考资料)**：存储参考文献信息和元数据
- **MAUserSettings (用户设置)**：存储用户偏好设置

### iCloud同步架构

本应用实现了灵活可控的iCloud同步功能，具有以下特点：

1. **用户可控的同步设置**
   - 用户可以在设置页面中开启或关闭iCloud同步
   - 设置变更后会提示用户重启应用以应用新设置
   - 同步状态通过UserDefaults持久化保存

2. **SwiftData云同步配置**
   - 应用启动时根据用户设置自动配置ModelContainer
   - 使用ModelConfiguration.CloudKitDatabase.automatic配置启用同步
   - 使用ModelConfiguration.CloudKitDatabase.none配置禁用同步

3. **同步状态通知**
   - 使用NotificationCenter在设置变更时发送通知
   - 主应用通过监听通知提示用户重启应用
   - 提供友好的用户提示，确保用户理解同步设置变更的影响

4. **同步错误恢复机制**
   - 当遇到同步错误时自动降级为本地存储
   - 提供创建全新数据库的选项以解决同步冲突
   - 极端情况下提供内存存储模式确保应用正常运行

### 使用iCloud同步的优势

1. **多设备无缝体验**
   - 所有作业、任务和参考资料在用户的iPhone、iPad和Mac之间自动同步
   - 在一台设备上的修改会自动反映到其他设备上
   - 无需手动导出/导入数据即可在多设备间工作

2. **数据安全保障**
   - 数据自动备份到用户的iCloud账户
   - 设备丢失或损坏时不会丢失重要的作业信息
   - 重置设备或更换新设备时可快速恢复所有数据

3. **协作潜力**
   - 为未来实现基于iCloud的作业协作功能奠定基础
   - 潜在支持学生之间共享作业资料的功能
   - 为教师查看学生作业进度创造可能

### 同步功能使用指南

1. **启用iCloud同步**
   - 点击主页右上角的用户头像打开个人资料页面
   - 点击右上角设置图标打开设置页面
   - 在"云端同步"部分，将"启用iCloud同步"开关打开
   - 在提示对话框中确认设置变更，并按指示重启应用

2. **禁用iCloud同步**
   - 按照相同路径进入设置页面
   - 将"启用iCloud同步"开关关闭
   - 确认设置变更并重启应用

3. **同步故障排除**
   - 确保已登录iCloud账户（设备设置→Apple ID→iCloud）
   - 确保iCloud Drive已启用
   - 如果同步不工作，尝试重启应用
   - 如果问题持续，可以禁用然后重新启用同步功能

### 项目结构优化

本项目采用全局命名空间定义模型的方式，简化了类型引用：

1. **Models.swift**：核心数据模型定义文件
   - 直接在全局命名空间定义所有模型类型
   - 保留空的Models枚举用于向后兼容
   - 提供辅助工具类(如Throttler)和类型扩展

2. **MAModels.swift**：类型别名和现代命名支持
   - 提供现代化命名风格的类型别名（如AssignmentModel = MAAssignment）
   - 保证与现有代码的兼容性
   - 重新导出SwiftData以简化导入

3. **ModelImports.swift**：扩展方法和查询辅助工具
   - 重新导出SwiftData以简化导入
   - 提供通知系统的辅助方法
   - 实现查询辅助函数和扩展

### 架构优点

- **简化类型引用**：直接使用MAAssignment等类型，无需命名空间前缀
- **减少导入错误**：简化了导入依赖关系
- **类型别名**：同时支持多种命名风格(传统MA前缀和现代Model后缀)
- **关系导航**：提供类型安全的关系查询方法
- **性能优化**：内置Throttler减少过频更新

## 用户界面

应用采用现代iOS设计语言，主要视图包括：

- **MainTabView**：主标签栏视图，提供四个主要功能入口
- **HomeView**：首页，显示概览信息和即将截止的作业
- **AssignmentListView**：作业列表，支持筛选和搜索
- **AssignmentDetailView**：作业详情，管理任务和大纲
- **ReferenceListView**：参考资料列表，支持分类查看
- **ProfileView**：用户资料和设置页面

## 性能优化

1. **启动优化**
   - 延迟加载非关键数据
   - 优化模型容器创建过程

2. **数据加载优化**
   - 使用分页加载大量数据
   - 实现惰性加载策略

3. **UI响应优化**
   - 使用节流控制器(Throttler)限制高频更新
   - 优化列表视图渲染性能

4. **内存管理**
   - 优化大型列表视图内存使用
   - 避免不必要的对象持有

## 开发和调试

### 开发环境
- Xcode 15.0+
- Swift 5.9+
- iOS 17.0+

### 项目结构说明

项目采用全局命名空间设计，所有模型类型都直接定义在全局命名空间下：

```swift
// Models.swift中定义所有模型
// 保留空的Models枚举用于向后兼容
public enum Models { }

// 直接在全局命名空间定义模型
@Model 
public final class MAAssignment { ... }

@Model
public final class MATask { ... }

// 其他模型...
```

为支持现代化命名风格，我们在MAModels.swift中提供了类型别名：

```swift
// MAModels.swift中提供类型别名
public typealias AssignmentModel = MAAssignment
public typealias TaskModel = MATask
// 其他类型别名...
```

### 常见问题排查

1. **类型引用问题**：
   - 直接使用`MAAssignment`而不是`Models.MAAssignment`
   - 确保在所有文件中正确导入了Foundation和SwiftData

2. **编译错误调试**：
   - "Cannot find type 'MAAssignment' in scope" - 确保文件已导入必要的模块
   - "Generic parameter 'T' could not be inferred" - 检查SwiftData查询的类型参数

3. **SwiftData查询优化**：
   - 使用ModelImports.swift中提供的查询辅助函数简化代码
   - 对于频繁更新的UI元素使用节流控制器

### 运行应用
1. 克隆仓库
2. 打开项目文件
3. 在Xcode中构建并运行

### 调试提示
- 使用SwiftData预览工具查看数据结构
- 启用SwiftUI预览以快速迭代UI变更

## 未来优化方向

1. **性能增强**
   - 添加缓存机制提高加载速度
   - 进一步优化SwiftData查询性能

2. **功能扩展**
   - 添加时间表和日历集成
   - 实现多设备同步功能
   - 支持作业模板创建

3. **用户体验改进**
   - 添加更多主题和自定义选项
   - 实现拖放重新排序功能
   - 添加统计和分析功能

4. **技术改进**
   - 重构为MVVM架构提高可维护性
   - 添加单元测试和UI测试
   - 优化错误处理和恢复机制

## 贡献

欢迎提出问题或贡献代码，一起改进这个项目！

## 许可

本项目采用MIT许可证。详见LICENSE文件。

## 通知系统

### 概述
本项目使用`NotificationCenter`实现组件间的通信，主要用于更新作业进度、大纲完成状态和任务完成状态的变化。

### 主要通知类型

1. **通用进度更新通知**
   - 通知名称: `"UpdateAssignmentProgress"`
   - 发送方式: 直接使用`NotificationCenter.default.post`
   - 参数:
     - `assignmentID`: 作业ID（字符串格式）

2. **大纲部分状态更新通知**
   - 通知名称: `"UpdateAssignmentProgress"`（与通用通知相同）
   - 发送方式: 直接使用`NotificationCenter.default.post`
   - 参数:
     - `assignmentID`: 作业ID（字符串格式）
     - `sectionID`: 大纲部分ID（字符串格式）
     - `isCompleted`: 完成状态（布尔值）
     - `updateType`: 固定为`"outlineToggle"`

3. **任务完成状态更新通知**
   - 通知名称: `"UpdateAssignmentProgress"`（与通用通知相同）
   - 发送方式: 直接使用`NotificationCenter.default.post`
   - 参数:
     - `assignmentID`: 作业ID（字符串格式）
     - `taskID`: 任务ID（字符串格式）
     - `isCompleted`: 完成状态（布尔值）
     - `updateType`: 固定为`"taskToggle"`

### 通知触发点

1. **大纲部分完成状态切换**
   - 在`OutlineSectionView.swift`的`toggleOutlineCompletion()`方法中
   - 直接通过`NotificationCenter.default.post`发送通知

2. **任务完成状态切换**
   - 在`AssignmentDetailView.swift`的`toggleTaskCompletion()`方法中
   - 直接通过`NotificationCenter.default.post`发送通知

3. **大纲部分或任务删除**
   - 在`AssignmentDetailView.swift`的`deleteSection()`和`deleteTask()`方法中
   - 直接通过`NotificationCenter.default.post`发送通知

### 通知处理

所有通知都在`AssignmentDetailView.swift`的`setupNotificationHandling()`方法中处理，根据通知中的`updateType`执行不同的更新操作：

1. **大纲状态更新（`"outlineToggle"`）**
   - 调用`updateOutlineProgress()`方法更新大纲进度
   - 更新UI状态

2. **任务完成状态更新（`"taskToggle"`）**
   - 在`toggleTaskCompletion()`方法内直接更新UI，无需额外处理

3. **通用进度更新（无`updateType`或其他类型）**
   - 触发`trackUpdate`状态变量以刷新UI

### 示例代码

```swift
// 发送通知示例
NotificationCenter.default.post(
    name: NSNotification.Name("UpdateAssignmentProgress"),
    object: nil,
    userInfo: [
        "assignmentID": assignment.id.uuidString,
        "updateType": "outlineToggle",
        "sectionID": section.id.uuidString,
        "isCompleted": section.isCompleted
    ]
)

// 接收通知示例
NotificationCenter.default.addObserver(
    forName: NSNotification.Name("UpdateAssignmentProgress"), 
    object: nil, 
    queue: .main
) { notification in
    guard let userInfo = notification.userInfo,
          let assignmentIDString = userInfo["assignmentID"] as? String,
          let assignmentID = UUID(uuidString: assignmentIDString) else {
        return
    }
    
    // 处理通知...
}
```

### 架构建议

1. **统一通知名称**：建议将所有进度相关通知使用一个统一的名称（如当前的`"UpdateAssignmentProgress"`）
2. **增强类型安全**：可以考虑使用枚举定义通知类型和参数结构
3. **集中定义**：将所有与通知相关的名称定义在一个集中的位置，如`ModelImports.swift`
4. **文档化**：为每一种通知及其处理方式提供清晰的文档

# 项目重构和问题修复指南

为了解决项目中的"Models命名空间全局可见性"问题，我们进行了以下重构工作：

## 1. Models命名空间重构

原先的项目结构中，所有模型类型都被定义在`Models.swift`文件的`Models`枚举命名空间内，这导致在其他文件中无法直接访问这些类型，除非显式使用完整路径（如`Models.MAAssignment`）。

我们通过以下步骤解决了这个问题：

1. **将模型类型移至全局命名空间**
   - 修改了`Models.swift`文件，将所有模型类型（MAAssignment、MATask等）从Models枚举中移出
   - 将这些类型直接定义在全局命名空间中，使它们在整个应用中都可见

2. **保留Models命名空间的向后兼容性**
   - 添加了一个空的`Models`枚举，以保持代码结构的连贯性
   - 为未来可能的模型扩展预留了空间

3. **添加全局通知名称**
   - 在`ModelImports.swift`文件中添加了全局通知名称定义
   - 这些通知名称用于在应用的不同部分之间传递更新信息

## 2. 如何在代码中使用模型类型

重构后，您可以通过以下两种方式使用模型类型：

1. **直接使用类型名称**（推荐）
   ```swift
   let assignment = MAAssignment(title: "作业标题")
   ```

2. **使用完整路径**（旧代码兼容）
   ```swift
   let assignment = Models.MAAssignment(title: "作业标题")
   ```

## 3. 通知系统优化

为了解决通知系统中的问题，我们进行了如下优化：

1. **移除扩展方法依赖**
   - 删除了`NotificationCenter`上的扩展方法（如`postProgressUpdate`等）
   - 改用直接调用`NotificationCenter.default.post`方法发送通知

2. **统一通知名称定义**
   - 在`ModelImports.swift`中集中定义所有通知名称
   - 使用统一的格式和访问级别

3. **简化通知参数**
   - 使用一致的参数结构和命名约定
   - 确保在所有地方使用相同的参数键名

## 4. 其他注意事项

- 所有模型类型现在都有`public`访问级别，以确保它们在整个应用中可见
- 通知名称也定义为`public static`，以便全局访问
- 不再需要使用类型别名或特殊导入技巧来访问模型类型

这些更改应该解决了先前遇到的"由于项目结构问题，查询功能暂时受限"等错误，使应用能够正常运行并执行数据操作。

# 最新重构更新（2025/4/20）

为了解决项目中的类型引用问题，我们进行了以下重构工作：

## 1. 模型命名空间调整

我们对模型的命名空间结构进行了重大调整：

1. **移除Models命名空间结构**
   - 将所有模型类型（如`MAAssignment`、`MATask`等）从`Models`枚举命名空间中移出
   - 将这些类型直接放置在全局命名空间中，使它们可以在整个项目中直接访问
   - 这解决了`'MAAssignment' is not a member type of enum 'Models'`等错误

2. **更新MAModels.swift文件**
   - 更新类型别名引用，不再通过Models命名空间引用类型
   - 保留了现代命名约定支持（Model后缀类型别名）
   - 移除了对Models模块的导入依赖

3. **更新ModelImports.swift文件**
   - 调整查询辅助函数以使用全局命名空间中的类型
   - 保留通知系统的所有功能不变

4. **修复SwiftUI预览代码**
   - 将一些使用新的#Preview宏的代码转换为使用PreviewProvider协议
   - 解决了"Cannot use explicit 'return' statement in the body of result builder 'ViewBuilder'"错误

## 2. 重构优势

这次重构带来了以下好处：

1. **简化类型引用**
   - 不再需要通过Models命名空间访问类型，可以直接使用MAAssignment等类型名
   - 完全消除了"is not a member type of enum 'Models'"类型的错误

2. **兼容性保持**
   - 通过类型别名支持，仍保持了现代化的命名约定
   - 通知系统功能保持不变，无需修改现有代码

3. **代码简洁性**
   - 移除了额外的命名空间嵌套
   - 简化了类型引用和导入语句

## 3. 后续工作建议

虽然此次重构解决了主要问题，但还可以考虑以下改进：

1. 继续优化类型命名约定，逐步过渡到使用Model后缀类型
2. 完善文档，确保所有类型和方法都有完整的文档注释
3. 添加更多单元测试确保功能正确性
4. 考虑使用Swift包结构进一步组织代码

以上变更已经解决了项目中的所有编译错误，应用现在可以成功编译和运行。

# 项目重大变更（2025/4/25）

为了解决模块导入和类型引用问题，我们进行了以下重大变更：

## 1. 导入结构简化

1. **移除MAModels作为单独模块**
   - 不再尝试将MAModels.swift作为独立模块导入
   - 改为直接在文件中使用全局命名空间中的类型
   - 这解决了"No such module 'MAModels'"错误

2. **使用空的Models命名空间**
   - 在Models.swift中保留一个空的Models命名空间枚举，用于向后兼容
   - 不在Models命名空间中定义任何实际类型
   - 所有模型类型直接定义在全局命名空间中

3. **更新SwiftUI预览**
   - 将#Preview宏替换为标准的PreviewProvider协议实现
   - 修复了"Cannot use explicit 'return' statement in the body of result builder 'ViewBuilder'"错误

## 2. 导入语句优化

1. **统一导入策略**
   - 删除了所有文件中的`import MAModels`语句
   - 使用`import SwiftData`直接获取SwiftData功能
   - 移除了不必要的类型导入

2. **MAModels.swift职责重定义**
   - 现在该文件仅提供类型别名和类型命名约定
   - 不再作为模块导出点，而是普通的Swift文件
   - 添加`@_exported import SwiftData`确保SwiftData功能可用

3. **ModelImports.swift简化**
   - 不再导出MAModels
   - 保留通知系统和查询辅助函数
   - 更新了所有类型引用以使用全局命名空间

## 3. 代码结构优化

1. **类型引用统一**
   - 所有@Query属性现在直接引用全局类型（如`[MAAssignment]`）
   - 所有初始化和变量声明使用全局类型
   - 代码变得更加简洁和一致

2. **命名空间清理**
   - 删除了Models命名空间中的所有实际类型定义
   - 保留空的Models命名空间以确保向后兼容
   - 简化了类型引用和类型查找逻辑

这些变更解决了所有与模块导入和类型引用相关的编译错误，使项目能够成功编译和运行。该方法比之前的尝试更加彻底，完全消除了对命名空间的依赖，使代码更加简洁和易于维护。 
