> 让 AI 科研数据提取人人可用

## 📖 为什么做这个项目

文献数据的结构化提取是材料科学研究中"耗时且低创造性"的环节。同一篇论文，不同人提取出的表格千差万别：漏读图内文字、混入外部知识、单位不统一、表格与正文数值冲突不知取舍……这些问题会直接污染后续的汇总分析。

在科研圈，数据提取的 prompt 工程正在成为一种"隐性资源"——成熟的研究组有自己打磨好的提取模板，而大多数人还在从零调试。我们不想看到这种不平等继续。

这个仓库收集**经过实战打磨的数据提取 Prompt**，把"从论文到结构化表格"的隐性经验显性化：不重不漏、字段可追溯、单位统一、冲突有章法。

## 🎯 我们做了什么

- **📝 数据提取 Prompt 库**：面向具体材料/测试场景的论文数据提取 Prompt，复制即用
- **🧾 字段级可追溯**：每个数据字段都必须能回溯到正文/图/表/SI 的具体位置
- **🗂️ 表格化输出规范**：固定表头 + 统一单位 + 多条件分行，多篇文章数据可直接纵向汇总建库
- **🔍 内置自检清单**：每个 Prompt 末尾附输出前自检项，让模型自行校验"不重不漏、不编造"

## ✨ 特点

- 🔬 **实战打磨**：来自一线科研的真实提取场景
- 🚀 **开箱即用**：完整复制 Prompt，配合论文全文使用，无需二次调试
- 🧭 **可追溯**：所有字段标注来源，杜绝编造与遗漏
- 🤝 **持续更新**：不断收集新材料体系与测试场景的提取规范

**不要在数据提取上浪费时间，把精力留给真正的科研。**

---

## 📑 目录 (Table of Contents)

