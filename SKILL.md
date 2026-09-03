# Lighting Image Director Pro — 灯具高质量视觉生成 Skill

## 适用范围
本 Skill 专用于灯具产品图片的生成、参考图重建、场景图扩展、电商主图、详情页卖点图、官网横幅、画册图片、材质特写、开灯效果、不同色温版本、批量多场景系列图，以及后续高清重绘/放大。

目标不是“生成一张好看的室内图”，而是生成 **产品结构准确、安装合理、光学可信、材质真实、放大后细节尽可能完整、可以用于商业展示的灯具视觉资产**。

---

# 1. 核心优先级
每次执行必须按以下优先级排序，不得为了画面美观牺牲前面的要求：

1. 产品身份与结构准确
2. 安装方式与物理关系合理
3. 尺寸比例与空间尺度可信
4. 光源位置、发光面、照射方向可信
5. 材质、颜色、表面工艺准确
6. 场景与灯具类型匹配
7. 构图、摄影、布光高级
8. 放大后的边缘、纹理与微细节
9. 氛围、美术风格与装饰性

若上述要求发生冲突，以前者为准。

---

# 2. 禁止擅自改变产品
当用户提供产品图、参考图或明确规格时：

- 不得改变灯臂数量
- 不得改变灯罩数量
- 不得改变灯罩形状
- 不得改变灯体几何结构
- 不得改变底盘/吸顶盘形状
- 不得改变吊线、吊杆、支架数量和连接关系
- 不得改变开孔、接缝、转角、装饰件位置
- 不得凭空增加螺丝、按钮、遥控器、传感器、拉绳、灯泡或附件
- 不得把圆形改成椭圆、方形改成梯形、直线改成弯线
- 不得为了透视“自动重设计”产品
- 不得把产品材质替换成更高级但不真实的材质
- 不得增加不存在的品牌标志、文字、认证图标或功能标识

若参考图与文字规格不一致：
- **视觉结构**优先参考用户提供的产品图
- **数值参数/功能**优先参考用户明确提供的文字规格
- 不自行猜测未提供的功能

---
## 单灯唯一性规则（Single Fixture Exclusivity Rule）

当用户只提供一个灯具，或明确要求展示某一个指定灯具时，生成结果中必须遵守以下规则：

1. 画面中有且只有一个灯具主体（one and only one lighting fixture）。
2. 只能出现用户指定的那一盏灯，不得额外生成任何其他灯具。
3. 禁止出现其他可识别照明设备，包括但不限于：
   - 台灯
   - 壁灯
   - 落地灯
   - 吊灯
   - 第二个吸顶灯
   - 射灯
   - 筒灯
   - 轨道灯
   - 灯带
   - 镜前灯
   - 床头灯
   - 装饰小夜灯
   - 户外辅助灯
4. 如果场景是卧室、客厅、餐厅、书房等室内空间，允许出现正常家具与装饰，但不得出现第二个灯具作为配景。
5. 空间氛围可以通过自然采光、窗外光、环境反射光、主灯发光效果来营造，不要通过添加其他灯具来补氛围。
6. 如果场景中常规上容易出现配套灯具（例如卧室床头台灯、客厅落地灯、餐厅壁灯），也必须省略，不得生成。
7. 若构图中出现镜子、玻璃、反射面，需避免因反射造成“看起来像第二盏灯”的效果。
8. 不允许为了画面平衡自动补出成对灯具、双灯、对称灯或重复灯。
9. 如果用户没有特别说明“多灯组合”或“整套空间照明”，默认按单灯唯一性规则执行。

# 3. 每次任务先建立 Product Lock
在生成之前，内部建立不可变的 Product Lock：

- fixture_type
- silhouette
- outer_dimensions_ratio
- chassis_shape
- chassis_color
- arm_count
- shade_count
- shade_shape
- arm_path_geometry
- hanging_method
- wall_or_ceiling_contact_shape
- visible_materials
- surface_finish
- light_emitting_area
- light_direction
- cable_or_rod_count
- remote_or_accessories_if_visible
- dominant_color
- symmetry_type

所有后续场景、角度、光色变化都不得破坏 Product Lock。

---

# 4. 输入解析
从用户输入中尽可能提取：

## 产品
- 品牌
- 灯具类型
- SKU/型号
- 主体颜色
- 底盘颜色
- 材质
- 表面工艺
- 尺寸
- 重量
- 灯臂/灯头/灯罩数量
- 灯泡接口
- 内置LED或可更换光源
- 功率
- 流明
- 色温
- 调光方式
- 遥控器/APP/墙控
- IP等级
- 安装环境

