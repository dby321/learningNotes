# 2022Study-cmi项目学习

> 参考ERP-新员工培训视频

## 预算管理 模块

### Opex&Mpax立项申请

> 类似于`Cpax立项申请`
>
> **归位一类**：`Opex&Mpax立项申请`，`Opex&Mpax项目查询`，`Opex&Mpax年度预算调整`，`Mpax项目预算调整`
>
> **主要功能**：即可以用Opex做立项申请，也可以用Mpax做立项申请

### Opex预算维护

> **归为一类**：`Opex预算维护`，`Opex预算执行查询`，`市场营销预算执行查询`
>
> **主要功能**：模板下载、导入与导出（`立项管理`-`立项申请`中添加的`预算信息-opex`预算行来自于`Opex预算维护`的导入）



> **Opex项目预算模板**部分字段讲解：
>
> - 年度
> - 成本中心编码（部门成本中心）
> - 预算科目（预算的种类）
> - 预算科目金额
> - 预算项目编码（不是市场营销预算时，会系统自动生成[年份-成本中心-预算科目]）
>
> **市场营销预算是特殊的Opex项目预算**（预算科目是SM05和SM08时，为市场营销预算）

### Opex&Mpax项目查询

### Opex预算执行查询

### Opex&Mpax年度预算调整

> **主要功能**：Opex预算调整和Mpax年度预算调整
>
> - 年度预算：是通过`Opex预算维护`导入的，没有带`P`和`O`的项目编码
> - 项目预算：有带`P`和`O`的项目编码

### Opex预算控制策略

> **主要功能**：按预算科目进行控制策略
>
> **结果**部分字段讲解：
>
> - 控制策略
>   - 刚性控制（不能超预算）
>   - 弹性控制（可以超预算一定比例）
>   - 不控制（不控制预算金额）
> - 项目化管控
>   - 是（可以在`立项管理`-`立项申请`中`预算信息-Opex`中选择）
>   - 否（不能在`立项管理`-`立项申请`中`预算信息-Opex`中选择，可以在采购和合同中选到）

### Mpax项目预算调整

### 市场营销预算执行查询