### Part I: 数据提取 Prompt 集合
- [CDI 脱盐 COF 电极数据提取](#cdi-脱盐-cof-电极数据提取)

---

# Part I: 数据提取 Prompt 集合

> 💡 **使用说明**：以下 Prompt 可直接完整复制到聊天框中与大模型交互使用。请务必**连同论文正文、所有图片、所有表格、Supporting Information（SI）一起提供**给模型——Prompt 的设计前提是"只依据给定材料提取"，不给全文就无法保证可追溯性。每个 Prompt 都经过精心设计，请完整复制使用以获得最佳效果。

## CDI 脱盐 COF 电极数据提取

> 📌 **适用场景**：从 CDI（电容去离子）脱盐研究论文中提取实际作为 CDI 电极的 COF 材料的制备、配方、测试条件与性能数据，输出 6 张可直接汇总的标准化表格。适用于单篇精提取与多篇文献数据建库。

````markdown
你是一名材料科学CDI脱盐领域的数据提取助手。请只依据我提供的论文正文、所有图片、所有表格和Supporting Information（SI）提取信息，不要使用外部知识，不要猜测或编造，不要引入非本文报道的材料。我的最终目的是把多篇文章的数据汇总成表格，因此你必须做到：不重不漏，覆盖正文、图、表格、SI四类来源，并且只提取本文实际作为CDI电极使用的COF材料。其他辅助材料，包括对电极、导电剂、粘结剂、基底、电解液组分、仅引用文献材料、背景介绍材料、文献对比材料等，不单独成行；若它们与某个CDI电极COF材料相关，只写入对应表格的字段或notes中。

【严格限定：只提取本文实际做CDI电极的COF材料】
1. 只把本文实际制备、使用、测试，并作为CDI电极的COF材料列入表格。
2. 非CDI电极的COF材料、仅对电极、仅基底、仅导电剂、仅粘结剂、仅电解液、仅引用文献材料、仅背景介绍材料、仅文献对比材料，不得单独成行。
3. 若同一COF材料有不同比例、不同处理条件、碳化/未碳化、复合/未复合、不同CDI条件、不同电压、不同放电电压、不同电流密度、不同流速、不同浓度、不同循环，必须每个条件单独一行，不得合并。
4. 不得根据领域常识默认材料为AC、活性炭、PVDF、Super P、碳布、石墨烯、CNT、MOF、MXene等；原文未明确写出的，写“未报道/未找到”。
5. 所有字段必须能追溯到本文正文、图、表或SI的具体位置。若本文没有，写“未报道/未找到”，不得留空，不得编造。
6. 化学结构字段只写原文明确给出或图中可确认的信息；不得用外部知识补全linkage、元素含量、比表面积等。

【数值优先级与冲突处理】
1. 当论文表格（Table）和正文数值不完全一样时，以正文为准；不要采用与正文冲突的表格值，除非正文完全没有给出而表格给出。
2. 若表格值与正文值冲突，必须在 notes 中写“表格值X与正文值Y冲突，采用正文值Y”，并在 data_source 中同时标注正文位置和表格位置。
3. 若同一材料同时有CV和GCD比电容数据，以GCD为准；specific_capacitances_F_g-1 填GCD值，并在 notes 注明“采用GCD，CV值为...”。若只有CV，则填CV值并注明“来自CV，非GCD”。
4. 若表格给CV比电容、正文给GCD比电容，以正文GCD为准。
5. 其他CDI/电化学指标若表格与正文冲突，也以正文为准，并在 notes 记录冲突。

【检查来源】
必须逐项检查：
1. 正文：摘要、实验方法、结果与讨论、结论、图注、表注；
2. 图：所有主图、SI图、图内文字、图例、坐标轴、箭头、嵌入表格、示意图；
3. 表：所有主表、SI表、表注；
4. SI：SI正文、SI图、SI表、SI图注、补充实验方法。
不得只看正文，不得只看表格，不得遗漏图中文字和SI。

【输出格式】
必须输出Markdown表格。为了确保在聊天界面中能够上下左右滑动且不折行，必须将整个Markdown表格包裹在HTML横向滚动容器中，即严格使用以下结构输出：

<div style="overflow-x: auto; white-space: nowrap;">

| 表头1 | 表头2 | ... |
| --- | --- | --- |
| 数据1 | 数据2 | ... |

</div>

严禁在Markdown表格的单元格内使用任何换行符或回车键。如果模型平台不支持渲染HTML，请将大表格按逻辑拆分为多个不超过10列的Markdown子表，纵向排列，以确保排版清晰、不折叠、不堆叠、可上下左右滑动。没有数据的字段写“未报道/未找到”。

不要输出材料总清单，不要输出逐条列表，不要输出分析建议，不要输出前后缀。必须按以下顺序输出6个独立的Markdown表格，每个表格第一列必须是 `sample_name`，确保数据能一一对应。

### 表1：材料与基底
<div style="overflow-x: auto; white-space: nowrap;">

| sample_name | counter_electrode | linkage_beta-ketoenamine | linkage_imine | linkage_pyrazine | linkage_pyridinium | is_carbonized | carbonization_temp_C | substrate_present | substrate_graphene_go | substrate_cnt | substrate_mof | substrate_mxene | substrate_loading_value | substrate_loading_unit | substrate_loading_basis |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

</div>

### 表2：电极配方
<div style="overflow-x: auto; white-space: nowrap;">

| sample_name | physical_additive_wt_percent | physical_additive_type_CB | physical_additive_type_Super P | physical_additive_type_none | binder_PVDF_wt_percent |
| --- | --- | --- | --- | --- | --- |

</div>

### 表3：CDI 测试条件
<div style="overflow-x: auto; white-space: nowrap;">

| sample_name | device_architecture | feed_concentration_mg_L | voltage_V | discharge_voltage_V | current_density_mA_g | charging_mode_CC | charging_mode_CV | flow_rate_mL_min |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

</div>

### 表4：CDI 性能与数据来源
<div style="overflow-x: auto; white-space: nowrap;">

| sample_name | target_SAC_mg_g | target_IAC_Na_mg_g | measurement_type | target_unit | data_quality | IAC_data_quality | is_Cl_measurement | data_source | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

</div>

### 表5：物理化学表征
<div style="overflow-x: auto; white-space: nowrap;">

| sample_name | SSA_m2_g | SSA_method | Vpore_cm3_g | Vmicro_cm3_g | element_content | pore_size_range_nm | pore_size_peak_nm |
| --- | --- | --- | --- | --- | --- | --- | --- |

</div>

### 表6：GCD 测试与电化学性能
<div style="overflow-x: auto; white-space: nowrap;">

| sample_name | GCD_NaCl_concentration_M | GCD_current_density | GCD_voltage_window | specific_capacitances_F_g-1 | specific_flow_Ag | charge_transfer_resistance |
| --- | --- | --- | --- | --- | --- | --- |

</div>

【字段填写规则】
- sample_name：写原文材料/样品名称，包括比例、处理条件、碳化条件、复合条件等；同一材料不同CDI条件需在sample_name或notes中区分。必须出现在每个表格中。
- counter_electrode：只写原文明确给出的对电极/阴离子电极材料；保留原文术语，如阴极、阳极、对电极、阴离子电极；未报道写“未报道/未找到”。
- linkage_beta-ketoenamine、linkage_imine、linkage_pyrazine、linkage_pyridinium：分别写“是/否/未报道”，只依据原文结构描述。
- is_carbonized：是/否/未报道；carbonization_temp_C：数值+°C，或未报道/未找到。
- substrate_present：是/否/未报道；substrate_graphene_go、substrate_cnt、substrate_mof、substrate_mxene：分别写是/否/未报道；substrate_loading_value、substrate_loading_unit：数值+单位，或未报道/未找到；substrate_loading_basis：只能从以下类别中选择其一——synthesis_input（合成投料量）、material_composition（最终材料组成比例）、electrode_loading（电极上负载量），或未报道/未找到。
- physical_additive_wt_percent：数值+wt%，或未报道/未找到；physical_additive_type_CB、physical_additive_type_Super P、physical_additive_type_none：分别写是/否/未报道；若为其他导电剂，在notes写类型。
- binder_PVDF_wt_percent：数值+wt%，或未报道/未找到。
- device_architecture：照原文写HCDI、CDI、MCDI或其他；多个条件分别行。
- feed_concentration_mg_L：优先写原文NaCl浓度；若原文为mg/L，写数值+mg/L；若原文为M，写数值+M并在notes注明“原文单位为M，未擅自换算”。若表格与正文冲突，以正文为准。
- voltage_V：写CDI脱盐充电电压/施加电压；数值+V；若原文为电压窗口、截止电压、恒压值，照原文并注明。若表格与正文冲突，以正文为准。
- discharge_voltage_V：写CDI脱盐放电电压/放电截止电压/反向电压/短路放电等；数值+V；照原文并注明。若表格与正文冲突，以正文为准。
- current_density_mA_g：优先写电流密度；数值+mA/g；若原文只给绝对电流mA或A，在notes中写绝对电流数值+单位，并注明不是电流密度；不得擅自换算。若表格与正文冲突，以正文为准。
- charging_mode_CC、charging_mode_CV：分别写是/否/未报道。
- flow_rate_mL_min：数值+mL/min；原文若为mL min−1，统一写mL/min；多个条件分别行。若表格与正文冲突，以正文为准。
- target_SAC_mg_g：盐吸附容量，数值+mg/g；多个条件分别行。若表格与正文冲突，以正文为准。
- target_IAC_Na_mg_g：钠离子吸附容量，数值+mg/g；多个条件分别行。若表格与正文冲突，以正文为准。
- measurement_type：写SAC、IAC、脱盐容量、吸附容量等原文术语。
- target_unit：写原文单位，如mg/g。
- data_quality、IAC_data_quality：写原始数据、估读、未报道等；图中估读必须标“估读”。
- is_Cl_measurement：是/否/未报道。
- data_source：写正文第X段/图X/表X/SI图X/SI表X/图注/表注；图中数据必须标明图号和是否估读。若表格与正文冲突，同时标注正文和表格来源。
- notes：写缺失、冲突、单位换算、原文模糊、无法确认是否COF电极、绝对电流、原文单位、表格与正文冲突及采用正文、CV/GCD选择等。
- SSA_m2_g：数值+m2/g，或未报道/未找到；SSA_method：写BET、DFT或其他原文方法。
- Vpore_cm3_g、Vmicro_cm3_g：数值+cm3/g，或未报道/未找到。
- element_content：写原文明确给出的元素含量，如C、N、O等，带单位（如at%或wt%），或未报道/未找到。
- pore_size_range_nm、pore_size_peak_nm：数值+nm，或未报道/未找到。
- GCD_NaCl_concentration_M：GCD测试所用NaCl浓度，数值+M，或未报道/未找到。若表格与正文冲突，以正文为准。
- GCD_current_density：GCD测试电流密度，数值+mA/g，或未报道/未找到。若表格与正文冲突，以正文为准。
- GCD_voltage_window：GCD电压窗口，数值+V，或未报道/未找到。若表格与正文冲突，以正文为准。
- specific_capacitances_F_g-1：数值+F/g，或未报道/未找到；若同一材料同时有CV和GCD数据，以GCD为准，填GCD值并注明“采用GCD，CV值为...”；若只有CV，则填CV值并注明“来自CV，非GCD”。若表格与正文冲突，以正文为准。
- specific_flow_Ag：数值+A/g，或未报道/未找到。
- charge_transfer_resistance：数值+Ω，或未报道/未找到；若原文为kΩ，换算为Ω并注明原文单位；若仅从Nyquist图估读，标注“估读”。

【自检要求】
输出前必须检查：
- 是否只依据本文正文、所有图、所有表、SI？
- 是否只列本文实际作为CDI电极的COF材料，未把辅助材料、对电极、基底、导电剂、粘结剂、引用文献材料单独成行？
- 是否覆盖正文、所有图、所有表、SI？
- 是否每个CDI电极COF材料/条件都有一行，并且跨6个表通过sample_name能对应上？
- 是否每个字段都有“数值+单位+来源”或“未报道/未找到”？
- 是否把同一COF材料的不同CDI条件分开列出？
- 是否把图中数据标成图号，估读标成“估读”？
- 是否没有编造、没有遗漏、没有合并不同样品？
- 是否明确提取了CDI脱盐测试中的充电电压、放电电压、电流大小/电流密度、NaCl流速mL/min？
- 是否所有单位统一为：mL/min、m2/g、cm3/g、F/g、Ω；电压用V，电流密度用mA/g，绝对电流按原文mA或A并注明？
- 是否严格按照6个指定表头和字段顺序输出，且未包含paper_id和material_id？
- 是否将每个Markdown表格包裹在HTML横向滚动容器中，或若HTML不支持则拆分为不超过10列的子表，且未在单元格内使用换行符？
- 是否在论文表格（Table）和正文数值不一致时采用正文，并在notes记录冲突？
- 是否比电容优先采用GCD；若仅CV则注明“来自CV，非GCD”？
- 是否只输出表格，没有额外文字？
````

---

## 🤝 如何新增 Prompt

仓库采用「目录 + 分节」的扁平结构，新增一个数据提取 Prompt 只需三步：

1. 复制 [CONTRIBUTING.md](./CONTRIBUTING.md) 中的 Prompt 模板；
2. 在 README 的「目录」和对应 Part 下新增一个 `##` 小节（适用场景说明 + 代码块）；
3. 提交 PR，说明该 Prompt 的目标领域与已验证的论文场景。

## 📄 License

[MIT](./LICENSE)