## 图片任务
- 主图/场景图/细节图/横幅/信息图/氛围图
- 比例
- 是否点亮
- 光色
- 是否需要人物
- 是否需要保留文字
- 是否需要品牌元素
- 图片数量
- 是否要求多个场景不重复

缺失信息不要阻塞任务。依据灯具类型采用保守、通用且物理合理的默认值。

---

# 5. 灯具分类路由
优先匹配以下类型，并加载对应模块：

- flush_mount_ceiling_light 吸顶灯
- semi_flush_ceiling_light 半吸顶灯
- pendant_light 单头/多头吊灯
- chandelier 枝形吊灯/水晶吊灯/Sputnik
- linear_pendant 线性餐吊灯/岛台灯
- wall_sconce 室内壁灯
- vanity_light 镜前灯/浴室灯
- picture_light 画灯
- bedside_wall_light 床头壁灯
- table_lamp 台灯
- desk_lamp 工作台灯
- floor_lamp 落地灯
- ceiling_fan_light 风扇灯
- track_light 轨道灯
- spotlight 射灯
- recessed_downlight 筒灯/嵌灯
- under_cabinet_light 橱柜灯
- outdoor_wall_light 户外壁灯
- porch_light 门廊灯
- garden_light 庭院灯
- bollard_light 草坪/柱灯
- pathway_light 步道灯
- solar_light 太阳能灯
- floodlight 泛光灯
- mirror_integrated_light 镜灯一体
- decorative_neon_or_strip 装饰灯带/氛围灯

详细逻辑见 `modules/02_fixture_taxonomy.md`。

---

# 6. 场景自动匹配规则
如果用户没有指定空间，按“最符合购买者预期 + 最能表现灯具功能”的原则选择。

例如：
- 大尺寸主吸顶灯 → 客厅、主卧、餐厅
- 小尺寸吸顶灯 → 玄关、走廊、书房、次卧
- 长条吊灯 → 餐桌、岛台
- Sputnik → 餐厅、客厅、挑空区
- 床头壁灯 → 卧室床头、阅读区
- 镜前灯 → 浴室镜柜
- IP65户外壁灯 → 门廊、庭院墙、车库入口
- 台灯 → 书桌、床头柜、边几
- 落地灯 → 沙发边、阅读角
- 风扇灯 → 卧室、客厅、书房

场景必须避免明显不合理：如巨大吊灯放低矮狭小走廊、室内IP20灯放在淋雨区域、镜前灯装在天花中央等。

---

# 7. 安装合理性
安装位置必须符合以下原则：

- 顶装灯与天花板接触面完整、无悬空
- 吸顶底盘应贴合天花，不出现明显不合理缝隙
- 吊灯吊线/吊杆垂直向下，长度一致或符合设计
- 多灯头吊灯的悬挂点应受力合理
- 壁灯背板贴墙，灯体与墙面关系准确
- 线性壁灯在用户要求时必须严格水平
- 镜前灯应和镜面/台盆中轴关系协调
- 户外壁灯应安装于真实外墙，不悬浮
- 台灯底座完整落在桌面
- 落地灯底座完整接触地面
- 风扇灯安装轴垂直，扇叶不穿过天花/家具
- 轨道灯必须连接于轨道
- 嵌入式筒灯与开孔边缘贴合

安装逻辑详见 `modules/04_installation_logic.md`。

---

# 8. 真实尺度与比例
禁止“场景漂亮但产品尺寸完全失真”。

若用户提供尺寸：以尺寸为主要尺度约束。
若未提供尺寸：根据灯具类别使用合理视觉尺度，但不要写出猜测的具体厘米数。

需要保持：
- 灯具与房间体量合理
- 吊灯与餐桌尺度合理
- 壁灯与门、床头柜、镜子尺度合理
- 台灯与桌面尺度合理
- 户外灯与门洞、砖墙尺度合理
- 不能因广角镜头让灯具近大远小到严重失真

---

# 9. 光学表现
发光产品不是“白色发光贴图”。必须同时表现：

- 发光面内部亮度渐变
- 灯罩/扩散罩本身材质
- 光源周围适量 bloom，但不能吞噬边缘
- 真实的墙面/天花/桌面照度变化
- 阴影方向与灯具照明方向一致
- 3000K、4500K、6500K需体现色温差异，但不把整个房间强行染成单一颜色
- 高亮区域保留层次，不 clipping
- 暗部保留细节，不死黑

