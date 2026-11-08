# v11 购物整合与维护定位

2026-11-07，接续已通过双机验收的 v10，整合独立购物清单。发布站点沿用 home-recipe-offline。

## 使用流程

主页面「安排菜单」安排日期、餐次、菜谱和人数。「当天材料用量」展示按比例汇总的材料，点「预览加入购物待购池」进入购物模块，确认后选店、加入行程和勾选。购物页可以返回菜谱与菜单，主页面顶部也可直接打开购物清单。

计划人数的小数向上取整。菜谱基础份数仍按原值作为分母，材料保留小数和原单位。不同单位保持独立。旧 v10 已勾选项在材料汇总中显示完成提示，转入购物时跳过，旧备份中的菜单和勾选仍可读。

菜谱资料和购物资料分别备份。购物状态在购物模块中保存，菜谱材料汇总显示计算结果与旧勾选记录。已送入的需求保留当时菜单快照，更改菜单后重新发送需预览确认。

## 源码和发布

| 工作内容 | 本地位置 |
| --- | --- |
| 菜谱与菜单界面 | app/index.html、app/app.js |
| 人数取整与用量汇总 | app/planner.js |
| 菜谱转购物协议 | app/shopping-adapter.js |
| 独立购物源码 | 工作区 shopping-list/ |
| 购物界面运行副本 | app/shopping/，由准备脚本生成 |
| 菜谱 IndexedDB | app/storage.js，原数据库和 schemaVersion 2 保留 |
| 购物 IndexedDB | shopping-list/src/storage/indexed-db.js，版本 1 |
| 菜谱备份和合并 | app/backup.js、app/merge.js |
| 离线缓存 | app/sw.js、app/offline.js，缓存标识 v11 |
| 复制购物模块 | tools/prepare-shopping.mjs |
| 运行文件清单 | tools/runtime-files.json、app/shopping-assets.json |
| 打包 | tools/package-site.py 或 tools/package-site.mjs |
| 整合回归 | tests/shopping-adapter.test.mjs、tests/shopping-browser.js |
| 升级回归 | tests/shopping-upgrade.js |
| 浏览器运行器 | 工作区 shopping-list/tests/run-chrome.mjs |

主程序打包只包含白名单运行文件、PDF 阅读器资源和购物模块。Drive 读取模块保留本地开发状态，继续按其原有单向更新边界接续。

本机发布检出为工作区「购物整合发布/」，从已验收 v10 正式仓库浅克隆。它与旧「发布仓库/」检出区分使用。开发源码仍在本交接包 app/，发布时只更新审查过的 site.zip 与维护说明。

## 测试结果

- 购物领域单元测试 6 项通过，菜谱适配器单元测试 3 项通过。
- 桌面和移动视口通过独立购物事务、重复导入、完整恢复、恢复前快照和旧窗口写入保护。
- 桌面和移动视口通过菜单转入、人数取整、来源追踪、购物勾选刷新、未购回退、历史和重复菜单提示。
- 真实网络禁用后，通过菜谱冷启动、菜单保留、购物预览与历史以及离线 PDF 绘制。
- 正式 v10 包升级到 v11，旧备注、菜单和勾选保留，旧备份可解码并转入未购材料，更新等待期间未保存输入保留。
- 本轮真机整合结果待用户验收，v10 与独立购物第一阶段的双机通过结果作为已完成基线保存。

本轮截图在工作区 shopping-list/review-screenshots/v11/。线上验证及提交号另记录在续接入口。
