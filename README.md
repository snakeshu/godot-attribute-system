# Godot 属性系统说明文档

## 系统概述
这是一个基于Godot 4.x的属性管理系统，参考了虚幻引擎的GAS (Gameplay Ability System)设计理念。系统由三个核心模块组成：

- `Attribute.gd`：基础属性模块，负责属性值的管理和计算
- `AttributeModifier.gd`：属性修改器模块，提供各种属性修改方式
- `AttributeSet.gd`：属性管理器模块，统一管理多个属性

## 功能特性

1. **基础属性管理**
   - 支持基础属性值设置和获取
   - 提供属性值变化信号通知
   - 支持属性间的依赖关系

2. **属性修改器**
   - 支持多种运算类型：ADD（加法）、SUB（减法）、MULT（乘法）等
   - 支持临时修改器（带持续时间）
   - 支持永久修改器
   - 支持DOT效果（持续性伤害/治疗）

3. **DOT系统**
   - 支持设置总伤害量和持续时间
   - 可自定义DOT间隔
   - 自动处理DOT效果的生命周期
   - 支持实时显示DOT效果状态

## 使用方法

### 1. 基础设置

```gdscript
# 创建属性组件
var attribute_component = AttributeComponent.new()

# 创建属性集
var attribute_set = AttributeSet.new()

# 创建属性
var health = Attribute.new()
health.attribute_name = "Health"
health.base_value = 100.0

# 设置属性集
var attrs: Array[Attribute] = []
attrs.append(health)
attribute_set.attributes = attrs

# 设置到组件
attribute_component.attribute_set = attribute_set
```

### 2. 添加修改器

```gdscript
# 添加临时buff
func add_health_buff():
    var health_buff = AttributeModifier.new()
    health_buff.type = AttributeModifier.OperationType.ADD
    health_buff.value = 20.0
    health_buff.set_duration(5.0)  # 持续5秒
    
    var health = attribute_component.find_attribute("Health")
    health.apply_modifier(health_buff)
```

### 3. 使用DOT效果

```gdscript
# 添加DOT效果（例如：持续伤害）
func add_poison_dot():
    var poison_dot = AttributeModifier.new()
    poison_dot.set_dot(50.0, 5.0, 0.5)  # 总伤害50，持续5秒，每0.5秒一次
    
    var health = attribute_component.find_attribute("Health")
    health.apply_modifier(poison_dot)
```

## 安装方法

1. 下载或克隆本仓库
2. 将 `script` 目录下的三个核心文件复制到你的项目中：
   - Attribute.gd
   - AttributeModifier.gd
   - AttributeSet.gd

## 示例场景

项目包含了一个完整的示例场景，展示了：
- 基础属性系统的使用
- 临时buff的添加和移除
- DOT效果的实现
- UI显示和更新

## 注意事项

1. 确保Godot版本为4.x或更高
2. 属性修改器的运算顺序会影响最终结果
3. DOT效果会在指定时间内持续造成伤害，且伤害是永久性的

## 许可证

MIT License

## 贡献

欢迎提交Issue和Pull Request！

[查看完整文档](docs/README.md)