光学细节见 `modules/07_lighting_optics.md`。

---

# 10. 高清细节策略
“8K、16K、100MP”只作为风格提示，不能替代真实细节约束。真正需要强调：

- sharp geometric boundaries
- resolved micro-textures
- edge integrity
- material micro-contrast
- fine seams and joints
- realistic surface roughness
- non-smeared reflections
- clean anti-aliased contours
- low noise
- no texture mush
- no painterly interpolation
- clear small components

若图片用于后续放大，生成阶段优先：
- 较深景深
- 主体完整对焦
- 避免强烈运动模糊
- 避免过度雾化
- 避免极端浅景深
- 避免强 bloom

---

# 11. 摄影语言
默认视觉方向为“真实商业摄影”，而不是渲染图。

可使用摄影风格词：
- high-end commercial product photography
- premium architectural interior photography
- medium-format look
- Hasselblad X2D 100C look
- natural color science
- high dynamic range
- controlled studio lighting
- realistic optical rendering
- low ISO appearance
- RAW-quality tonal range

注意：相机型号仅用于视觉风格提示，不代表真实输出分辨率或真实拍摄。

---

# 12. 构图模式
根据任务自动选择：

## Hero 产品主视觉
- 产品清晰完整
- 视觉重心明确
- 周围留白可控
- 不裁切关键结构

## Interior 场景
- 产品优先级高于家具
- 环境用于建立尺度和使用情境
- 避免杂物和抢眼装饰

## Detail 特写
- 聚焦材质、接口、灯罩、边缘、表面工艺
- 仍需保持产品真实结构

## Banner 横幅
- 预留文案安全区
- 灯具不贴边
- 视觉动线服务于广告信息

## 1:1 电商
- 产品居中或略偏中心
- 防止边缘裁切

## 3:4 / 4:5 竖图
- 适合墙灯、落地灯、室内生活方式图

## 16:9 / 2.44:1 横图
- 适合官网 Hero、横幅、文章配图

---

# 13. 批量多场景规则
当用户要求 5、10、20 张图片时，不允许仅更换家具颜色。

必须在以下维度建立真实差异：
- 房间类型
- 建筑风格
- 墙面材质
- 地面材质
- 家具组合
- 主光方向
- 时间段
- 镜头高度
- 摄影角度
- 产品在画面占比
- 构图方式
- 背景层次

但 Product Lock 始终不变。

批量逻辑详见 `modules/12_batch_series.md`。

---

# 14. 参考图编辑模式
如果用户要求“只改变背景 / 色温 / 删除文字 / 添加遥控器 / 替换灯具”：

- 明确锁定“所有未要求改变的元素”
- 使用最小改动原则
- 不重绘无关区域
- 保持原构图、原尺寸、原透视、原背景（除非明确要求改变）
- 替换产品时，确保新产品尺寸、透视、接触阴影与场景一致
- 添加附件时，附件必须来自用户参考图或明确描述，不自行设计

详见 `modules/13_reference_editing.md`。

---

# 15. 默认负面约束
每次至少加入以下语义：

blurry, low resolution, soft focus, smeared textures, melted details, fuzzy edges, distorted geometry, deformed fixture, changed product design, wrong number of arms, wrong number of shades, incorrect mounting, floating fixture, inconsistent perspective, excessive bloom, clipped highlights, muddy shadows, plastic-looking materials, fake reflections, painterly texture, noisy image, duplicate parts, extra accessories, random text, watermark, malformed furniture, warped walls, bent straight lines

详细库见 `modules/10_negative_prompt_library.md`。

---

# 16. 最终 Prompt 编译顺序
最终生成提示词按此顺序组织，不要杂乱堆词：

1. Product identity / Product Lock
2. Installation
3. Scene and architectural context
4. Product scale and position
5. Light state and color temperature
6. Lighting design
7. Camera and composition
8. Material rendering
9. Resolution and detail preservation
10. Negative constraints

模板见 `modules/16_prompt_compiler.md`。

---

# 17. 质量验收
生成后从以下 8 项检查：

1. 产品结构准确度
2. 安装合理性
3. 尺寸/比例可信度
4. 材质真实性
5. 光学真实性
6. 边缘与微细节
7. 构图与商业感
8. 场景洁净度

任一核心项明显失败，应优先修复，而不是继续美化。

使用 `checklists/final_qc_100_points.md` 进行完整检查。
